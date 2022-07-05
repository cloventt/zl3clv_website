+++
author = "David Palmer"
title = "Running OpenWebRx with DMR in Docker behind an Nginx reverse proxy"
date = "2022-02-04"
description = "How I got OpenWebRx running on my home server."
tags = [
    "radio",
    "sdr",
    "rtl-sdr",
    "openwebrx",
    "dmr",
]
+++

I finally received my [RTL-SDR](https://www.rtl-sdr.com/about-rtl-sdr/) in the mail at the start of the year, and I've been tinkering with it off-and-on whenever I get some time to myself on my computer. One of the issues I've run into is that there doesn't seem to be an absolute "killer app" for SDR on Linux, with all of the top options on the [RTL-SDR blog](https://www.rtl-sdr.com/big-list-rtl-sdr-supported-software/) being Windows-only. So the hunt was on for something I could use.

{{< figure src="/images/2022-02-04-openwebrx/openwebrx-dmr.png" caption="Listening to DMR channels using OpenWebRx." alt="openwebrx DMR">}}

## The Rest

I've tried out [SDR++](https://github.com/AlexandreRouma/SDRPlusPlus) but ran into a decent variety of issues. First was that there aren't pre-built binaries for Fedora, which is my current daily driver distro. No worries, I can compile it from source, but already that makes the technical skill required to install it well out of reach of most home users. Once I had it compiled and running I ran into a weird issue[^1] where the audio sink would throw a hissy fit and have a meltdown, I suspect because Fedora uses Pipewire. Even when it did work, there is still no support for Hi-DPI displays in SDR++[^2] so the UX was crunched down into ant-sized fonts on my machine. So I gave up on that as an option, despite it being top of the list on the RTL-SDR blog for Linux.

Next I tried [GQRX](https://gqrx.dk/). While it worked _much_ better than SDR++ in almost every way, I still found the UI pretty clunky to use. If you do pick GQRX for your RTL-SDR, make sure you check the `Hardware ACG` option in the receiver config; without this enabled it was incredibly fiddly to get any good signals out of the noise.

## The Best

So, finally, I decided to try [OpenWebRx](https://www.openwebrx.de/). Not only is it an incredibly robustly-featured SDR, it also uses a web UI that works even on a smartphone. The ability to tune around the bands from the other end of my house - or even when I'm not at home - makes it head-and-shoulders better than the others I tried. The project seems to be incredibly well looked after by current maintiner Jakob Ketterl, and it was a breath of fresh air to find a detailed wiki explaining how to install the programme in a variety of ways. And I have to say as software engineer that shipping a pre-built Docker image for your project is such a no-brainer these days.

## Installation

The first step is to get OpenWebRx running somehow. I use Docker for everything on my home server, so I already have `docker-compose` installed and orchestrating all the services I run in my house. The Github wiki for the project has a [fantastic guide](https://github.com/jketterl/openwebrx/wiki/Getting-Started-using-Docker) on getting up-and-running in Docker, so it was a piece of cake to adapt that into my existing docker-compose config.

Here is the docker-compose entry I used to get OpenWebRx running:

```yaml
version: "2"
services:
  openwebrx:
    container_name: openwebrx
    # use the full-fat image to get all the features
    image: jketterl/openwebrx-full:stable
    ports:
        # expose the web-UI port
      - "8073:8073"                            
    volumes:
        # mount in the config dir for stability between restarts
      - .config/openwebrx:/var/lib/openwebrx
    devices:
        # mount in the USB bus so that OpenWebRx can find the dongle
      - /dev/bus/usb:/dev/bus/usb
    tmpfs:
        # recommended if you're using an SSD
      - /tmp/openwebrx
```

Then simply running `docker-compose up -d` starts the service.

{{< notice tip >}}
You may need to blacklist the `dvb_usb_rtl28xxu` kernel module on your host machine for this to work. If you don't blacklist that module, the host machine will detect the RTL-SDR dongle and "claim" it before the OpenWebRx container gets a chance to use it. In my case I was able to use `modprobe -r dvb_usb_rtl28xxu` to unload the module, and then I additionally blacklisted it following the [guide on the Arch wiki page](https://wiki.archlinux.org/title/Kernel_module#Blacklisting). 
{{< /notice >}}

## Getting DMR Working

OpenWebRx used-to support DMR, but it was using the legally-questionable `mbelib` to do so. As a result the maintainer removed it at the request of his employers[^3]. However, he also took the chance to refactor some of the signal processing backend into a separate service called `codecserver`. From what I can understand, `codecserver` acts as a decoder service, turning your digital radio signals into audio. Without `mbelib` inside `codecserver`, OpenWebRx cannot decode DMR and some other digital voice modes. The only (completely legal) fix is to buy a fairly expensive USB dongle that acts as a "black-box" decoder for these. I'm pretty reluctant to drop so much money on a tiny USB stick, or support proprietary codecs in general, so I looked for ways to get `codecserver` working with `mbelib` again.

Luckily for me, someone else has already done the leg work for me. If you're willing to adopt the legal risks involved in doing so, there is a [Github repo](https://github.com/fventuri/codecserver-mbelib-module) available containing code that can be used to compile a `codecserver` module containing `mbelib`, so that digital voice can once-again be decoded by OpenWebRx. After a bit of finagling I was able to compile this and get it running. Given that I'm running OpenWebRx inside Docker it made sense to do the same for my `codecserver` instance. If you're interested in doing the same I've posted my [Dockerfile](https://github.com/cloventt/codecserver-mbelib-docker) up onto Github. This Dockerfile will build an image containing `codecserver` with the `mbelib` module installed and working. Then all you need to do is change your OpenWebRx to connect to the separate `codecserver` and DMR should magically start working next time you restart OpenWebRx.

## Reverse Proxy

Once I had OpenWebRx running, I wanted to allow it through my Nginx reverse proxy. Luckily this was also the easiest thing in the world. Al that is required is to add something similar to this to your Nginx config:

```conf
location /openwebrx/ {
    proxy_pass    http://localhost:8073/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
}
```

The additional config there are to ensure that websockets continue working, which is a protocol that OpenWebRx relies upon. I've verified that this works well on desktop browsers and on my iPhone.

## Things to Do
I've already been busy tuning into my local amateur repeaters, both on simplex and digital modes. I've also spent some time clicking around the bands and getting a feel for what's out there between the static. As an example, I've found APRS beacons sending out GPS positions (mostly for private cars, but also the odd bus). I was amazed to find I could pick up these signals from as far as Timaru with no issues. I also stumbled onto the emergency services DMR channels, which are all heavily encrypted and unreadable these days. Of course there's air band, with all the regular comings-and-goings from my local airport, and really the list just goes on and on. 

{{< figure src="/images/2022-02-04-openwebrx/openwebrx.png" caption="Decoding APRS location beacon packets using OpenWebRx." alt="openwebrx UI">}}

## What's next?
Once I've collected a reasonable list of confirmed and reliable bookmarks, I'll collate them and publish them either here or on Github for other local OpenWebRx users to import. Most interesting to me is some of the private un-encrypted DMR channels, mostly used by worksites and logistics companies. The easy part is seeing them on the waterfall - the harder part is figuring out who they are.

I'll share my SDR profiles somewhere as well, once I've finished tidying them up. The RTL-SDR has a fairly narrow bandwidth, forcing you to select between a lot of bands more frequently than other higher-end SDRs. While this is a major drawback, it is really offset by the super low cost of the device.

Now that I can hear some amateur SSB beacons, I'm planning to begin practicing my morse code. I've already started - but been side-tracked by work and studying for my amateur radio certificate. Besides that, I'm also curious to learn how to recognise different types of transmissions just by looking at their visual profile on the waterfall.

***

[^1]: I opened a [bug report](https://github.com/AlexandreRouma/SDRPlusPlus/issues/618) for the issue but the maintainer immediately closed it, referring me to a different project as the cause. Such is life in open source software.

[^2]: You can check here whether this issue has been resolved at time of reading: https://github.com/AlexandreRouma/SDRPlusPlus/issues/53

[^3]: He explains the decision in a [post](https://groups.io/g/openwebrx/message/3487) on his mailing list. It's cool that his employer lets him work on OSS full-time, but it does suck that it came at this high cost. Radio should be accessible to all; these proprietary codecs are a blight on the airwaves.
+++
author = "David Palmer"
title = "I got my license! + Baofeng UV82 reviewed"
date = "2022-02-04"
description = "Quick catchup post on everything that's been going on in my hobby lately."
tags = [
    "radio",
    "sdr",
    "baofeng",
    "hardware",
    "review",
]
+++

So, the big news is I got my [General Amateur Operator's Certificate of Competency](https://www.nzart.org.nz/learn/exam/)!

I attended the May 2022 HamCram event at the Chirstchurch branch of NZART, hosted by Mike Duncan ZL3MWD at the Christchurch clubrooms. The two-day session was going to happen earlier, but as with all things COVID-19 got in the way of it. I was already pretty well prepared for the exam, but it was a lot of fun to hang out and go over the material in a more directed way. I really recommend attending the course; with the additional study I was able to scrape a perfect 60/60 on the exam. A follow-up "HamCram Plus" session is scheduled to happen soon as well, though that's also been delayed due to covid. So I'm now officially licensed, I've chosen ZL3CLV as my callsign, and I'm looking forward to getting more involved with the club and stepping out onto the air.

# First Radio: Baofeng UV-82
## Finding a store
So, with my callsign sorted the first step is to get myself a radio. I'd been planning to build out a shack at home beginning with a quad-band TYT-9800 mobile station and a home-made antenna, but in the end I decided to take a smaller first step and go for a Baofeng. I'd been planning to avoid buying a low-end cheap radio, but for a starter option there really isn't anything better.

The options for purchasing one in New Zealand are fairly limited though. Of course, I can always import one from overseas, with many sellers having them available on [AliExpress](https://www.aliexpress.com/wholesale?catId=0&initiative_id=SB_20220519152036&SearchText=baofeng&spm=a2g0o.home.1000002.0). That would come at a cost though; I'd be waiting several weeks for it to arrive, and I'd have no protection under the [New Zealand Consumer Guarantees Act](https://www.consumerprotection.govt.nz/general-help/consumer-laws/consumer-guarantees-act/). Digging around online didn't find me many good options for local sellers of Baofeng radios, with most of the local outfits selling either ICOM or Yaesu branded radios. I was planning to wait for the HamCram Plus event to ask some of the club members for advice, when I stumbled on an online store almost by accident when it's Trademe page was listed in the product carousel at the top of one of my many Google searches.

[Techoman Electronics](https://techoman.co.nz/) is an online store based right here in Christchurch, run by fellow ham Craig Sullivan ZL3TOO/ZL3TECH. Craig is licensed by RSM to supply unrestricted two-way radios in New Zealand and even offers free shipping on orders over $150. While the range was a little limited, he did have the venerable Baofeng UV-5R as well as the UV-82. Interestingly, the UV-82 on offer was the variant with 8 watt output, rather than the standard 5 watt output. While it wasn't sold as UV-82HP, I'm pretty sure this model is the same as the UV-82HP which I've seen be reviewed well online. It looks like I picked up the last of the stock because as of writing it is no longer listed for sale on the website. I took the chance to also pick up a Nagoya NA-771 antenna which I had also heard good things about. A few other little bits and bobs later (most importantly, a programming cable), and I was through checkout. Shipping was really fast, with the radio arriving the very next day.

Overall, I'm so far very happy with the radio, although I haven't used it for transmit yet (I'm leaving that until I've had some guidance from the old hands at the club). I've found it fairly easy to use and I've pretty quickly got my head around the slightly clunky menu. Luckily though, like most amateur radios it can be programmed from a PC, so I was able to get it quickly set up to fit my needs using CHIRP.

## CHIRP
Yeah, so, CHIRP is a less than impressive piece of software. According to the website, "CHIRP's preferred platform is Linux"[^1], and that can only be true if you're running a ten-year-old distro. I'm on Fedora Workstation 35 currently, and this software really does not want to run. The RPM recommended in the wiki doesn't work on my machine because binaries for Python 2 aren't available anymore. _That's right, this software is still written in Python2._ Annoyingly it also uses PyGTK and getting that to build these days seems basically impossible. Even with Python 2 installed via `pyenv` (yuck), running from source was a no-go. I tried installing a Snap and a flatpak, but in both cases they had permissions issues with connecting to the virtual serial port needed to communicate with the radio. As with all things, Docker came to my rescue.

Helpfully a [Github project](https://github.com/linuxluser/docker-chirp) exists with a small bash script to get chirp running reliably in Docker. I found the script actually didn't work for me directly[^2], but it really helped me bypass 99% of the work in getting this app to run in Docker. In the end, I adapted the script to the following (make sure you have the Dockerfile from Github as well):

```
#!/bin/sh

NAME="chirp"
# Allow docker to connect to current X session
xhost +local:docker


# Build
docker build -t "local/${NAME}" $(realpath $(dirname $0))


# Run
docker run --rm -i -t \
       --device=/dev/ttyUSB0:/dev/ttyUSB0"
       --device=/dev/dri:/dev/dri \
       --volume ${HOME}/.config/gqrx:/root/.config/gqrx \
       --volume /dev/shm:/dev/shm \
       --volume /tmp/.X11-unix:/tmp/.X11-unix:ro \
       --volume /run/user/$(id -u)/pulse:/run/pulse:ro \
       --volume /var/lib/dbus:/var/lib/dbus \
       --volume /dev/snd:/dev/snd \
       --volume ${HOME}:/data/ \
       --env USER_UID=$(id -u) \
       --env USER_GID=$(id -g) \
       --env DISPLAY=unix$DISPLAY \
       --name $NAME \
       local/${NAME} $@
```

With CHIRP running I was able to programme my radio exactly as intended. I added my local amateur repeaters as well as the NZ/Aus CB bands. Technically this radio is not legal to use for transmitting on the CB frequencies, but it might come in handy for listening to them in the future. I'm planning to create a Github repo and share my configs there once I've fleshed them out a bit more, so other hams in Christchurch can just quickly import the config and go.

## Next Steps
Once I've settled on a good config for my radio, I'll share it on my Github. After that, I'm planning to build a [VHF/UHF flowerpot antenna](https://vk2zoi.com/articles/dual-band-half-wave-flower-pot/) to go up in (or on) the roof. I've seen some builds for this on Youtube and it looks like a good option for improving my signal from home. The benefit of buying a Baofeng HT is I can swap out the Nagoya whip for a base station antenna on the fly. Later if I do pull the trigger on a dedicated home station transceiver, that fixed antenna will be ready to go. I'll post a write-up of my experiences when I build that antenna.

***

[^1]: https://chirp.danplanet.com/projects/chirp/wiki/Running_Under_Linux

[^2]: The issue came down to the helpful bit of code at the start to detect your USB serial adapter. I'd been provided with a Chinese clone adapter cable based on the CH340 serial interface (I have experience with this thing from my adventures in Arduino and those experiences were not good). This meant the hardware ID did not match what was in the script. Then, when I put my dongle's ID into the script, I got a cryptic error in CHIRP about it being unable to connect to the serial port due to some mysterious `ioctl` issue. In the end I was able to solve it by skipping all of this convenience code and just directly mounting in `/dev/ttyUSB0` which worked perfectly. I also threw in a mount to my home directory so I could load/save the configs from CHIRP onto my hard drive to be reused (just navigate to `/data/` in the CHIRP save dialog).
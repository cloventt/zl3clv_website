+++
author = "David Palmer"
title = "Dipole Antenna for Shortwave Listening"
date = "2022-01-06"
description = "A very vrief overview of a project I completed to build an HF receiving antenna."
tags = [
    "radio",
    "shortwave",
    "antenna"
]
+++

I'm pretty notorious for jumping around between hobbies. I'll normally stick to one for a few months and then drop it, only to circle back around to it a few years later. One hobby I've always been fascinated with but never actually got stuck into is amateur radio. At the moment I'm gearing up towards getting my General Amateur Operator’s Certificate of Competency so I can get on the air (I've had some pretty amazing help from the Christchurch branch of the NZART and I'm hoping to achieve that goal in the next few weeks).

In the meantime, I've been absorbing everything I can about how radio works. The beauty of radio is that you can learn an awful lot about it without needing a license, as long as you're happy just to receive. Last time I considered having a stab at this hobby I picked up a HanRongDa HRD-737 wide-band receiver, and I've had a lot of fun with it just tuning around the bands. I think it cost me about NZD$50 from [AliExpress](https://www.aliexpress.com/wholesale?SearchText=HanRongDa+HRD-737). You get what you pay for though; while it's fine for picking up broadcast music radio - as well as air-band and even VHF - it was absolutely hopeless at picking up shortwave signals. I'll include a video review from todderbert below in case you're interested in buying one (I recommend it if you just want to see what's out there but not for anything more serious). 

{{< youtube id="2DSfCNgzJ28" title="todderbert Review of the HanRongDa HRD-737 Wideband Receiver" >}}

I live in New Zealand, and sadly that means we're a long way from anyone that broadcasts on shortwave. If I got the antenna in _just_ the right place and squinted _just_ right the HRD-737 could sometimes pick up broadcasts from China, but they were never anything but a wash of noise and nowhere near intelligible. If I used the clip-on wire antenna that came with the receiver I could sometimes pick up a wee bit more, but nothing was particularly clear. So it seemed obvious I needed a larger antenna.

Generally the advice seems to be _"just use whatever random length of wire you can get"_. For me this was a bit of a challenge. My house is on the smaller side, and my property isn't huge either. Me entire property is only about 20m on it's longest side, and the house itself is a little over 12m on it's longest side. Add to that my wife's reluctance to let me hang visible wires all over the property or put up a pole and suddenly my options were getting pretty limited. But where there's a will there's a way, and where there's 12m of clear space up in my roof space there's a way to have an antenna.

After a bit of reading online I settled on a dipole antenna, mostly because it seemed like a fun project. A 12m run of wire will probably work well too but I learned much more about antennas by designing a dipole than I did from just putting up a long wire. A 12m antenna puts me somewhere in the middle of the popular SW bands, especially the ones that are most-used at night. As I only really get time to do my hobbies in the late evening, this seemed like a good choice.

Given that the antenna was going in the attic, I was able to get away with just using 18AWG speaker cable that I sourced from [Jaycar](https://www.jaycar.co.nz/heavy-duty-speaker-cable-30m-roll/p/WB1709). This probably wouldn't survive long outdoors but in the relatively static enviroment of my roof-space I think it will last a long time. To feed the antenna I decided to use 50Ω coax so that I could have an unbalanced but shielded feed-line. My thinking was that the shielded feed-line would help reduce RF interference from all the gadgets in the house, especially given that the antenna was going to be indoors. But this then presented a new challenge: how to connect unbalanced coax to a balanced dipole antenna?

The answer is of course a balun. Generally, the impedance of a dipole at the feed point is somewhere between 50Ω and 75Ω (depending on how high it is off the ground). My antenna is only going to be less than 5m off the ground, so it's probably closer that the 75Ω range. The signal loss from a 75Ω RX antenna to 50Ω coax is not something I'm going to be losing sleep over, and I'm happy that it's "good enough" that I don't need to change the impedance. That means all I really need is a 1:1 balun to connect the antenna to the coax, and I should be sweet.

Luckily for me, a 1:1 balun is pretty easy to make. There's an absolutely fantastic guide by VK6YSF that you can look at [here](https://vk6ysf.com/balun_guanella_current_1-1.htm). Basically the balun consists of paired wires wrapped around a toroid. Better yet, VK6YSF is in Australia, so I was able to directly buy all the exact parts listed in the guide directly from Jaycar. While my son was down for a nap on Christmas Day, I slipped out to the shed and managed to get everything assembled. I initially built one using green electrical tape as insulation, but as the guide called for using teflon thread-seal tape I decided to make another one that way. The one using teflon is what ultimately ended up in the roof. 

{{< notice tip >}}
I think the teflon only really matters if the balun is used for transmitting, as teflon is good at resisting high temperatures. These things can probably get a bit warm once a few watts are pumping through them. For receiving though it probably doesn't matter, but I figured I may as well use the one capable of transmitting.
{{< /notice >}}

{{< figure src="/images/IMG_5873.jpg" caption="Constructing baluns on Christmas Day. Notice the SO-239 connector at the bottom for interfacing with the coax. One of the wires is attached to the centre pin and the other is bonded to the SO-239's outer housing." alt="builing-the-baluns">}}

I waited for a cool day and dragged myself up a ladder into the roof. As I mentioned, my house is not that big, so the roof space can be a little cramped. Once I'd figured out how to get around up there, it only took me about half an hour to put the antenna up. I'd bought some nail-in cable clips ([these ones](https://www.jaycar.co.nz/cable-clips-7-10mm-expandable-pk-25/p/HP0694)) but they ended up being totally useless - the weight of the cable just pulled them straight out of our rimu top chords. Instead I used zip ties, wrapped around the frames. This ended up being better for another reason: I was able to separate the cable slightly from the frame of the house.

{{< figure src="/images/IMG_5913.jpg" caption="Why is it that dark, confined spaces always look scary when using flash photography?" alt="antenna-in0-the-roof">}}

Finally, I connected my length of coax to the balun and ran that down to the spare room. To connect it to the radio, I initially just used a wire with alligator clips at each end, attached to the antenna of the HRD-737. Suddenly the sensitivity seemed to jump right up. Success! I could hear a much wider range of stations across all bands, especially just after sunset. However, most of the stations were still very noisy with a lot of fading. Luckily I'd anticipated this and made another purchase in the meantime.

Enter the Tecsun PL-310ET shortwave radio. At under NZD$100, including shipping from Australia, it seemed like a worthy upgrade to the HRD-737. Importantly, it has a dedicated antenna plug, meaning it can be connected directly to the antenna without the need for alligator clips. I soldered together a harness from a female banana socket to the tip of a 3.5mm mono audio cable, and this was good enough to get the connection through to the radio. The PL-310ET is a much more capable radio, with a huge list of features compared to the HRD-737. As always toddebert has a great review of it where he covers the important things. 

{{< youtube id="sMZZG87iQL4" title="todderbert Review of the Tecsun PL-310ET shortwave radio" >}}

{{< notice warning >}}
I soon realised I'd made a mistake with my harness. The signal improved a massive amount when I connected the ring of the 3.5mm TRS connector to the sleeve of the PL-259 on the coax. As soon as that connection was made, I was suddenly able to clearly hear stations from all over Asia and America.
{{< /notice >}}

So far I've mostly picked up Chinese SW broadcasts in a variety of languages, but I've also received a few stations from other interesting places:

* Taiwan
* North Korea
* Japan
* Hawaii
* Guam
* North America (barely)

And of course, I've even managed to collect the venerable Radio New Zealand. This is one of the more interesting contacts: in theory I am inside the skip zone for the RNZ shortwave broadcast. But, I was able to pick it up very clearly for about 15 minutes during sunset.

The antenna is also really effective for standard broadcast AM and FM. I can pick up any station in Christchurch with no difficulty, and have even picked up FM stations from as far afield as Cheviot - a distance of 130km! 

A crucial resource in all of this is [short-wave.info](https://short-wave.info) which is a fantastic resource for determining what's on the air when you're listening. So far I've only really had time to listen during my evening, so next I'll be tuning around sometime during the day if I ever get time. I currently have an RTL-SDR in the mail, so I'm planning to hook that up to the antenna and make it accessible with OpenWebRX so that I can do SWL from my phone - even on my lunch break at the office. 

73
+++
author = "David Palmer"
title = "New Years Day 2023 - Field Report"
date = "2023-01-05"
description = "An in-depth report of my attempts to make contacts on VHF simplex with my Baofeng."
summary = "New Year’s Day is the busiest time of the year for SOTA activity, so I decided to stretch my legs and have a go at getting on the air. Just one problem: my only transceiver is a Baofeng HT. How did it hold up to the challenge of operating on simplex?"
tags = [
    "radio",
    "baofeng",
    "field",
    "report",
    "sota",
]
thumbnail = "images/2023-01-05-field-report-new-years-day/marleys-pano.jpg"
+++
New Year’s Day is the busiest time of the year for SOTA activity, so I decided to stretch my legs and have a go at getting on the air. Just one problem: my only transceiver is a Baofeng HT. How did it hold up to the challenge of operating on simplex?

## Attempt #1: POTA at South Brighton
“It’s such a nice day, we should go for a walk on the beach!” was all the convincing I had to do get my wife to join me for a trip to South Brighton beach. With my trusty Baofeng UV-82 and Nagoya 771 whip antenna, I chose my moment and slipped away from the family - climbing the highest dune I could find. Surely from up here - with a view out over the rooftops - my signal had a chance of getting out?

First indications were not good. I announced my intentions on the 705 repeater and switched over to 650 simplex. My CQ call was met with silence - nothing even breaking the squelch. Suspicious that I’d made some mistake, I opened my personal SDR at my QTH in Redwood from my phone. As I suspected, my Baofeng wasn’t even making it to my house. Back on the repeater, Geoff ZL3QR confirmed that I had only just broken the squelch at his place out in Rangiora (I assume his antenna and receiver are a damn-sight better than my RTL-SDR dongle with bunny-ears, so not surprising).

As we walked back to the car, I suddenly picked up Ian ZL3GIG up on Mt Pleasant. His signal was clear as day on simplex. Unfortunately, strapping a toddler back into the car makes it a bit difficult to transmit in reply, but it did give me an idea: maybe with some elevation my fortunes would change. Before long we were making a beeline for Mt Pleasant.

## Attempt #2: Mt Pleasant
My wife opted to wait in the car while I walked the last few hundred metres to the summit of Mt Pleasant. The various broadcast antennas at the top were the first worrying sign. I’d heard that these cheap Chinese radios really don’t cope with QRM very well, and the nearby presence of so much powerful RF was sure to cause some problems. And, indeed, it did.

Almost as soon as I was on the air I got a very clear response from Keith ZL3JA out in Kaiapoi. I did have to disable the squelch, as the very high noise floor was playing havoc with the radio’s ability to tell signal from noise. The first success was followed by another: Geoff ZL3QR came back to make the second contact. The third was Bill ZL3NB in north New Brighton, and he was the clearest of the three. He brought bittersweet news: he’d heard my POTA call earlier but had been otherwise occupied and unable to respond.

These contacts brought to my attention that James ZL3FV was trying to get through to me. He was walking around in Papanui on a portable, and could hear me pretty clearly. I could hear something through the noise, but it definitely wasn’t readable. My wife texted me to say the kids were getting bored in the car, so I had to cut my activation short of the necessary four contacts.

Keith ZL3JA came back to tell me about some filters he had designed to reduce QRM interference on Baofeng radios. I’ve been in touch with him since then and hopefully we can organise [another filter-building course](https://chchhamradio.org.nz/filter-project-2nd-round-31st-october-2021/) at the clubrooms in the coming months. In the meantime he’s sent me on a deep dive down the rabbit hole of RF filters.

On my way down from the top I did manage to quickly chat to James ZL3FV on the 705 repeater. He’d had good copy on me but I couldn’t say the same in return. His advice to improve performance on the Baofeng was to put it under a car tyre, drive over it repeatedly and then buy a better radio. In hindsight, probably very good advice.

## Attempt #3: Marley’s Hill
I had intended to return to Mt Pleasant a few days later, but in the intervening days my wife and I ended up deciding on a walk from Sign of the Kiwi to Marley’s Hill.[^1] On the way up I tried a CQ call on 146.500 from the south-side of Marley’s Hill, and made a contact with Ian ZL3GIG. In fact, I had a direct line of sight to his QTH in Lyttelton so the signal was very clear. When I mentioned my experience up on Mt Pleasant to him, he confirmed the QRM soup problems and mentioned that there are some spots up there that the handhelds can cope with (down by the flax on the road, and right up on the trig). I’ll have to test that advice on my next trip up there.

The top of Marley’s itself was a total noise washout. With Sugarloaf looming over us, and the Marley’s repeater right behind us, the Baofeng had no hope of picking up anything but growls, clicks, beeps and whirs from the wall of random RF overloading it’s frontend. The repeater CTCSS could still reliably break my squelch, but the only audio the radio put out sounded like a truck driving downhill with the engine brake on. I tested the repeater a few times on the way back down, and even down at Sign of the Kiwi the Baofeng just couldn’t cope with the noise.

## Next Steps
I learned a lot from this experience. As a new ham, the best thing you can do is just get out there and get on the air. Even if you have problems and don’t make a lot of contacts, you’re sure to learn a bunch. My next homebrew project will likely be some filters to cut out some of the QRM, hopefully with some help from Keith ZL3JA. And I think I’ll also hedge my bets by taking James ZL3FV’s advice (though I’ll maybe just buy a better radio and skip the automotive demolition).

***

[^1]: I recommend the walk; it’s a reasonably gentle grade, with beautiful views of both Christchurch and Governor’s Bay, and it only takes about 30 minutes each way (longer with a 2-year-old strapped to your back).
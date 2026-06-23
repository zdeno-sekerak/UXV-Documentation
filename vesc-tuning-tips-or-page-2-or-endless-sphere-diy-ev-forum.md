# VESC Tuning tips | Page 2 | Endless Sphere DIY EV Forum

**Thread starter:** [mxlemming](https://endless-sphere.com/sphere/members/mxlemming.73685/)\
**Start date:** [Sep 25, 2024](https://endless-sphere.com/sphere/threads/vesc-tuning-tips.125527/)

## Post #26 — dimos15

_Aug 21, 2025_

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1862867)
>
> ### [Home - VESC Labs](https://www.vesclabs.com/)
>
> VESC Labs - The official range of motor controllers, battery management systems and accessories powered by VESC Tool. Manufactured from the highest quality components, ensuring maximum power, reliability and functionality.
>
> Benjamin and Co have released their own lineup of controllers and auxilliary bits, probably worth giving a go as the new reference hardware for the VESC project!
>
> Click to expand...

Amazing news! And you have an amazing team for this project!\
Can you give me some more info on the vesc duet? On the website it says 22s compatible but on the spec sheet it says 84V(20s) recommended voltage? Are the mosfets 100V? I want to pair it with two 6384 motors

## Post #27 — mxlemming

_Aug 22, 2025_

> [dimos15 said:](https://endless-sphere.com/sphere/goto/post?id=1863028)
>
> Amazing news! And you have an amazing team for this project!\
> Can you give me some more info on the vesc duet? On the website it says 22s compatible but on the spec sheet it says 84V(20s) recommended voltage? Are the mosfets 100V? I want to pair it with two 6384 motors
>
> Click to expand...

100V MOS are typically stable with 20s under most conditions, 22s is OK if regeneration is limited, the battery cables are low impedance (short and fat...), the BMS is guaranteed not to suddenly cut out under regen and the design (device and system) doesn't intrinsically stress the FETs. This is pretty much applicable to all ESCs, VESC derivative or otherwise. For design into a serial product, I would personally suggest derating and not pushing the boundaries, but opinions vary...

You probably notice that while charging your battery the voltage on the terminals goes above 84V due to the internal resistance, same is true in regen.

There are people that have been running 22s for ages on 100V parts without issues, and others that have experienced a significant life reduction doing it.

_Last edited: Aug 23, 2025_

## Post #28 — amberwolf

_Aug 22, 2025_

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1863132)
>
> , 22s is OK if regeneration is limited, the battery cables are low impedance (short and fat...), the BMS is guaranteed to to suddenly cut out under regen and the design (device and system) doesn't intrinsically stress the FETs
>
> Click to expand...

I'm not quite sure if you're describing the problem or the solution?

If it's a solution, I'd like to know more, to see if it's something I could recommend to people that are likely to encounter the regen-vs-bms-shutdown issue.

(we get them every so often, especially those that need to start their whole journey from the top of a hill and want to use regen to slow their descent but also start with a full or near-full battery).

## Post #29 — scianiac

_Aug 22, 2025_

> [amberwolf said:](https://endless-sphere.com/sphere/goto/post?id=1863229)
>
> I'm not quite sure if you're describing the problem or the solution?
>
> If it's a solution, I'd like to know more, to see if it's something I could recommend to people that are likely to encounter the regen-vs-bms-shutdown issue.
>
> (we get them every so often, especially those that need to start their whole journey from the top of a hill and want to use regen to slow their descent but also start with a full or near-full battery).
>
> Click to expand...

It's not a solution for the starting a top of hill circumstance you describe, he's saying that is part of the reason doing so is a bad idea. He's listing some situations that can cause voltage levels to exceed the FET ratings, BMS cutout being one. I believe the same is also true for a BMS cutout when doing field weakening and with enough of that even staying in the recommended voltage range could still cause an issue.

## Post #30 — amberwolf

_Aug 22, 2025_

> [scianiac said:](https://endless-sphere.com/sphere/goto/post?id=1863236)
>
> It's not a solution for the starting a top of hill circumstance you describe, he's saying that is part of the reason doing so is a bad idea. He's listing some situations that can cause voltage levels to exceed the FET ratings, BMS cutout being one. I believe the same is also true for a BMS cutout when doing field weakening and with enough of that even staying in the recommended voltage range could still cause an issue.
>
> Click to expand...

Ah, then perhaps this line

> BMS is guaranteed to to

is supposed to say\
"BMS is guaranteed **NOT** to"\
instead?

FWIW, the only way I know (with a typical BMS) to guarantee that is to either not use a BMS, or to use one with separate ports so that the port supplying the controller cannot block the regen current _even if_ the BMS shuts down the pack for any reason...but then there is nothing to protect the pack against whatever happens in this instance.

## Post #31 — scianiac

_Aug 22, 2025_

Yes that is a typo, I believe that was his point, you don't want to go around ignoring the manufacturer's safety margins because a BMS cutout is possible in most cases.

## Post #32 — mxlemming

_Aug 23, 2025_

> [amberwolf said:](https://endless-sphere.com/sphere/goto/post?id=1863229)
>
> I'm not quite sure if you're describing the problem or the solution?
>
> If it's a solution, I'd like to know more, to see if it's something I could recommend to people that are likely to encounter the regen-vs-bms-shutdown issue.
>
> (we get them every so often, especially those that need to start their whole journey from the top of a hill and want to use regen to slow their descent but also start with a full or near-full battery).
>
> Click to expand...

For your people starting at the top of a hill and running regen, the solution is simple: just set the over voltage fault significantly below the DCDC/MOS/caps rating, and it will be OK unless you are running into field weakening on your hill

If you are field weakening, find the motor max speed WITHOUT the FW enabled, and set that as an rpm limit, or scale the max speed linearly with the voltage margin available... this forces the controller to stay at a safe speed.

At the end of the day, if you hang on to a car and get towed to 50mph over your base speed, your controller is going to die regardless. There comes a point when common sense and correct system specification is important.

Automotive can implement a number of solutions, including not shutting off the battery, pre-emptively rolling back power, forcing use of the main brakes, running at reduced power factor to waste energy into the motor... etc... but this takes substantial system design, analysis and testing, way beyond the willingness of the average builder.

For about 3 years now, VESC has behaved well with regards to over voltage protection, shutting down very rapidly in the event of an over voltage fault. Everything still needs analysing critically from a system design perspective with failure modes in mind, there is no one component that can solve your issue.

_Last edited: Aug 23, 2025_

## Post #33 — m0d01

_Sep 2, 2025_

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1824100)
>
> I cannot really recommend one against another. I only really work on my own hardware (which is not commercially available) and hardware I make for clients, which I am not going to divulge or credit one over another. I do not test common commercially available (aliexpress...) hardware.
>
> Edit, there is now an obvious choice: [Home - VESC Labs](https://www.vesclabs.com/) since these will obviously be best supported by the VESC project, and covers the power range from small robot to mid size motorbike/personal plane/launch winch/surfboard/whatever magical creation you have...
>
> Click to expand...

Love the Aug 2025 update to VESC labs. Currently have one in hand being installed into my scoot! Can’t wait!!

## Post #34 — TPEHAK

_Sep 8, 2025_

> [m0d01 said:](https://endless-sphere.com/sphere/goto/post?id=1864746)
>
> Love the Aug 2025 update to VESC labs. Currently have one in hand being installed into my scoot! Can’t wait!!
>
> Click to expand...

Please post the pictures and details.

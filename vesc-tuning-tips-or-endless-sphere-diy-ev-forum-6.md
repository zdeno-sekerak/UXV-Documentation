# VESC Tuning tips | Endless Sphere DIY EV Forum

## mxlemming · Sep 25, 2024

Copied from my original post on the flipsky 75100 thread... reposted in its own thread at the request of a few people. [FLIPSKY new 20s 100A tiny controller (vesc based)](https://endless-sphere.com/sphere/threads/flipsky-new-20s-100a-tiny-controller-vesc-based.113445/post-1823405)

I shall update occasionally, maybe...

I am not saying this is definitive, just to give you an insight into the way I tune and fiddle with VESC. I have based this on... let's say extreme fanaticism with discovering all the niches and oddities of controller behaviour and studying the Math of FOC and control loops. I am the author of one of the VESC sensorless observers and one of the HFI methods, as well as having generated more mundane things like the interpolation algorithm (an alternative for V0 and V7 control loop running for low side shunt hardware and persuaded the addition of full clarke transform..).

I try to condense this to a practical level and info that can be useful to those that do not actively enjoy math and physics. Hope it helps.

One day, I intend to make a video on tuning motors. Here I will outline my methodology in limited detail... I think it is kind of important because much of the common internet folklore about it is garbage/dangerous.

DO NOT:

* use slow abs max, EVER
* blindly increase limits to make errors go away

These are two commonly cited suggestions. This is akin to ignoring problems.

When I set up a new motor/controller, I roughly follow this order:

{% stepper %}
{% step %}
## Plug in controller

Preferably to current limiting PSU, 18V or the lowest it will turn on at and 0.2A. Check FOC offets on current sensors are sensible (typically 2048+/-150 counts).
{% endstep %}

{% step %}
## Set the general current and voltage limits

Set the general current to like 10A and 20A abs max.

Set the voltage limits to be sensible... I tend to use 10-15% above battery max voltage but always at least 5V off the FET rating. Under voltage I just put at a level where going lower would result in the controller turning off (typically 20V).

Take care that the battery current and voltage cut off limits are sensible to avoid power rollback and wondering why you have no power. Or blowing your battery.
{% endstep %}

{% step %}
## Press the bandbrake button

Measure the phase voltage with scope or multimeter - scope = 50% duty (might recently have changed to all low FETs on...) multimeter = vbus/2 (might now be 0V).

Now connect a motor.
{% endstep %}

{% step %}
## Repeat the measurement with a motor connected

Repeat 3), watch out for overcurrents. At this stage, if you get overcurrents, you probably have bad hardware or shorts somewhere.
{% endstep %}

{% step %}
## Set currents conservatively

Set currents to like half what your motor/ESC should be capable of, abs max like 50A over.
{% endstep %}

{% step %}
## Run detection RL and lambda

FOC page... run detection RL and lambda, with PSU set to half your battery voltage and maybe 5-10A... Many people have not got a PSU and so this becomes a lithium battery and YOLO. Check the results are sensible!

Flux linkages for EVs are usually:

* 10-20mWb region for in-runners,
* 20-40mWb for hubs.
* 2-10mWb for smaller outrunners can be down in the region...

Higher kV implies lower flux linkage, more PP implies lower flux linkage.

R and L... quite variable.

* big inrunners are in the 2-10mohm, 20-100uH region
* outrunners can belike 5-100mohm, inductance is hugely variable,
* hubs maybe 10-300mohm and 20-300uH

It is usually best to decrease the time constant to 400us. I have not seen a case where higher time constants helped except with broken hardware, where it can mask really bad current sensor readings.

If you get values much outside this, check it is really correct. Without correct values, there is little point proceding.
{% endstep %}

{% step %}
## Run hall/encoder detection

Remember if doing encoder you need to manually enter the encoder counts and pole pairs in the General Sensors tab before doing the setup in FOC Encoder... It can be really tricky to guess what the real counts are given motor datasheets since there are counts, transitions, steps... many factors of 2 and 4 between the naming.

Remember to apply and write the values using the Mdown button on the right...

At this point, your motor should spin and if you are lucky you are done and just need to set up a throttle.
{% endstep %}

{% step %}
## Engage MTPA if needed

However, if you are pushing a motor to the limit, or running a highly salient motor you usually need to go further.

Engage MTPA if your Ldq difference is more than maybe 20% of the L value. Use IQ measured, always, otherwise you can get extreme amounts of unintended field weakening.

At about this point you should consider jacking it up to the currents you actually want to run.
{% endstep %}

{% step %}
## Work through the observer options

Now we get to the painful world of observers. If you have an encoder... and it works... just set the erpm limit in the FOC encoder tab to be higher than your max erpm and it should just be better that way without effort.

If you have not... then you just need to try various things. Different motors respond better to different observers.

Generally, highly salient motors run best with ortega original and a very low gain whereas a lot of problematic motors which have rapid saturation and high pole count (like Tmotor U15) run much better with mxlemming observer.

My findings with running Ortega back to back with an encoder has been that the lower the gain, the better it tracks... right up until it becomes unstable and stops working atall. If you run a higher gain, you can easily push more more more amps... btu the tracking angle is bad and the power output drops off so you create much more heat for not much benefit.

The flux linkage observer uses the same gain factor as the main observer, which means you cannot tune them independently - this causes trouble, and has led to some dislike of the flux linkage observer.

If running Mxlemming observer, you can use the gain factor independently for the flux linkage... if using Ortega... cannot. Frequent comment from scooters has been that Mxlemming observer gets more torque, but Ortega more amps. I have verified this, BUT if you sit and mess with gain and inductance tuning, eventually you get the best result with Ortega original in many cases.

Other tips for getting higher current under sensorless:

* Use the FOC Sensorless Saturation Compensation Mode... use factor. Higher current = more saturation. Maybe start with like 30%.
* Maybe reduce the L or Ldq by 30%, this often improves stability but sometimes at the expense of efficiency.

I have never seen cross coupling help in an EV application. Turn it off, it is for things like servo drives with huge inductance and rapidly changing setpoints, not throttle twisting and accellerating.
{% endstep %}
{% endstepper %}

The key thing is to identify the real source of your problem. This is done using the "trigger sampled at fault" option, and inducing the fault while connected (USB, BLE, wifi...) to a proper computer. Nothing else is much use. NOTHING.

![1726752459417.png](https://endless-sphere.com/sphere/attachments/1726752459417-png.359817/)

This is a very nice looking trace, 500A, 60% duty cycle, 20s battery... But it was on low side shunts. Therefore, you start to get current sensing anomolies at high duty and current (dinks at the bottom of the sin). Perfectly useable though.

![1726752591302.png](https://endless-sphere.com/sphere/attachments/1726752591302-png.359818/)

Same thing but 80% duty. This starts to be a problem, there is not much you can do about this, it is a hardware limitation this device still pushed like 20kW though so meh)

![1726752727885.png](https://endless-sphere.com/sphere/attachments/1726752727885-png.359819/)

This is an example of unstable observer. The tracking goes off and therefore the currents go haywire. Need to fiddle the gain/inductance/saturation comp.

![1726752830149.png](https://endless-sphere.com/sphere/attachments/1726752830149-png.359820/)

Here is an example of a small motor running field weakening on an ESC with 600A current meaurement range. It looks rough, but it is fine. The blue line is inverting because it cannot decide if it is breaking or accellerating - free spin. There is some evidence of low side shunt abbheration but it is minor.

![1726753037795.png](https://endless-sphere.com/sphere/attachments/1726753037795-png.359823/)

The dinks in this are due to dead time abherration (ignore the first 0.02s, that is where it locks the rotor into synch). Not much you can do about it. Hardware limitation imposed by the maker. This is running on a phase current sensor board I had with very nice sensing but needed power stage tuning.

![1726753225393.png](https://endless-sphere.com/sphere/attachments/1726753225393-png.359824/)

This looks horrid because of noise but is actually nice. Board was 1800A sensors, running 20A. Be aware of the noise floor.

Annoyingly, I cannot find an example of the typical unstable observer behaviour. That looks like a pulsating sin wave, with fault triggering when the pulsation goes above the abs max. If I find an example, or next time I experience it... I will post.

Edit: [@thezenox](https://endless-sphere.com/sphere/members/100230/) sent me an example, thanks!

[![1732067214150.png](<.gitbook/assets/222974 47f198bc5ae62c38bd0b365a00d290f6.jpg>)](https://endless-sphere.com/sphere/attachments/1732067214150-png.362033/)

The reason this happens is that the observer struggles to accurately track the angle, and so the pwm output voltage phase angle changes faster than the PI current control loops can keep up. The motor will oscillate between leading and lagging power factors, and eventually even oscillate between motoring and regen. Observer gain and inductance (L and Lqd) are the tools to tune against this. Sometimes with very salient motors (or very saturated ones), it is hard to eliminate this. This is where Ortega observer can help; when applying the correction factor, the ortega observer applies it at an angle that results in a power factor reduction which is stable, but less efficient.

You can see in the above plot that the motor is motoring then regenerating in oscillation from the blue line (total current) going + then -.

Field weakening: the only setting I ever use on VESC is 200ms, 80-85% start duty. Ramp up the amps to taste... bear in mind infinite speed is when L\*id = lambda. Field weakening on VESC is somewhat crudely implemented but works for most cases. Where it is lacking is limited to applications where there are very rapidly changing setpoints at high speed, such as with a balance wheel pushed to the limit (EUC, VESC'd single wheel sk8board etc). In these cases, users have found that setting the FW to start at lower duty can be advantageous, since it suppresses the duty cycle (hit max duty = fall off on balancing machine). In an ideal world, the field weakening controller would apply field weakening very quickly with a feedforward term to avoid ever reaching this duty saturation while still allowing running at max duty, but this is actually surprisingly hard to implement mathematically and stably and would require extra loop tuning.

Offsets: This is one of the most common issues causing poor performance, especially at low power. If there's any error during startup you might find the controller doesn't measure the current sensor offset correctly. This can result in buzzing for small error and clunking for larger errors. It will inject noise into the control loops.

Some hardware drifts after startup, usually not by much. This is especially true on hall sensor based current sensors. Using the full Clark transform (FOC advanced all sensors combined mode) helps this a lot. This mode can be worse on low side shunts.

## G8trwood · Sep 25, 2024

Wondering what the go to controller is for reliability OffRoad.

Thanks for all the greatposts

## mxlemming · Sep 25, 2024

I cannot really recommend one against another. I only really work on my own hardware (which is not commercially available) and hardware I make for clients, which I am not going to divulge or credit one over another. I do not test common commercially available (aliexpress...) hardware.

Edit, there is now an obvious choice: [Home - VESC Labs](https://www.vesclabs.com/) since these will obviously be best supported by the VESC project, and covers the power range from small robot to mid size motorbike/personal plane/launch winch/surfboard/whatever magical creation you have...

## memeticmusic · Nov 1, 2024

I hope you make some videos, I'm trying to get HFI to start my qs205 with the 100v 250a spintend.

## Mihai\_F · Nov 6, 2024

Hy, nice, that is a solid explanation, man if you had made this post a few months ago, i had a lot of head bashing with my FOC code since then, (FOC questions thread....), now this clears some things out.

## L29Ah · Nov 21, 2024

Thank you for your great post, that's the best VESC FOC tuning article i could find! Thanks to it i figured i oversaturate my motor with all the current; reducing max current by 15% solved most of FOC faults i had, with crazy overcurrent spikes, without frying VESC 4.12:

![3qBn2G38PHMbrYMvZu1stcKRba3FBZbynue8vw6UKsQPVtv2yvuRpvvrLGUgYLeq1SkvryGn5jfeLNbiVjTVUbGo.png](.gitbook/assets/proxy.php)

Now i wish i understood what amps should i run it at, ideally. It's a "500W 36V" bike hub motor.

## mxlemming · Nov 22, 2024

That does look quite awful. I've seen worse, at least there's kind of a sin wave there hidden under the noise... I could make any number of guesses what's wrong there (initial guess is you'd somehow picked up vastly too high inductance parameter which has the current controller gain so high it's unstable but it could also be broken hardware or garbage adc readings due to corrupt firmware/settings/...) But it's best to figure it out in front of the hardware where you can change things and resample quickly.

Hubs are really hit and miss with hfi. I have one that works perfectly (cheap leaf motor replica) and some that are totally hopeless (superflux funwheel motor). I spent hours on the superflux and got nowhere with hfi using vesc or my code. Fortunately they come with hall sensors. Key parameter is inductance for hfi, tweak it up and down in 10% intervals, you might have to go quite far. Really it needs a separate hfi inductance/threshold parameter but that's a thing for Benjamin some other day.

## mxlemming · Nov 22, 2024

My new dyno arrives soon so I'm holding this early post to put a video of tuning it in case the masses gradually flood this thread.

## egtscs · Nov 28, 2024

First off, thank you very much for such a thorough guide!

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1824085)
>
> DO NOT:
>
> 1. use slow abs max, EVER
>
> Click to expand...

I'd like to ask: with a clear understanding of the fact that slow abs max will not solve any underlying issues with the tuning of your control system, does this advice hold true to vehicles that have already been tuned properly and rely on power delivery for rider safety? (such as onewheels, electric skateboards, or EUCs)

My understanding was that while slow abs won't solve the issue causing a fault to occur it can help certain vehicles fail in a way that better protects the rider.

## mxlemming · Nov 28, 2024

> [egtscs said:](https://endless-sphere.com/sphere/goto/post?id=1830137)
>
> First off, thank you very much for such a thorough guide!
>
> I'd like to ask: with a clear understanding of the fact that slow abs max will not solve any underlying issues with the tuning of your control system, does this advice hold true to vehicles that have already been tuned properly and rely on power delivery for rider safety? (such as onewheels, electric skateboards, or EUCs)
>
> My understanding was that while slow abs won't solve the issue causing a fault to occur it can help certain vehicles fail in a way that better protects the rider.
>
> Click to expand...

As far as I am concerned, slow abs is simply never OK to use. If it seems to make things better, it means you are way out of control or have broken hardware throwing random bad current samples.

Typically, if you have... say... 250A setpoint and 350A abs, then to trigger it, you must be 100A in error. If you have a properly tuned system that got that far into error, the chance of it finding it's way back is close to nothing. If you look at the last pic in my original post, you see the oscillations growing. In that, it it has abs'd out with a 150A threshold. If you enabled slow abs to overcome this, it might last a few more samples, but would then abs out with... 300? 400? Who knows how many amps as the oscillation grows.

Where it sometimes appears to help is like in the below (thanks to thezenox again for the pic) where you have an instability. in this case, it will mask the issue for quite some time, but... one day it will bite you. Note this pic is a sinusoidally pulsating sin wave... This will sound like a 100Hz hum/vibration.

[![1732786811879.png](<.gitbook/assets/223391 643186e184d2b0686bc8ad7cfc47d5fe.jpg>)](https://endless-sphere.com/sphere/attachments/1732786811879-png.362450/)

I would be very sad to leave a motor tuning looking like this. If I was relying on it to hold my face away from tarmac, I don't think I would accept it.

Slow abs is pervasive behaviour, and the recommended option given all over the place. Use it if you want, I will be laughing if you trade landing on your face for landing on your face with a dead controller. That said, most controllers die because of miswiring, shorting battery to throttle input/whatever...

I do get that for self balancing objects it can be very hard to test your setup, unlike say a motorbike where you can just grab the rear brake for a split second, or use the inertia of the wheel spinning up while on a stand.

## egtscs · Nov 28, 2024

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1830140)
>
> If I was relying on it to hold my face away from tarmac, I don't think I would accept it.
>
> Click to expand...

I think I'm struggling to phrase my question properly. I also might be being pedantic, I am trying to better understand please bear with me.

I understand that slow ABS OC is not a solution to an improperly tuned system, the system must be tuned to be capable of measuring current as it flows within a reasonable margin of error to avoid OC faults. (I think that's the general idea)

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1830140)
>
> If it seems to make things better, it means you are way out of control
>
> Click to expand...

I'm actually asking about the opposite of this, what if it seems to have no effect, what if your system shows predictable power delivery and no OC faults during testing.

I'm not considering using slow ABS OC as a solution. I'm asking about solving tuning issues first, then deciding on slow ABS OC.

I guess a more important question, is it at all possible for a properly tuned system to throw an OC fault? (water ingress over time, electrical noise, degradation of components, motor/wire issues, environmental factors)

If that's genuinely impossible regardless of other circumstances I can be done here lol.

If it is at all possible would slow ABS OC give the user any more time to catch themselves, even fractions of a second can help quite a bit on an electric skateboard.

## mxlemming · Nov 29, 2024

> [egtscs said:](https://endless-sphere.com/sphere/goto/post?id=1830159)
>
> I think I'm struggling to phrase my question properly. I also might be being pedantic, I am trying to better understand please bear with me.
>
> I understand that slow ABS OC is not a solution to an improperly tuned system, the system must be tuned to be capable of measuring current as it flows within a reasonable margin of error to avoid OC faults. (I think that's the general idea)
>
> I'm actually asking about the opposite of this, what if it seems to have no effect, what if your system shows predictable power delivery and no OC faults during testing.
>
> I'm not considering using slow ABS OC as a solution. I'm asking about solving tuning issues first, then deciding on slow ABS OC.
>
> I guess a more important question, is it at all possible for a properly tuned system to throw an OC fault? (water ingress over time, electrical noise, degradation of components, motor/wire issues, environmental factors)
>
> If that's genuinely impossible regardless of other circumstances I can be done here lol.
>
> If it is at all possible would slow ABS OC give the user any more time to catch themselves, even fractions of a second can help quite a bit on an electric skateboard.
>
> Click to expand...

Reducing the fault recovery time is better. I understand what you mean.

It is not useable fractions of a second. Overcurrent faults occur on the <1ms timeframe (samples are 15000 current samples a second, and slow abs IIRC ignores the fault until the threshold is reached by an exponential filter with default constant 0.1... so you are looking at quite a lot (order of 10) samples before exceeding the threshold. This is enough to blow a power stage, but not enough to even feel.

A well tuned system should never throw a fault, but yes sometimes it does due to external circumstances. slow abs may help with that, but reducing the fault recovery time to something that would not eject the user.

You can endlessly debate the tradeoff of "a system that runs perfectly won't fault so there's no harm in ignoring the faults" vs "a system that runs perfectly won't fault so there is no need to ignore the faults" and yes, when personal injury is a factor, then maybe you want that last little % of hope that it does not explode and recover... but that is very much not the tradeoff 99% of people using slow abs max are making, they are using it because they do not know how to tune, or have something broken and just want the problem to go away.

This is the extreme philosophical end of the debate, and it is not applicable to most situations. I will accept you might be correct if you are truly tuned well.

Ideally, we would not fill this thread with the slow abs debate...

## egtscs · Nov 29, 2024

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1830226)
>
> It is not useable fractions of a second. Overcurrent faults occur on the <1ms timeframe (samples are 15000 current samples a second, and slow abs IIRC ignores the fault until the threshold is reached by an exponential filter with default constant 0.1... so you are looking at quite a lot (order of 10) samples before exceeding the threshold. This is enough to blow a power stage, but not enough to even feel.
>
> Click to expand...

I think this is the reply I was looking for, thank you for elaborating.

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1830226)
>
> This is the extreme philosophical end of the debate, and it is not applicable to most situations.
>
> Click to expand...

You're correct, I tend to better understand a concept after these debates.

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1830226)
>
> Ideally, we would not fill this thread with the slow abs debate...
>
> Click to expand...

Also correct, I'm unfamiliar with this forum, please forgive me while I adjust to the proper etiquette. Is there a "beginner question thread" or another better place to ask questions/have discussions like this?

## e-hover · Feb 7, 2025

I've been using your brilliant tuning guide to try to increase the thrust on my diy electric hovercraft. Although it runs well enough to carry 2-3 adults, above about 400 motor amps the STR500, driving an MP202150 motor, tends to cut out with the usual 'abs current' fault. The craft has a 10kwh 14s LIPO battery.

Following your guide I've taken samples of the phase waveforms etc. It shows that for example at 370 motor amps, where it is apparently running smoothly, there's actually some oscillations in the top of the waveform and the waveform seems to have a kind of dc offset - the top of the waveform peaks around +400A, whilst the bottom curves correctly around -270A. I've attached a sample showing just one phase for clarity but all three are similar.

I've taken similar samples on the lift motor (VESC 75/300 driving an MP15470) but that shows textbook sinusoidal waves even at full 5kw power, and no 'abs alarms' - see sample.

Any ideas on the causes of the oscillations and the 'dc offset' and maybe what settings to adjust? I'm assuming that these oscillations progress as the amps go up and are the root cause of the abs current alarms at the higher power.

### Attachments

* ![plot1430 370A.jpg](<.gitbook/assets/226312 699644dedffbd94749e0f8bc62079154.jpg>) [plot1430 370A.jpg](https://endless-sphere.com/sphere/attachments/plot1430-370a-jpg.365371/) 105.1 KB · Views: 50
* ![lift 5kw.jpg](https://endless-sphere.com/sphere/data/attachments/226/226313-2d323bbaedbeaea0bdd28c348748be4.jpg) [lift 5kw.jpg](https://endless-sphere.com/sphere/attachments/lift-5kw-jpg.365372/) 115.4 KB · Views: 51

## Foujiwara · Feb 25, 2025

This post is way underrated, Thank you very much for this explanatory post, it gives more information in a few lines than anything I have heard and read for a long time!

I am happy to see that I had a good method and understanding on several points discussed.

## roscheee · Feb 25, 2025

yep

## colmor · May 16, 2025

Very well put together and educational post. Thank you.\
I'm new but learning and this helped me to realize how the Flipsky (FSESC75350) is sub-par and causes problems/faults when you run it over \~75% what they claim to be rated at. There is a reason they suggest turning ON Slow ABS Max- the current bounces around wildly at higher loads!!\
Finally got some Trampa 75300 MK IV VESC's and they output _so_ much cleaner current, no more Slow ABS Max, and no faults!

## chuyskywalker · May 17, 2025

> [colmor said:](https://endless-sphere.com/sphere/goto/post?id=1849950)
>
> Finally got some Trampa 75300 MK IV VESC's
>
> Click to expand...

Yeah, lol. You went from bottom of the barrel to top tier. If you build more, you can give spintend controllers a try; they are near trampa quality, but closer to flipsky prices; I've been really happy with them.

## mxlemming · Aug 20, 2025

![www.vesclabs.com](<.gitbook/assets/proxy (1).php>)

### [Home - VESC Labs](https://www.vesclabs.com/)

VESC Labs - The official range of motor controllers, battery management systems and accessories powered by VESC Tool. Manufactured from the highest quality components, ensuring maximum power, reliability and functionality.

![www.vesclabs.com](<.gitbook/assets/proxy (2).php>) www.vesclabs.com

Benjamin and Co have released their own lineup of controllers and auxilliary bits, probably worth giving a go as the new reference hardware for the VESC project!

## j bjork · Aug 20, 2025

I thought trampa was the official vesc stuff that I thought Benjamin was part of\
Edit, it was but they have now parted ways.\
Read more about it here:

### [VESC Project](https://vesc-project.com/Hardware)

![vesc-project.com](<.gitbook/assets/proxy (3).php>) vesc-project.com

## scianiac · Aug 20, 2025

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1862867)
>
> <img src=".gitbook/assets/proxy (1).php" alt="www.vesclabs.com" data-size="original">
>
> ### [Home - VESC Labs](https://www.vesclabs.com/)
>
> VESC Labs - The official range of motor controllers, battery management systems and accessories powered by VESC Tool. Manufactured from the highest quality components, ensuring maximum power, reliability and functionality.
>
> ![www.vesclabs.com](<.gitbook/assets/proxy (2).php>) www.vesclabs.com
>
> Benjamin and Co have released their own lineup of controllers and auxilliary bits, probably worth giving a go as the new reference hardware for the VESC project!
>
> Click to expand...

I've been waiting for this release, I like this lineup, beyond 20S has been so limited options wise with some disappearing recently, a high quality high power option is a real win. Fully potted is nice waterproofing wise. Look at those pinouts man, ALL THE PINS.

Interesting they are low side shunts, but what do I know, I trust they had good reasons for it and that it works well. I think higher voltage kinda allows us to run a higher voltage battery so we use high duty cycle left so low side shunts work better, idk?

## TPEHAK · Aug 21, 2025

> [mxlemming said:](https://endless-sphere.com/sphere/goto/post?id=1862867)
>
> <img src=".gitbook/assets/proxy (1).php" alt="www.vesclabs.com" data-size="original">
>
> ### [Home - VESC Labs](https://www.vesclabs.com/)
>
> VESC Labs - The official range of motor controllers, battery management systems and accessories powered by VESC Tool. Manufactured from the highest quality components, ensuring maximum power, reliability and functionality.
>
> ![www.vesclabs.com](<.gitbook/assets/proxy (2).php>) www.vesclabs.com
>
> Benjamin and Co have released their own lineup of controllers and auxilliary bits, probably worth giving a go as the new reference hardware for the VESC project!
>
> Click to expand...

Where I can download the schematics for those things? If I go the the DOWNLOAD section it has DATASHEET file but there is no schematic in that file

![1755762487048.png](<.gitbook/assets/236836 de0fe862a162599f3d53986d6e9b32c9.jpg>)

## kuromaku · Aug 21, 2025

> [scianiac said:](https://endless-sphere.com/sphere/goto/post?id=1862884)
>
> Interesting they are low side shunts, but what do I know, I trust they had good reasons for it and that it works well.
>
> Click to expand...

I can think of only a handful of opamps that can withstand a common mode input of 150V+, and their GBWP isn't going to be stellar, as well as expensive, not rail-to-rail input, and for the whole circuit the resistors would have to match really well, and also the relatively low input impedance of the whole thing. That said, I do wonder if they have separate mechanisms for detecting the usual HS-sense things like a load-to-ground short.

## scianiac · Aug 21, 2025

> [kuromaku said:](https://endless-sphere.com/sphere/goto/post?id=1862996)
>
> I can think of only a handful of opamps that can withstand a common mode input of 150V+, and their GBWP isn't going to be stellar, as well as expensive, not rail-to-rail input, and for the whole circuit the resistors would have to match really well, and also the relatively low input impedance of the whole thing. That said, I do wonder if they have separate mechanisms for detecting the usual HS-sense things like a load-to-ground short.
>
> Click to expand...

I thought for these high current VESCs most use hall current sensors or some similar non-shunt based setup instead? I'm sure there are good reasons why not to use those, low performance at low current? To be clear I have just enough understanding to know I have no idea what I'm talking about, just curious.

## mxlemming · Aug 21, 2025

There is an enormous difference between a good and bad implementation of low side shunts. The tricky bit is that you need to have the opamp stabilise fast enough that it gets a correct current reading in the tiny time available when the MOS bridge is low. It is hard, many of the ESCs with low side fail to do this, and get poor performance as a result. The problem gets worse and worse with higher current, but it is solvable...

Phase shunts sidestep this problem, but lower voltage.

Current rings are typically slower to respond so the HFI does not work so well, but they also sidestep the issue.

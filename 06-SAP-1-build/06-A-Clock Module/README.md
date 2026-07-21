# What is a Clock #

> At the heart of every computer is a system clock — a component that generates a continuous stream of electrical pulses called the clock signal. This signal alternates rapidly between HIGH and LOW voltage states, or TRUE/FALSE state. or 0/1 State. This also from where the idea comes that computers think in just 0s & 1s. Ie absence of Elctricity is understood by computer as 0 and presence of electricity is understood by computer as 1
>Modern computers use a crystal oscillator for this — a tiny quartz crystal that vibrates at a precise frequency when current is applied, a phenomenon known as the piezoelectric effect. Those physical vibrations are converted into a stable, accurate clock signal.
>The clock doesn't carry data — it just tells the computer at WHEN to excute the certain task. 

Without a clock, every module would act at its own pace. A register might try to store a value before the ALU finishes calculating. RAM might output data before the address is even set. Everything would collide. The clock prevents that — it keeps every module marching to the same beat, ensuring operations happen in the right order, every time.

## Rising and Falling states ##

>  the state when the pusle on of the clock is in a transition state of going from 0v to Xv or LOW to HIGH or 0 to 1 is called the rising edge of the pulse. 
> the state when the pusle on the clock falls back Xv to 0v after an interval or HIGH to LOW or 1 to 0 is known as falling edge of the pulse

> The goal of a clock module is to generate these pulses continuosly indefinetly or on command

And every time when state either rises or falls. the computer executes one command. and thus we need these pulses and clock signal to go on continuosly indefinetly so that the computer can keep operating and executing commands.

> A computer which is executing its commands on rising edge of pulse is called a rising edge synced system
> A computer which is executing its commands on falling edge of the pulse is called a falling edge synced system

Our SAP computer is synced at rising edge of the pulse. Ie all the commands and executions happen when our clock pulse switches from low to high state.

# CLock Module Architecture #

Our Clock module consists of 2 different types of Clock Signals. Mono Stable and Bistable

> A bistable Clock is bascially what we discussed earlier. generating the signal indefinetly forever. is its constantly switching between 2 states (high and low) hence Bi stable

> but on the other hand when we are actually builduing the computer, we moight wanna manually pulkse the clock one command at a time. say we are debugging something or just wanna slow down the lcok to go at our own pace to see hwats going on while a program executes. for that we use a mono stable cvlock. a monostable clock is basically a pulse set in on estate (high or low) unless triggered by an ecxternal command. say for our computer a command executes at rising at of a clock so the monostable clock going from 0 >>> 1. so our mono stable clock will default to a state of 0. and we can say press a button and it will go high for a breif period of time and then default backl to zero until we press the button again. but the time it goes from 0 to 1. it generates a risijng edge which uis exactly what we need from a clock module to synce the commands on the computer. thus we can say generate a puls eby press of a button. debug and se ehwat we wanna work with and then when we are ready to execute the nect comnmand we press the button again to gebnerate anoither pulse and so on.

thus a mono stablke clock will be used when debugging and builduing the computer whule a bistable clock will be used when we are executing the commands and running programs on the final build. here is the clock module i built. architecture is explained below 

# CLock Module Scematics #

>>> BISTABBLE CLOCK <<<

A very common Ic used to generate such PWM signal; is know NE555P or a  555 timer IC. when wired as shown in the schematics below, it generates a steady PWM signal on it=s out put pin 3. we can change how fast or slow th eclock goes; ie duty cycle and on time by using a variable resistor as shown or changing the value of capacitor on pin 2. a capcitor with low capcitance with generate a faster clock and vice versa. for our convinbece ive used a 1uF capacitor and used a 1Mohm variable resistor to set the pace of the clock as per needed. Il build a copmprehensive guid eon how this clock works but for now you can take a look at Data shgeet of the 555 timer wehich gives us the numericals and brefis of how the clock works

<!-- add the page showing 555 timer clock fdrom DATA sheet and shcematics for Bistabel clock -->

>>> MONO STABLE CLCOK amd Swi9tch bouning <<<

we can simply use push button with a pull down resitor but there is an issue with that, push buttons are spring liades.w hich meand they hjave physical contact pads inside them wghich complete the circuit which might oscillate and generate multiple pulse even thiough we just need one.

thinkj like a heavy load attached to a spring. if we pull that load down a ndn let go, assume like a push of a button. the spring oscfilates a few tim,es up and dowm, before coming to the rets./ same happens inside a push button,. it oscillate a few times thus genertaing muykltiple pulses when only one is neeeded. toi fix this w need a debouncing circuit

although there are manier weays of making a debounciong circuits , isnce were on topic of 555 timer. im gonna use the same IC to amke it and casue its very cheap and common to find. the data sheet shows us that 555 timer conmtains an SR latch and were gonna use that to our advantage,

> here i have a simplified version of the 555 timer drawn.

> it essentially has an SR latch whos Q output is our pin 3. ie the out put pin of the iC and Q* pin is conneted to the base of a transistor. then one of the the pin sof transistoir is connected to gnd and other pin is connectd to pin 7 ie the discharge pin of the IC. essentially making a C B E transisior whiuch gives us a NPN tranbsistyor confiuguyration., basicaaly when the current flows through base of transistor via the Q* pin of SR latch. it connects the deischange pin 7 to gnd.

>next the resistoir nettwork insde thge ic sets 2 volateg dividers with refenec points of 1.67 v and 3.33 v as s hwon.. teh dischagre pin is also connected to 5v via a 1M ohm resistor and pin 6 is connected ot gnd via a capaitor. pin 6 and 7 bare connectd togethr on their own too

>the 3.33 v refrecne goes to inverting input of Comparator A which feeds into resety pin of SR lactjh
> the 1.67 v refrence goes to non inverting input of comparator B which feeds into set pin of SR latch

> noe essentially if we wnat lacth to set, and giv ean out put on pin 3. we want refrence V+ >>> V- on comparator B ie  1.67V (V+)(B) >>> V-(B) and if we want latych to reset we wnat V+(A) >>> 3,33V (V-)(B)

> the initial V-(B) is connected to pin 2 of thge ic (trigger) through a pull up resistor to 5v thus V-(B) is default to 5v. thus V-B (5v) >>>> V+(B) 1.67V. thdu we will have a 0 on input of set pin of latch thus on initial power up we will always start on 0 at Q pin. and a 1 at the reste pin and thu s1 at q* pin of latch.

pin 2 is also connectd to gnd via a push button which we will use o generate the pusle.

# Workings #

> since initailly latch is in rest mode ie Q* = 0. it powers the base of transistor and and thus any cuirrecnt flowiung through the 1M ohm resistor goes to gnd and any charg eon capacitor also discahges via gnd and transistor.

> Next when siwtch is pressed : V-B is no 0v and since V-b(0) <<<  V+b(1.67). the comparator gioves an out put ehich sets the latch. givuing us an out put and and Q* now = 0

> after that the base no longer gets powered. the curreent starts flowing through gnd via the capacitor instead of transistor and thus cahrginbg the capcitor. (note that siwcthed is realeased by this point. we onmly press it for a breif sec and let go)

> the capacit starts chargings for the duartin we hod down the siwcth , assume it charges >>> 3.3 v while the duration of our switch is presesed. now V+A >>> 3.3 thus it resets the latch which turns of the output an dhold it at 0 while teh current again drains via transistor again and capcitor also discahrges via transisot.e sentially repeating the cycle when button is pressed next.

# pulse generatiuon 

> to make this system genertae a pulse we need it such that capcitor charges fully (or atleast > 3.3v) in the duration we momentarily press the swithc and on the counteratcive side we want the latch to turn on. capciutor to charge thus resetiing thge lacth and then discahrge again for it to fall back to 0. there are 2 ways 2 do this.

> first we need the time vairble delta T to be ---> 0 ie we want capcitopr to charge and discharge in as less time as possible. thu sreducing duty cycle to as little as possible ie. duty cycle delta D ---> 0 which wil essentially simulate the clock cycle of going from LOW >>> HIGH >>> LOW again in that delat T --- > 0 tiem frame refrence. and our computer can detect the rising edge froim that. and while if anyt debouncing cocurs psot press of the switch. it wont matter sicne were using an SR latch so out put eill be latched no matter what unless the capcitor refrecne dischatrges to 0. esentially rmoving the dwitch bouncing. to do that we can use a capacitor witha a very small caapcity like 0.1 nF thus it charges quicky in th etim edelta T---> 0 refecne which is the time we keep the swicth pressed. and sicne it discharsges very quickly too since its very small capcituy. it also gives us duty cyclke delta D --- > sicne its pretty much instantanous. thus simulating aa pulse for us. thus in thi smanner we can create a swicth debouvner for the clockj using a 555 timer vbelow. ive shown the schemtics below

<!-- attach a schemtic and LT SPCIE grapghs here --->

# slector and halt line

Now that we hav e2 independetnt clocks which can geenrate a pulse on demand as a mono tsble clock and a continoius ruynning clock as bistable clock. we need a way to seamlessly switch between them. also its a good idea to add a halt line which essentyialy stops the clock in its last position. this is use ful when debuggingf or we wanna stop the clock once our program is donrt executing/ for the halt line ansd selction. ive designed a circuit as shown below :

<!- add a video in DLS of clock slelctor >

first out put of both the clocks is fed into their independent AND gate, then second input of each of their AND gate is fed withg an out put of an SR NOR latcvh.

from teh diagram :

AND gate A : input A is clock 1 ; input B is Q out put of latch
AND gate B : input A is clock 2 ; input B is Q* out of latch

thus only one AND gate will out put a signal at a time which will be the clock out put itself. 

Next both of these inputs are fed into an OR gate. thus either of the clock can be active to give an out put

Next an input is fed into a NOT gate. thus;
when HALT line is low --> NOT gate : high
when HALT line is HIGH --> NOT gate : 

the output of NOT gate is then anded with the out put of the OR gate which give us our finbal out put. thus whenerve halt line is pulled high. our clock will halt since anything comin g from or agte anded with 0 from halt line will essential out put 0 at and gate, ie our final output

this essentially cocnludes the clock module

we have a clcok module which can now generate a contionous PWM pulse along with a debounced clock which can generate pulses on demand. also we have a seamless way to switych between them and a HaLT line which halts/stops the clock output. this clock module will be used to keep every other mdoule which we build next in sync


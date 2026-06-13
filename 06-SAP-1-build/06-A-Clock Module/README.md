# What is a Clock #

At the heart of every computer is a system clock — a component that generates a continuous stream of electrical pulses called the clock signal. This signal alternates rapidly between HIGH and LOW voltage states, or TRUE/FALSE state. or 0/1 State. this also from where the idea comes that computers think in just 0s & 1s
Modern computers use a crystal oscillator for this — a tiny quartz crystal that vibrates at a precise frequency when current is applied, a phenomenon known as the piezoelectric effect. Those physical vibrations are converted into a stable, accurate clock signal.

The clock doesn't carry data — it just tells the computer at WHEN to excute the certain task. 
Without a clock, every module would act at its own pace. A register might try to store a value before the ALU finishes calculating. RAM might output data before the address is even set. Everything would collide. The clock prevents that — it keeps every module marching to the same beat, ensuring operations happen in the right order, every time.

## Rising and Falling states ##

1. the state when the pusle on of the clock is in an intermideate state of going from 0v to Xv or LOW to HIGH or 0 to 1 is called the rising edge of the pulse.
2. the state when the pusle on the clock falls back Xv to 0v after an interval or HIGH to LOW or 1 to 0 is known as falling edge of the pulse

The goal of a clock module is to generate these pulses continuosly. Thus going from :  
## HIGH : (FALLS) > LOW : (RISES) > HIGH : (FALLS) > LOW : (RISES) > HIGH : (FALLS) > LOW : (RISES) ............ ##   
And every time when state either rises or falls. the computer executes one command. and thus we need these pulses and clock signal to go on continuosly indefinetly so that the computer can keep operating and executing commands.

Our SAP computer is synced at rising edge of the pulse. Ie all the commands and executions happen when our clock pulse switches from low to high in that intermediate stage

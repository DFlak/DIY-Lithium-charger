# DIY Lithium charger


<b>First of all, I'm not in any way responsible for any damage that 
could be an outcome of using any part of this circuit.
Lithium batteries are dangerous and shouldn't be handled by unexperienced
persona. Never leave your packs unattended.</b>


I'm aiming to develop a cheap, safe, easy to build and multi-channel lithium charger.
Everyone knows that you can already buy things like iMax B6 clones or whatever. Simple answer is 
those chargers only allow you to charge one battery at a time and their balancing current is less than enough
for anyone that built a battery from used or mismatched cells.

Main goals:<b>

    Independent, balanced channels,
    Easy and cheap to expand,
    Possibly the smallest form factor,
    Wide input and output voltage range, 
    Making it safe to use.

Please remember that it is all work in progress.

## Balancer

The circuit used to build this balancer was created by Afdhal Atiff Tan Amin Husaini Tan from the http://www.afdhalatifftan.com/ blog. His work and his results inspired me to create a board from his schematic. The board uses 10mil trace width which is big enough for toner transfer and unskilled person and also plenty of spacing. The only thing worth mentioning is that resistors which form differential amplifier need to have 0.1% or less tolerance. I was succesful at buying 600pcs of 1k resistors from AliExpress and sorting them by using ADS1115 ADC. Basically you're looking for resistor closest to 1k value with a DMM and then you compare this resistor with any unknown 1k5% one. If value gets close enough LED blinks which indicates that its' resistance is close enough to be 1% tolerance. I can share the code and schematic if anyone needs it but its really basic. Sorting 600 resistors took me about 1,5h and yielded 47 1000+-1ohm and 12 1012+-1ohm resistors. 
A thing worth noting is that it doesn't matter if you choose 1012ohm resistors or 980ohm or whatever value. Only thing that matters is that those resistors need to be 0.1% or even closer to each other. 
I've created this design on regular FR4 1,6mm 35um copper clad. Initially I wanted to etch two sides on one-sided clads and then solder them together by vias but it ended up looking like crap and taking way more time that it needed. 
Will post updates with photos as I etch double-sided clad and test this circuit.
- [*] Create prototype PCB
- [*] Get really dissapointed with the results
- [ ] Order some double-sided PCBs
- [ ] Etch and prototype
- [ ] Post results

## The bare bones 

Basically this charger will push current into lithium packs using (LM2596 based step-down converters) [https://www.aliexpress.com/item/1Pcs-LM2596S-DC-DC-Constant-Current-Module-LM2596-DC-DC-7V-35V-Step-down-Adjustable-CC/32849005778.html?spm=a2g0s.9042311.0.0.27424c4dF48BmC]. They have usefull feature which is constant current mode and low drift floating voltage. However, them being step-down topology they need their Vin to be at least 2,5V greater than output. Anything lower than that can result in unstable operation of such converter. To remedy this problem I utilized (150W DC-DC Boost converter) [https://www.aliexpress.com/item/150W-DC-DC-Boost-Converter-Step-Up-Power-Supply-Module-10-32V-To-12-35V-10A/2038554691.html?spm=a2g0s.9042311.0.0.27424c4dqFSutO] which is set to 19,5V, 2,7volts higher than full 4S Li-po pack. 
In order to stack the LM25965 converters I needed to change the orientation of trimpots from vertical to horizontal. It was done easily with a help of solder wick. Converters are held to each other via epoxy glue on top of the electrolytic capacitors. Heat didn't pose any issues while charging at 1.7A x3 packs. Neither LM25965 nor 150W boost converter were hot to the touch. 
<img align="centert" width="200" height="200" src="https://github.com/DFlak/DIY-Lithium-charger/blob/master/Bare%20bones/charger.jpg">

I was powering this ciruit from diy-made 4s pack. 
The bare bones of this project are considered done. To expand this charger for more packs you would need to put more LM25965's in parallel keeping in mind the maximum of 150W for each boost converter. My estimate is that one boost converter can supply five charger modules.

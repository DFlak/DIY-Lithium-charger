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
- [x] Create prototype PCB
- [x] Get really dissapointed with the results
- [ ] Order some double-sided PCBs
- [ ] Etch and prototype
- [ ] Post results

## The bare bones 

Basically this charger will push current into lithium packs using [LM2596 based step-down converters](https://www.aliexpress.com/item/1Pcs-LM2596S-DC-DC-Constant-Current-Module-LM2596-DC-DC-7V-35V-Step-down-Adjustable-CC/32849005778.html?spm=a2g0s.9042311.0.0.27424c4dF48BmC). They have usefull feature which is constant current mode and low drift floating voltage. However, them being step-down topology they need their Vin to be at least 2,5V greater than output. Anything lower than that can result in unstable operation of such converter. To remedy this problem I utilized [150W DC-DC Boost converter](https://www.aliexpress.com/item/150W-DC-DC-Boost-Converter-Step-Up-Power-Supply-Module-10-32V-To-12-35V-10A/2038554691.html?spm=a2g0s.9042311.0.0.27424c4dqFSutO) which is set to 19,5V, 2,7volts higher than full 4S Li-po pack. 
In order to stack the LM25965 converters I needed to change the orientation of trimpots from vertical to horizontal. It was done easily with a help of solder wick. Converters are held to each other via epoxy glue on top of the electrolytic capacitors. Heat didn't pose any issues while charging at 1.7A x3 packs. Neither LM25965 nor 150W boost converter were hot to the touch. 



<img align="centert" width="600" height="600" src="https://github.com/DFlak/DIY-Lithium-charger/blob/master/Bare%20bones/charger.jpg">


I was powering this ciruit from diy-made 4s pack. 
The bare bones of this project are considered done. To expand this charger for more packs you would need to put more LM25965's in parallel keeping in mind the maximum of 150W for each boost converter. My estimate is that one boost converter can supply five charger modules.

## Control, safety and feedback

This charger will use Arduino Nano as a controller. Packs will be switched with a 5V RelPol relays. Each cell will be monitored using [CD74HC4067](https://www.aliexpress.com/item/Smart-Electronics-CD74HC4067-16-Channel-Analog-Digital-Multiplexer-Breakout-Board-Module-for-Arduino/32724398868.html?spm=a2g0s.9042311.0.0.61b64c4dKA2mvk), calibrated resistive voltage divider and [ADS1115 16bit ADC](https://www.aliexpress.com/item/I2C-ADS1115-ADS1015-16-Bit-ADC-4-channel-Module-with-Programmable-Gain-Amplifier-2-0V-to/32674556200.html?spm=2114.search0104.3.9.12472300DBfJgK&ws_ab_test=searchweb0_0,searchweb201602_4_10065_10068_10843_10059_5016517_10696_100031_5016717_10084_10083_10103_451_10618_452_10304_10307_5016617_10820_10301_10821_5016417,searchweb201603_45,ppcSwitch_5&algo_expid=fb968794-6da6-4dc2-80d7-a833cd81f520-1&algo_pvid=fb968794-6da6-4dc2-80d7-a833cd81f520&transAbTest=ae803_2&priceBeautifyAB=0). I'm planning on adding current monitoring but it all depends whether I can develop reliable and cheap low-side current meter without introducing any more losses. Voltage of each cell will be displayed on regular [16x2 LCD](https://www.aliexpress.com/item/1pcs-lot-New-1602-16x2-Character-LCD-Display-Module-HD44780-Controller-blue-blacklight-IN-STOCK/32328960211.html?spm=2114.search0104.3.2.56aa3c811IH5dc&ws_ab_test=searchweb0_0,searchweb201602_4_10065_10068_10843_10059_5016517_10696_100031_5016717_10084_10083_10103_451_10618_452_10304_10307_5016617_10820_10301_10821_5016417,searchweb201603_45,ppcSwitch_5&algo_expid=6a017778-b50d-4f92-a064-2eaae25a3f11-0&algo_pvid=6a017778-b50d-4f92-a064-2eaae25a3f11&transAbTest=ae803_2&priceBeautifyAB=0). Arduino will guard if every cell's voltage is in 3.2V < X > 4.2V range. Only then will it allow the relay to connect pack to the charger. Good idea would be to add a capacity meter, capacity limit and charging time limit. First two are dependant on my current meter so I don't know if it will be included.
[x] Order all of the parts
[ ] Receive all of the parts 
[ ] Think of a way to measure current
[ ] Create schematic
[ ] Create .brd 
[ ] Etch and prototype PCB
[ ] Create arduino code
[ ] Test the thing thoroughly
[ ] Post all the files here

###If anyone is interested in helping me feel free to contact me via mail at wtflolrotfl@gmail.com :)

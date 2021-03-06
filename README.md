## Concept

<p align="center">
<img src = "drue.png" class="inline" width="200"/> 
</p>

### Introduction
Drüe, a piano-themed Whac-A-Mole game, aims to pull young children away from computer screens and help them develop hand-eye coordination, motor skills, and memory. On a basic level, Drüe can run classic Whac-A-Mole; however, different modes can be used to teach piano, test reaction time and improve memory. 

The system architecture works as follows. The Mbed signals which “mole” or “key” the user has to press. After the user presses the key, the Mbed processes the input depending on the mode it is currently operating in, thereby closing the loop. 

### Baseline Goals
The proposed milestones will be used to guide development. 
1. Fully figure out the mechanism for one key 
2. Implement selection menu and at least two modes, such as mini piano and reaction test 
3. Drüe runs Whac-A-Mole, along with a score-keeping LCD screen 

### Reach Goals
These goals outline other features that may be implemented if time permits. 
- Advanced Whac-A-Mole requires different types of inputs (e.g. hold key) 
- Drüe runs a memory test, akin to Simon 
- Drüe can teach piano 

### Technical Goal
From a technical standpoint, our goal is to create a self-contained product, complete with a custom PCB, clean enclosure, and power supply. We also wish to implement algorithms that make the project sound as piano-like as possible. 

## Alpha Prototype 
For the alpha prototype, a very simple version of Drüe was created using a breadboard, LEDs, and simple push buttons. As for the functionality, a very simple version of Whac-A-Mole was coded where a button press would play a tone only if the corresponding LED was lit. A seconday mode was also included where the buttons acted like a piano. 

The main problem we encountered was needing to debounce the buttons for two reasons. First, the buttons have a high frequency contact bounce that leads to buttons being read ad pressed multiple times. Second, it needs to be implemented such that if somebody presses and holds the button, the same tone just plays. We plan on doing extensive research on debouncing and implement it in hardware first. If it is not good enough, we'll also implement software fixes. 

<p align="center">
<img src="alpha.png" class="inline" width="500"/> 
</p>

The significance of the alpha prototype, however, is that we fully developed what we wish to accomplish with the custom PCB. They are listed in the next section. 

### Revised Goals
From this point forwards, our goals for this project have slightly shifted from the initially stated milestones. We wish for Drüe to have the following features:
1. **Music:** implement an algorithm to make Drüe sound like an actual piano
2. **Power Supply:** needs boost converter for 12V LED strips, but we also want to make it rechargeable 
3. **Keys:** we want to use as keys that are as piano-like as possible and need to implement debouncing (hardware first, then software if needed) 
4. **LEDs:** LED strips to light up keys, mode led indicators, and a display of some sort (not necessarily LCD because we want to shift our focus to the other goals) 
5. Will likely need an IO expander chip to have enough outputs 

## Beta Prototype
### Custom PCB
For the beta prototype, we focused a lot on designing the custom PCB so that we could place an order with all the parts and the board itself in time. [The schematic and PCB are documented here.](https://drive.google.com/file/d/1xk1kX1hMNSZrc-SdiFMO_hBEZa87qssB/view?usp=sharing)

<p align="center">
<img src="drue pcb.PNG" class="inline" width="500"/> 
</p>

### Piano Sound Replication
We researched Karplus-Strong Algorithm and FM Synthesis for producing a piano sound, but found that neither were close enough. For this reason, we will likely go with the sampling route where audio files of a piano will be played from an SD Card. To keep our options open, however, we are maintaining a PWM output along with the AnalogOut output. For this week's demo, a PWM signal will still be used; however, a better speaker and an amplifier circuit were implemented. 

### Software (Demo) 
We temporarily removed the "wack-a-mole" functionality to focus on implementing debouncing and driving an I2C IO expander. On the front of the debouncing, we implemented a simple RC hardware fix first. The idea behind this fix is that the button input has to go through an RC circuit and charge the capcitor before the microcontroller receives the signal. This makes the random noise when pressing a button unable to reach the microcontroller. On the software side, we made it so the piano keys could only be pressed one at a time. That is, the keys are now dependent. 

For the I2C IO expander, we are working on getting them to light up representative LEDs. For the final project, it will be driving mosfets that control the LED strips on the keys. 

<p align="center">
<img src="beta.png" class="inline" width="500"/> 
</p>

The goal for the baseline demo is to have the LEDs working and Whac-A-Mole coded to work with these LEDs. In addition, we'd like some sort of display added as well, be it an 8888 bar led display or a full SPI screen. 

## Baseline Demo
For the baseline demo, we were able to get the I2C I/O Expander working to light up the piano key's indicator LEDs. In addition, Whac-A-Mole was programmed and playing piano sounds from an SD card was prototyped. We also experimented with different types of amplifier circuits for the Baseline Demo to see what types of results we could achieve. The breadboard did not change significantly, so a picture of the PCB that came in is included below: 

<p align="center">
<img src="unpopulatedPCB.jpg" class="inline" width="500"/> 
</p>

### Reach Goals
At this point, there are 6 directions the project needs to move in to achieve our goal of a polished product. While realistically these won't all be achieved by the reach demo on Wednesday, the product will continue to be worked on all the way up to the public demo. 
1. An SPI screen needs to be implemented to make scrolling through menus more intuitive. The screen would also help with displaying the scrore in Whac-A-Mole or even adding a feature where notes fall on the screen before needing to be pressed (like with guitar hero). 
2. A more complicated scoring system that uses timers. Right now the scoring is just 1 point for 1 correct press. A simple improvement would be deducting points for incorrect presses and timing the game. A more complicated system could award points based on response time. 
3. The PCB needs to be soldered so work on the project will move to that instead. 
4. The "learn a piano" mode should be expanded to include more songs. 
5. An enclosure needs to be designed for the piano. 
6. Playing piano sounds needs to be designed/tuned better. 

## Reach Demo
For the Reach Demo, we decided to focus on points 3 and 5 in order to present a project that would look more like what we would finally end up going for. On the PCB side, there were quickly power supply problems. 

### Power Supply Problems
First, we were planning on using Lipo batteries, but our lab placed a ban on those. Thus, we had to look for new power sources. For the time being, we decided to use power supplies. However, we didn't realize that the 5V output on the mbed is from the uSB cable and not stepped down from this power supply. As a result, we still need to connect a USB input in order to power the amplifier. Finally, there is a lot of noise from the speaker that we can't attribute to any source, unless both the usb power and external power are plugged in. 

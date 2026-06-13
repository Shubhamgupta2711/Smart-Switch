# Smart Day/Night Automated  Timer Switch
### 1. Purpose of this Project
This project is an intelligent, micro-controller-based electrical switch system designed for building automation. It resolves daily physcialy ON/ OFF task.
By installing this system you automate all Light also it save energy and time.

### 2. System Features
#### • [Dusk-to-Dawn Library:](https://www.arduinolibraries.info/libraries/dusk2-dawn) 
Automates  dynamically using geographic coordinates (Latitude/Longitude) to match solar shifts across summer and winter cycles.
##### •	Non-volatile Timekeeping:
Driven by an I2C-connected DS3231 Real-Time Clock (RTC) chip equipped with an independent coin-cell backup battery, ensuring zero time loss during power grid failures.

### 3. Hardware Requirements & Pin Mapping
Component Inventory
1.	Micro-controller: Arduino Uno, Nano, or Pro Micro (ATmega328P or compatible)
2.	RTC Module: DS3231 Real-Time Clock
3.	Relay: 5V  Relay module
4.	Power Supply: 5V DC

<img width="4080" height="2304" alt="IMG20260519104719" src="https://github.com/user-attachments/assets/b0152013-f8fa-490f-92a4-5f26184b747e" />


### Hardware Connections

Component	Module Pin &emsp; - &emsp;	Arduino Pin
* DS3231 RTC  &emsp; - &emsp;	VCC	5V
* GND   &emsp; - &emsp;	GND
* SDA	 &emsp;- &emsp; A4 (SDA)
* SCL &emsp;- &emsp;	A5 ( SCL)

 4-Channel Relay Board
* 	VCC &emsp; - &emsp;	5V
*	GND &emsp;- &emsp;	GND
*	IN1 &emsp;- &emsp;	D2
*	IN2 &emsp;- &emsp;	D3
*	IN3 &emsp;- &emsp;	D4
*	IN4 &emsp;- &emsp;	D5
###<img width="4080" height="2304" alt="Picsart_26-05-19_12-00-39-885" src="https://github.com/user-attachments/assets/cce3f4e3-ec58-479d-8460-791d0b13b489" />
 

## 4. Software Implementation
Dependencies
The codebase requires the installation of the following libraries through the Arduino IDE Library Manager:

* <DS3231.h>
*	[<Dusk2Dawn.h>](https://www.arduinolibraries.info/libraries/dusk2-dawn)

## Code
* [Smart_switch_code](smart_Switch_code)

### 5.	Logical Analysis & Core Calculations
The code uses a logical OR (||) operator. This means the relay will turn ON if either of the two conditions is true:
 	
## Why is it written this way? 
Standard time tracking resets to 0 at midnight. If you want a device to run from 10:00 PM (Ontime1 = 1320) to 6:00 AM (Offtime1 = 360), a simple totalMin >= Ontime1 && totalMin < Offtime1 will fail because a number cannot be both greater than 1320 and less than 360 at the same time.
For the relay to turn ON, the current time (totalMin) must meet both conditions simultaneously:
 
## Why this is used for Daytime Schedules
This is the standard way to program a timer that starts and stops on the same calendar day.
For example, if you want a device to run from 8:00 AM (Ontime2 = 480) to 4:00 PM (Offtime2 = 960):
•	If the current time is 12:00 PM (720 minutes), it is greater than 480 AND less than 960. Both conditions are true, so the relay turns ON.
•	If it's 6:00 PM (1080 minutes), it's greater than 480, but not less than 960. One condition fails, so the relay turns OFF.



## Astronomical Shift Matrix
The update dynamically inside the main loop every 24 hours. The library uses the calendar date matrix parameters to shift values smoothly along the following estimated annual curves for the Howrah/Kolkata area:
•	Summer (June Equinox): Sunset computes late (~18:24). The algorithm delays activation until roughly 18:09 and schedules morning cutoff at 05:06.
•	Winter (December Solstice): Sunset computes early (~17:12). Activation advances automatically to 16:57 and extends morning illumination to 06:48.




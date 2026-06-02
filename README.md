# Smart Day/Night Automated  Timer Switch
1. Purpose of this Project
This project is an intelligent, micro-controller-based electrical switch system designed for building automation. It resolves daily physcialy ON/ OFF task.
By installing this system you automate all Light also it save energy.

2. System Features
•	Astronomical Dusk-to-Dawn Control: Automates  dynamically using geographic coordinates (Latitude/Longitude) to match solar shifts across summer and winter cycles.
•	Non-volatile Timekeeping: Driven by an I2C-connected DS3231 Real-Time Clock (RTC) chip equipped with an independent coin-cell backup battery, ensuring zero time loss during power grid failures.

3. Hardware Requirements & Pin Mapping
Component Inventory
1.	Micro-controller: Arduino Uno, Nano, or Pro Micro (ATmega328P or compatible)
2.	RTC Module: DS3231 Real-Time Clock
3.	Relay: 5V  Relay module
4.	Power Supply: 5V DC 
Hardware Connections

Component	Module Pin	Arduino Pin
DS3231 RTC	VCC	5V
	GND	GND
	SDA	A4 (SDA)
	SCL	A5 ( SCL)
4-Channel Relay Board	VCC	5V
	GND	GND
	IN1	D2
	IN2	D3
	IN3	D4
	IN4	D5

 

4. Software Implementation
Dependencies
The codebase requires the installation of the following libraries through the Arduino IDE Library Manager:
•	<DS3231.h> 
•	<Dusk2Dawn.h>
5.	Logical Analysis & Core Calculations

The code uses a logical OR (||) operator. This means the relay will turn ON if either of the two conditions is true:
 	
Why is it written this way? 
Standard time tracking resets to 0 at midnight. If you want a device to run from 10:00 PM (Ontime1 = 1320) to 6:00 AM (Offtime1 = 360), a simple totalMin >= Ontime1 && totalMin < Offtime1 will fail because a number cannot be both greater than 1320 and less than 360 at the same time.
For the relay to turn ON, the current time (totalMin) must meet both conditions simultaneously:
 
Why this is used for Daytime Schedules
This is the standard way to program a timer that starts and stops on the same calendar day.
For example, if you want a device to run from 8:00 AM (Ontime2 = 480) to 4:00 PM (Offtime2 = 960):
•	If the current time is 12:00 PM (720 minutes), it is greater than 480 AND less than 960. Both conditions are true, so the relay turns ON.
•	If it's 6:00 PM (1080 minutes), it's greater than 480, but not less than 960. One condition fails, so the relay turns OFF.



Astronomical Shift Matrix
The update dynamically inside the main loop every 24 hours. The library uses the calendar date matrix parameters to shift values smoothly along the following estimated annual curves for the Howrah/Kolkata area:
•	Summer (June Equinox): Sunset computes late (~18:24). The algorithm delays activation until roughly 18:09 and schedules morning cutoff at 05:06.
•	Winter (December Solstice): Sunset computes early (~17:12). Activation advances automatically to 16:57 and extends morning illumination to 06:48.




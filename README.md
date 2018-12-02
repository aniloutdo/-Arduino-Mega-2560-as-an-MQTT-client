# MQTT-Arduino-MEGA-I-O

Written by Peter Illmayer

## Description

Uses Arduino Mega 2560 as an MQTT client for Digital Inputs and Outputs.

When an input is open/closed an MQTT message is sent to a broker

Outputs can be controlled by MQTT messages

This code should be considered ALPHA and use at your own risk.  The code is here to keep as a backup.  There is considerable cleanup to be done however it is functional.  I'm not a programmer but get a kick out of I0T MQTT devices.

Lots of debug available on the serial port.

## MQTT messages

1. When the unit powers up it will send all of its inputs via MQTT.  If outputs have a retain flag set, the output will follow the MQTT retain flag

2. Inputs: When the pin is grounded the MQTT payload is 0, when open the payload is 1.  Topic can be customised via panelID value
	
   - Inputs will automatically send status on pin toggle

   - stat/panelId/Input/1 0 for pin shorted to ground

   - stat/panelId/Input/1 1 for pin open circuit (has pullups enabled)

3. Outputs: When a payload 1 is received the output goes LOW, payload 1 sets output HIGH. panelID currently set to 20(iD)

   - cmnd/panelId/Output/1 1  this will turn output 1 on
   
   - cmnd/panelId/Output/1 0  this will turn output 1 off

4. When an output is turned on, a response is sent back to the broker so you know the output turned on (great for NodeRed)

   - Received message: cmnd/panelId/Output/1 0
   
   - Reply message: stat/panelId/Output/1 0

5. Rudimentry LWT is implemented

   - tele/panelId Connected-Disconnected
   
   - On connected, board sends all of the input statuses
   
6. Send TOPIC cmnd/panelID/reboot 0 and it will reboot the arduino.  tele/panelID will populate payload with rebooting.
   
## Pin Mapping

   - A0-A7, A8-A15, D40-D47 are inputs
   
   - D16-D23, D24-D31, D32-D39 are outputs


## Known Bugs: 

Input A0 always reports status on boot, need to find that

Considerable load testing of turning 8 outputs on and off very rapidly causes the arduino to reboot.  Not sure if this is bloated code or a buffer overflow in the MQTT client.  This kind of rapid output switching would not occur in a home automation project

## Acknowledgements:

In particular inspiration provided by Jonathan Oxer from Superhouse.  This code was based on his LIghtswitchMQTT controller. 



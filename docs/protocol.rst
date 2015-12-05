Protocol
========

The protocol used between the Gateway and the Controller is a simple semicolon
separated list of values. The last part of each "command" is the payload.
All commands ends with a newline. The serial commands has the following format:

    ::
    
    <node-id>;<child-sensor-id>;<message-type>;<ack>;<sub-type>;<payload>\n

Message Structure Elements
**************************

================  =======================================================================================================================
Message Part      Description
================  =======================================================================================================================
node-id           The unique id of the node that sends or should receive the message (address)
child-sensor-id   Each node can have several sensors attached. This is the child-sensor-id that uniquely identifies one attached sensor
message-type      | Type of message sent - See table below ack
                  | The ack parameter has the following meaning:
                  | Outgoing: 0 = unacknowledged message, 1 = request ack from destination node
                  | Incoming: 0 = normal message, 1 = this is an ack message
sub-type          Depending on messageType this field has different meaning. See tables below
payload           The payload holds the message coming in from sensors or instruction going out to actuators.
================  =======================================================================================================================
    
.. warning::

    The maximum payload size is 25 bytes!

    The NRF24L01+ has a maximum of 32 bytes. The MySensors library (version 1.5) uses 7 bytes for the message header.

Message Types
*************

============= ===== ====================================================
Type	      Value	Comment
============= ===== ====================================================
presentation  0     Sent by a node when they present attached sensors. This is usually done in setup() at startup.
set	          1     This message is sent from or to a sensor when a sensor value should be updated.
req	          2     Requests a variable value (usually from an actuator destined for controller).
internal	  3     This is a special internal message. See table below for the details.
stream	      4     Used for OTA firmware updates.
============= ===== ====================================================

Message Sub-types
*****************


.. note::

    If you feel that these sub-types don't fit your needs, there are some
    custom varaibles that you can use. However, you think that people maybe
    have the same issue, you can modify the liberary and add your variable,
    then, pull your modifications to the development branch.

Presentation
------------

When a presentation message is sent from a sensor, sub-type can one the following:

The payload of presentation message will be set to the library version (node device) or an optional description for the sensors.

=======================   ====== ========================================= ====================
Type	                  Value  Comment	                               Variables
=======================   ====== ========================================= ====================
S_DOOR                    0      Door and window sensors                   V_TRIPPED, V_ARMED
S_MOTION                  1      Motion sensors                            V_TRIPPED, V_ARMED
S_SMOKE                   2      Smoke sensor                              V_TRIPPED, V_ARMED
S_LIGHT                   3      Light Actuator (on/off)                   V_STATUS (or V_LIGHT), V_WATT
S_BINARY                  3      Binary device (on/off), Alias for S_LIGHT V_STATUS (or V_LIGHT), V_WATT
S_DIMMER                  4      Dimmable device of some kind              V_STATUS, V_DIMMER, V_WATT
S_COVER                   5      Window covers or shades                   V_UP, V_DOWN, V_STOP, V_PERCENTAGE
S_TEMP                    6      Temperature sensor                        V_TEMP, V_ID
S_HUM                     7      Humidity sensor                           V_HUM
S_BARO                    8      Barometer sensor (Pressure)               V_PRESSURE, V_FORECAST
S_WIND                    9      Wind sensor                               V_WIND, V_GUST
S_RAIN                    10     Rain sensor                               V_RAIN, V_RAINRATE
S_UV                      11     UV sensor                                 V_UV
S_WEIGHT                  12     Weight sensor for scales etc.             V_WEIGHT, V_IMPEDANCE
S_POWER                   13     Power measuring device, like power meters V_WATT, V_KWH
S_HEATER                  14     Heater device                             V_HVAC_SETPOINT_HEAT, V_HVAC_FLOW_STATE, V_TEMP
S_DISTANCE                15     Distance sensor                           V_DISTANCE, V_UNIT_PREFIX
S_LIGHT_LEVEL             16     Light sensor                              V_LIGHT_LEVEL, V_LEVEL
S_ARDUINO_NODE            17     Arduino node device
S_ARDUINO_REPEATER_NODE   18     Arduino repeating node device
S_LOCK                    19     Lock device                               V_LOCK_STATUS
S_IR                      20     Ir sender/receiver device                 V_IR_SEND, V_IR_RECEIVE
S_WATER                   21     Water meter                               V_FLOW, V_VOLUME
S_AIR_QUALITY             22     Air quality sensor e.g. MQ-2              V_LEVEL, V_UNIT_PREFIX
S_CUSTOM                  23     | Use this for custom sensors where no
                                 | other fits.
S_DUST                    24     Dust level sensor                         V_LEVEL, V_UNIT_PREFIX
S_SCENE_CONTROLLER        25     Scene controller device                   V_SCENE_ON, V_SCENE_OFF
S_RGB_LIGHT               26     RGB light                                 V_RGB, V_WATT
S_RGBW_LIGHT              27     RGBW light                                V_RGBW, V_WATT
S_COLOR_SENSOR            28     Color sensor                              V_RGB
S_HVAC                    29     Thermostat/HVAC device                    V_HVAC_SETPOINT_HEAT, V_HVAC_SETPOINT_COLD, V_HVAC_FLOW_STATE, V_HVAC_FLOW_MODE, V_HVAC_SPEED
S_MULTIMETER              30     Multimeter device                         V_VOLTAGE, V_CURRENT, V_IMPEDANCE
S_SPRINKLER               31     Sprinkler device                          V_STATUS, V_TRIPPED
S_WATER_LEAK              32     Water leak sensor                         V_TRIPPED, V_ARMED
S_SOUND                   33     Sound sensor                              V_LEVEL(dB), V_TRIPPED, V_ARMED
S_VIBRATION               34     Vibration sensor                          V_LEVEL(Hz), V_TRIPPED, V_ARMED
S_MOISTURE                35     Moisture sensor                           V_LEVEL, V_TRIPPED, V_ARMED
S_INFO                    36     LCD text device                           V_TEXT
S_GAS                     37     Gas meter                                 V_FLOW, V_VOLUME
S_GPS                     38     GPS Sensor                                V_POSITION
=======================   ====== ========================================= ====================


Set & Req
---------

When a set or request message is being sent, the sub-type has to be one of the following:

=======================   ====== ========================================= ====================
Type	                  Value  Comment	                               Used by
=======================   ====== ========================================= ====================
V_TEMP                    0      Temperature                               S_TEMP, S_HEATER, S_HVAC
V_HUM                     1      Humidity                                  S_HUM
V_STATUS                  2      Binary status. 0=off 1=on                 S_LIGHT, S_DIMMER, S_SPRINKLER, S_HVAC, S_HEATER
V_PERCENTAGE              3      Percentage value. 0-100(%)                S_DIMMER
V_PRESSURE                4      Atmospheric Pressure                      S_BARO
V_FORECAST                5      | Whether forecast. One of "stable",      S_BARO
                                 | "sunny", "cloudy", "unstable",
                                 | "thunderstorm" or "unknown"	
V_RAIN                    6      Amount of rain                            S_RAIN
V_RAINRATE                7      Rate of rain                              S_RAIN
V_WIND                    8      Windspeed                                 S_WIND
V_GUST                    9      Gust                                      S_WIND
V_DIRECTION               10     Wind direction                            S_WIND
V_UV                      11     UV light level                            S_UV
V_WEIGHT                  12     Weight (for scales etc)                   S_WEIGHT
V_DISTANCE                13     Distance                                  S_DISTANCE
V_IMPEDANCE               14     Impedance value                           S_MULTIMETER, S_WEIGHT
V_ARMED                   15     | Armed status of a security sensor.      S_DOOR, S_MOTION, S_SMOKE, S_SPRINKLER, S_WATER_LEAK, S_SOUND, S_VIBRATION, S_MOISTURE
                                 | 1=Armed, 0=Bypassed
V_TRIPPED                 16     | Tripped status of a security sensor.    S_DOOR, S_MOTION, S_SMOKE, S_SPRINKLER, S_WATER_LEAK, S_SOUND, S_VIBRATION, S_MOISTURE
                                 | 1=Tripped, 0=Untripped
V_WATT                    17     Watt value for power meters               S_POWER, S_LIGHT, S_DIMMER, S_RGB, S_RGBW
V_KWH                     18     | Accumulated number of KWH for a         S_POWER
                                 | power meter
V_SCENE_ON                19     Turn on a scene                           S_SCENE_CONTROLLER
V_SCENE_OFF               20     Turn off a scene                          S_SCENE_CONTROLLER
V_HVAC_FLOW_STATE         21     | Mode of header. One of "Off",
                                 | "HeatOn", "CoolOn", or "AutoChangeOver" S_HVAC, S_HEATER
V_HVAC_SPEED              22     | HVAC/Heater fan speed                   S_HVAC, S_HEATER
                                 | ("Min", "Normal", "Max", "Auto")
V_LIGHT_LEVEL             23     | Uncalibrated light level. 0-100%.       S_LIGHT_LEVEL
                                 | Use V_LEVEL for light level in lux.
V_VAR1                    24     Custom value                              Any device
V_VAR2                    25     Custom value                              Any device
V_VAR3                    26     Custom value                              Any device
V_VAR4                    27     Custom value                              Any device
V_VAR5                    28     Custom value                              Any device
V_UP                      29     Window covering. Up.                      S_COVER
V_DOWN                    30     Window covering. Down.                    S_COVER
V_STOP                    31     Window covering. Stop.                    S_COVER
V_IR_SEND                 32     Send out an IR-command                    S_IR
V_IR_RECEIVE              33     | This message contains a received        S_IR
                                 | IR-command
V_FLOW                    34     Flow of water (in meter)                  S_WATER
V_VOLUME                  35     Water volume                              S_WATER
V_LOCK_STATUS             36     | Set or get lock status.                 S_LOCK
                                 | 1=Locked, 0=Unlocked	
V_LEVEL                   37     Used for sending level-value              S_DUST, S_AIR_QUALITY, S_SOUND (dB), S_VIBRATION (hz), S_LIGHT_LEVEL (lux)
V_VOLTAGE                 38     Voltage level                             S_MULTIMETER
V_CURRENT                 39     Current level                             S_MULTIMETER
V_RGB                     40     | RGB value transmitted as ASCII          S_RGB_LIGHT, S_COLOR_SENSOR
                                 | hex string (I.e "ff0000" for red)	
V_RGBW                    41     | RGBW value transmitted as ASCII         S_RGBW_LIGHT 
                                 | hex string 
V_ID                      42     | Optional unique sensor id               S_TEMP
                                 | (e.g. OneWire DS1820b ids)
V_UNIT_PREFIX             43     | Allows sensors to send in a string      S_DISTANCE, S_DUST, S_AIR_QUALITY
                                 | representing the unit prefix to be
                                 | displayed in GUI. This is not parsed by
                                 | controller! E.g. cm, m, km, inch.	
V_HVAC_SETPOINT_COOL      44     HVAC cold setpoint                        S_HVAC
V_HVAC_SETPOINT_HEAT      45     HVAC/Heater setpoint                      S_HVAC, S_HEATER
V_HVAC_FLOW_MODE          46     | Flow mode for HVAC                      S_HVAC
                                 | ("Auto", "ContinuousOn", "PeriodicOn")
V_TEXT                    47     Text message to display on LCD or         S_INFO
                                 | controller device
V_CUSTOM                  48     Custom messages used for controller       S_CUSTOM
                                 | /inter node specific commands, 
                                 | preferably using S_CUSTOM device type.
V_POSITION                49     GPS position and altitude. Payload:       S_GPS
                                 | latitude;longitude;altitude(m). 
                                 | E.g. "55.722526;13.017972;18"
V_IR_RECORD               50     Record IR codes S_IR for playback         S_IR
=======================   ====== ========================================= ====================

Internal
--------

The internal messages are used for different tasks in the communication between sensors, the gateway to controller and between sensors and the gateway.

When an internal messages is sent, the sub-type has to be one of the following:

=======================   ====== =========================================
Type	                  Value  Comment	                              
=======================   ====== =========================================
I_BATTERY_LEVEL           0      Use this to report the battery level (in percent 0-100).
I_TIME                    1      Sensors can request the current time from the Controller using this message. The time will be reported as the seconds since 1970
I_VERSION                 2      Used to request gateway version from controller.
I_ID_REQUEST              3      Use this to request a unique node id from the controller.
I_ID_RESPONSE             4      Id response back to sensor. Payload contains sensor id.
I_INCLUSION_MODE          5      Start/stop inclusion mode of the Controller (1=start, 0=stop).
I_CONFIG                  6      Config request from node. Reply with (M)etric or (I)mperal back to sensor.
I_FIND_PARENT             7      When a sensor starts up, it broadcast a search request to all neighbor nodes. They reply with a I_FIND_PARENT_RESPONSE.
I_FIND_PARENT_RESPONSE    8      Reply message type to I_FIND_PARENT request.
I_LOG_MESSAGE             9      Sent by the gateway to the Controller to trace-log a message
I_CHILDREN                10     A message that can be used to transfer child sensors (from EEPROM routing table) of a repeating node.
I_SKETCH_NAME             11     Optional sketch name that can be used to identify sensor in the Controller GUI
I_SKETCH_VERSION          12     Optional sketch version that can be reported to keep track of the version of sensor in the Controller GUI.
I_REBOOT                  13     Used by OTA firmware updates. Request for node to reboot.
I_GATEWAY_READY           14     Send by gateway to controller when startup is complete.
I_REQUEST_SIGNING         15     Used between sensors when initialting signing.
I_GET_NONCE               16     Used between sensors when requesting nonce.
I_GET_NONCE_RESPONSE      17     Used between sensors for nonce response.
I_HEARTBEAT               18
I_PRESENTATION            19
I_DISCOVER                20
I_DISCOVER_RESPONSE       21
=======================   ====== =========================================

Stream
------

============================   ====== =========================================
Type                           Value  Comment	                              
============================   ====== =========================================
ST_FIRMWARE_CONFIG_REQUEST     0
ST_FIRMWARE_CONFIG_RESPONSE    1
ST_FIRMWARE_REQUEST            2
ST_FIRMWARE_RESPONSE           3
ST_SOUND                       4      Used to transfer sound to controller
ST_IMAGE                       5      Used to transfer image to controller
============================   ====== =========================================

Examples
********

Received message from radio network from one of the sensors: Incoming presentation
message from node 12 with child sensor 6. The presentation is for a binary
light S_LIGHT. The payload holds a description of the sensor.
Gateway passes this over to the controller.

::

    12;6;0;0;3;My Light\n

Received message from radio network from one of the sensors: Incoming
temperature V_TEMP message from node 12 with child sensor 6. The gateway
passed this over to the controller.

::

    12;6;1;0;0;36.5\n

Received command from the controller that should be passed to radio network:
Outgoing message to node 13. Set V_LIGHT variable to 1 (=turn on) for child
sensor 7. No ack is requested from destination node.

::

    13;7;1;0;2;1\n

.. note::

    There are some messages which are processed by the MySensors library.
    Which means that you don't have to implement an action for them.

    E.g.: I_REBOOT.

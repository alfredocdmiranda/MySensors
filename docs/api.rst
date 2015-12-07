Api
===

In this page you will find about the functions you must implement and the 
objects you may use on your sketches.

Functions
*********

presentation
^^^^^^^^^^^^

:code:`void presentation()`

This function must be implemented (at least present) in your sketch. It will be 
called at the start of your node and every time your nome receive an 
I_PRESENTATION.

@params: None

@return: None

receive
^^^^^^^

:code:`void receive(const MyMessage &msg)`

If you want to handle messages you must implement this function on your sketch. 
If you don't implement it, you won't be able to handle the messages and they will 
just be ignored, if they are not "special" messages which are handled behind 
the scenes.

Then, you will use the MyMessage object to see what is inside the message.

@params:

- msg: It takes a reference of a MyMessage that you will to manipulate.

@return: None

receiveTime
^^^^^^^^^^^

:code:`void receiveTime(unsigned long time)`

This function will be called every time your node receives time from controller. 
So if you want to handle it, you must implement this functions.

@params:

- time:

@return: None

getConfig
^^^^^^^^^

:code:`ControllerConfig getConfig()`

It takes the most recent node configuration received from controller

@params: None

@return: ControllerConfig struct with all configuration

getNodeId
^^^^^^^^^

:code:`uint8_t getNodeId()`

@params: None

@return: The node's id

loadState
^^^^^^^^^

:code:`uint8_t loadState(uint8_t pos)`

Load a state (from local EEPROM).

@params:

- pos: The position to fetch value from  (0-255)

@return: Value to store in position

present
^^^^^^^

:code:`void present(uint8_t sensorId, uint8_t sensorType, const char* 
description="", bool ack=false)`

Each node must present all attached sensors before any values can be handled correctly by the controller.
It is usually good to present all attached sensors after power-up in setup().

@params:

- sensorId: Select a unique sensor id for this sensor. Choose a number between 0-254.
- sensorType: The sensor type. See sensor typedef in MyMessage.h.
- description: A textual description of the sensor.
- ack: Set this to true if you want destination node to send ack back to this node. 
  Default is not to request any ack.
- description: A textual description of the sensor.

request
^^^^^^^

:code:`void request(uint8_t childSensorId, uint8_t variableType, uint8_t 
destination=GATEWAY_ADDRESS)`

@params:

- childSensorId: The variable's node's id you want to request.
- variableType: The type of variable you are requesting.
- destination: Destination node. The default destination is the Gateway.

@return: None
 
requestTime
^^^^^^^^^^^

:code:`void requestTime()`

Requests time from controller. Answer will be delivered to receiveTime function 
in sketch.

@params: None

@return: None

saveState
^^^^^^^^^

:code:`void saveState(uint8_t pos, uint8_t value)`

Save a state (in local EEPROM). Good for actuators to "remember" state between
power cycles.
You have 256 bytes to play with. Note that there is a limitation on the number
of writes the EEPROM can handle (~100 000 cycles on ATMega328).

@params:

- pos: The position to store value in (0-255)
- value: Value to store in position

@return: None

send
^^^^

:code:`bool send(MyMessage &msg, bool ack=false)`

Sends a message to gateway or one of the other nodes in the radio network

@params:

- msg: It takes a reference to a Message object to send.
- ack: Set this to true if you want destination node to send ack back to this 
  node. Default is not to request any ack.

@return: Returns true if message reached the first stop on its way to 
destination.

sendBatteryLevel
^^^^^^^^^^^^^^^^

:code:`void sendBatteryLevel(uint8_t level, bool ack=false)`

Send this nodes battery level to gateway.

@params:

- level: Level between 0-100(%)
- ack: Set this to true if you want destination node to send ack back to this 
  node. Default is not to request any ack.

@return: None

sendHeartbeat
^^^^^^^^^^^^^

:code:`void sendHeartbeat()`

Send a heartbeat message (I'm alive!) to the gateway/controller.
The payload will be an incremental 16 bit integer value starting at 1 when 
sensor is powered on.

Allows node to send heartbeat and controller to ping nodes.

@params: None

@return: None

sendSketchInfo
^^^^^^^^^^^^^^

:code:`void sendSketchInfo(const char* name, const char* version, 
bool ack=false)`

It sends sketch meta information to the gateway. Not mandatory but a nice thing 
to do.

@params:

- name String containing a short Sketch name or NULL  if not applicable
- version String containing a short Sketch version or NULL if not applicable
- ack Set this to true if you want destination node to send ack back to this 
  node. Default is not to request any ack.

@return: None

sleep
^^^^^

:code:`void sleep(unsigned long ms)`

Sleep (PowerDownMode) the MCU and radio. Wake up on timer.

@params:

- ms: Number of milliseconds to sleep.

@return: None

smartSleep
^^^^^^^^^^

:code:`void smartSleep(unsigned long ms)`

@params:

- ms: Number of milliseconds to sleep.

sleep
^^^^^

:code:`bool sleep(uint8_t interrupt, uint8_t mode, unsigned long ms=0)`

Sleep (PowerDownMode) the MCU and radio. Wake up on timer or pin change.
See: http://arduino.cc/en/Reference/attachInterrupt for details on modes and which pin
is assigned to what interrupt. On Nano/Pro Mini: 0=Pin2, 1=Pin3

@params:

- interrupt: Pin that should trigger the wakeup
- mode: RISING, FALLING, CHANGE
- ms: Number of milliseconds to sleep or 0 to sleep forever

@return: True if wake up was triggered by pin change and false means timer woke 
it up.

smartSleep
^^^^^^^^^^

:code:`bool smartSleep(uint8_t interrupt, uint8_t mode, unsigned long ms=0)`

@params:

- interrupt: Pin that should trigger the wakeup
- mode: RISING, FALLING, CHANGE
- ms: Number of milliseconds to sleep or 0 to sleep forever

@return: True if wake up was triggered by pin change and false means timer woke 
it up.

sleep
^^^^^

:code:`int8_t sleep(uint8_t interrupt1, uint8_t mode1, uint8_t interrupt2, 
uint8_t mode2, unsigned long ms=0)`

Sleep (PowerDownMode) the MCU and radio. Wake up on timer or pin change for two separate interrupts.
See: http://arduino.cc/en/Reference/attachInterrupt for details on modes and which pin
is assigned to what interrupt. On Nano/Pro Mini: 0=Pin2, 1=Pin3

@params:

- interrupt1 First interrupt that should trigger the wakeup
- mode1 Mode for first interrupt (RISING, FALLING, CHANGE)
- interrupt2 Second interrupt that should trigger the wakeup
- mode2 Mode for second interrupt (RISING, FALLING, CHANGE)
- ms Number of milliseconds to sleep or 0 to sleep forever

@return: Pin number wake up was triggered by pin change and negative if 
timer woke it up.

smartSleep
^^^^^^^^^^

:code:`int8_t smartSleep(uint8_t interrupt1, uint8_t mode1, uint8_t interrupt2, 
uint8_t mode2, unsigned long ms=0)`

@params:

- interrupt1 First interrupt that should trigger the wakeup
- mode1 Mode for first interrupt (RISING, FALLING, CHANGE)
- interrupt2 Second interrupt that should trigger the wakeup
- mode2 Mode for second interrupt (RISING, FALLING, CHANGE)
- ms Number of milliseconds to sleep or 0 to sleep forever

@return: Pin number wake up was triggered by pin change and negative if 
timer woke it up.

wait
^^^^

:code:`void wait(unsigned long ms)`

Wait for a specified amount of time to pass.  Keeps process()ing.
This does not power-down the radio nor the Arduino.
Because this calls process() in a loop, it is a good way to wait
in your loop() on a repeater node or sensor that listens to messages.

@params:

- ms: Number of milliseconds to sleep.

@return: None

Objects
*******

MyMessage
^^^^^^^^^

This object will handle incoming and outcoming messages. You must create one 
message for each sensor you have in your node. 

E.g.: 

- :code:`MyMessage msg1(CHILD_ID, CHILD_TYPE);`
- :code:`MyMessage msg2();`

Attributes
----------

:code:`uint8_t last`
    
    8 bit - Id of last node this message passed

:code:`uint8_t sender`

    8 bit - Id of sender node (origin)

:code:`uint8_t destination`
    
    8 bit - Id of destination node

:code:`uint8_t version_length`
    
    2 bit - Protocol version
    
    1 bit - Signed flag
    
    5 bit - Length of payload

:code:`uint8_t command_ack_payload`
    
    3 bit - Command type
    
    1 bit - Request an ack - Indicator that receiver should send an ack back.
    
    1 bit - Is ack messsage - Indicator that this is the actual ack message.
    
    3 bit - Payload data type

:code:`uint8_t type`
    
    8 bit - Type varies depending on command

:code:`uint8_t sensor`

    8 bit - Id of sensor that this message concerns.

:code:`char data[MAX_PAYLOAD + 1];`

    That is the message's payload

Methods
-------

getCommand
~~~~~~~~~~

:code:`uint8_t getCommand()`

@params: None

@return: It returns the value of command (type of message). E.g.: 
C_SET, C_REQ, ...

isAck
~~~~~

:code:`bool isAck()`

@params: None

@return: It return if it is an ack or not.

set
~~~

:code:`MyMessage& set(void* payload, uint8_t length)`

:code:`MyMessage& set(const char* value)`

:code:`MyMessage& set(float value, uint8_t decimals)`

:code:`MyMessage& set(uint8_t value)`

:code:`MyMessage& set(uint32_t value)`

:code:`MyMessage& set(int32_t value)`

:code:`MyMessage& set(uint16_t value)`

:code:`MyMessage& set(int16_t value)`

@params:

- payload:
- length:
- value:
- decimals:

@return: It returns a reference to your MyMessage object.

setDestination
~~~~~~~~~~~~~~

:code:`MyMessage& setDestination(uint8_t destination)`

@params:

- destination

@return: It returns a reference to your MyMessage object.

setSensor
~~~~~~~~~

:code:`MyMessage& setSensor(uint8_t sensor)`

@params:

- sensor

@return: It returns a reference to your MyMessage object.

setType
~~~~~~~

:code:`MyMessage& setType(uint8_t type)`

@params:

- type

@return: It returns a reference to your MyMessage object.

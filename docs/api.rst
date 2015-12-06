Api
===

In this page you will find about the functions you must implement and the 
objects you may use on your sketches.

Functions
*********

void presentation()
^^^^^^^^^^^^^^^^^^^

void receive(const MyMessage &)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to handle messages you must implement this function on your sketch. 
If you don't implement it, you won't be able to handle the messages and they will 
just be ignored, if they are not "special" messages which are handled behind 
the scenes.

Then, you will use the MyMessage object to see what is inside the message.

void sendHeartbeat()
^^^^^^^^^^^^^^^^^^^^

Allows node to send heartbeat and controller to ping nodes.

Objects
*******

MyMessage
^^^^^^^^^

This object will handle incoming and outcoming messages. You must create one 
message for each sensor you have in your node. 

E.g.: 
- `MyMessage msg1(CHILD_ID, CHILD_TYPE);`
- `MyMessage msg2();`

Attributes
----------

`uint8_t last`
    
    8 bit - Id of last node this message passed

`uint8_t sender`

    8 bit - Id of sender node (origin)

`uint8_t destination`
    
    8 bit - Id of destination node

`uint8_t version_length`
    
    2 bit - Protocol version
    
    1 bit - Signed flag
    
    5 bit - Length of payload

`uint8_t command_ack_payload`
    
    3 bit - Command type
    
    1 bit - Request an ack - Indicator that receiver should send an ack back.
    
    1 bit - Is ack messsage - Indicator that this is the actual ack message.
    
    3 bit - Payload data type

`uint8_t type`
    
    8 bit - Type varies depending on command

`uint8_t sensor`

    8 bit - Id of sensor that this message concerns.

Methods
-------

uint8_t getCommand()
~~~~~~~~~~~~~~~~~~~~

bool isAck()
~~~~~~~~~~~~

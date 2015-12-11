MySensors
==========

Welcome to MySensors' unofficial documentation. We are trying to gather
relevant information and API documentation which will help new users and
developers.

MySensors is a library built focusing on Arduino platform to make home automation
easier. There is a set of third-party libraries which complements the whole
library.

If you don't find what you want in this page, you always can go to the `MySensors' official
website <http://www.mysensors.org/>`_ and its `Forum <http://forum.mysensors.org/>`_

Protocol version 1.6

Contents:

.. toctree::
   :maxdepth: 2

   getting_started
   hardware
   bootloader
   protocol
   controllers
   nodes
   security
   api

What's new:

- Library size reduced ~20% by removing a lot of C++ overhead.
- Full configuration of library behaviours and features directly from sketch 
  code. 
- No more MySensors constructor mess with radio drivers and signing backends. 
  All setup and initialization is handled “behind-the-scene”.
- Calls to process() is handled in the background automatically.
- Structural change: Embedding of drivers and libraries in a structured way 
  instead of using the weird arduino-build-system util-folder which includes 
  everything when building.
- All gateway variants available as examples without any need for 
  re-configuration (E.g. SOFT-SPI automatically enabled for W5100).
- Gateway just another sensor node! So now you can have wired ethernet sensors 
  and wireless ESP8266 sensors without any radio attach if you want.
- AES encryption (RF24)
- Introducing a presentation() function in sketch - This allows controller to 
  re-request presentation and do re-configuring node after startup.
- Optional receive() function in sketch replaces begin(callback).
- sendHeartbeat() - Allows node to send heartbeat and controller to ping nodes.
- Ethernet/ESP8266 gateway supports setting static ip and using dhcp.
- Ethernet/ESP8266 gateway allowing communication using UDP.
- Ethernet/ESP8266 gateway can now act as a client which opens a socket to 
  controller at startup. NOTE: This has to be supported by controller.
- MQTT client gateway on ESP8266 or Arduino+W5100 Ethernet adapter.
- Serial transport layer (RS232/RS485) with dePin management.
- Added variable V_CUSTOM - For sending/requesting custom data to/from 
  controller and between nodes. Preferable using S_CUSTOM device.  This 
  variable-type is controller specific so use it with care in the officially 
  provided examples. 
- New sensors types and variables: S_INFO, S_GAS, S_GPS, V_TEXT, V_CUSTOM, 
  V_POSITION.
- New command I_DISCOVER - retrieve active nodes and topology.
- Detection of mis-wired RFM69 radios.
- MyMessage:getCommand() (get message type, e.g.: internal, stream, set, ...)
- “Smart sleep” variants of all sleep methods that allows nodes to receive 
  buffered messages/commands from controller that was issued while node was 
  sleeping. When calling the smartSleep() variant, the node immediately sends a 
  heartbeat mesage when waking up (informing controller that node is awake). 
  Before going back to sleep it waits MY_SMART_SLEEP_WAIT_DURATION to process 
  any incoming buffered messages from the controller. NOTE: Controller must 
  support this feature.



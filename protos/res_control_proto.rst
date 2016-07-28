.. _rc-proto:

Resouce Control Protocol
===============================================================================

The main purpose of :ref:`ra-proto` is for handling the metadata of resource.
We also need some control signals for *changing the state of resource*. This
signal was usually sent from the IoTtalk server. The state we concerned will
be:

- Resource data channel connection state

- Resource data transfer state

- Resource unpluging

:state: WIP


Preamble
----------------------------------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"
in this document are to be interpreted as described in :rfc:`2119`.


Transportation Layer
----------------------------------------------------------------------

The transportation layer MUST be bidirectional.
The message can be fired by both server and client side. The passive server
model, for example HTTP, is unqualified.


Implementation Suggestion
----------------------------------------------------------------------

We RECOMMEND the Message Queuing Telemetry Transport (MQTT) or any kind of
message queue protocol can achieve the bidirectional communication.


Control Signal
----------------------------------------------------------------------

A single control signal on the transportation wire MUST be identity as a
distinct, minimal unit. It has following structure:

- Signal type

- Signal payload: this field is OPTIONAL. It depends on the signal type.

The type reveals the main behavior of the signal will achieve. And the payload
is for the required parameters about the signal.

The available types:

- Connect signal

- Disconnect signal

- Resume signal

- Suspend signal

- Shutdown signal


Connect Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This signal makes a resource setup its feature data channel with the proper
parameters. The parameters of data channel MUST be revealed in the signal
payload.


Disconnect Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This signal makes a resource reset its specific feature data channel connection
and feature data transfer state. In other world, the resource will disconnect
from data channel and disable the feature. The specific feature MUST be
revealed in the signal payload.


Resume Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This signal is sent from IoTtalk that requests a resource feature start to do
data exchange on the wire. The specific feature MUST be revealed in the signal
payload.


Suspend Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This signal aims at stop the data transfer of specific resource feature
on the wire. The specific feature MUST be revealed in the signal payload.


Shutdown Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This signal makes the resource fully shutdown. It will stop all data transfer,
disconnect all from data channel, unregister itself, then maybe do some
internal cleanup.

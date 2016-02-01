Component Discovery Protocol
===============================================================================

In a local area network, we may have one or more input/output devices.
We will introduce how to plug/remove devices here.

:state: Draft


Preamble
----------------------------------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"
in this document are to be interpreted as described in :rfc:`2199`.


Component Identity
----------------------------------------------------------------------

We use UUID to identify a component.


Transportation Layer
----------------------------------------------------------------------

The EasyPlugin Data Gateway MUST broadcast its identity on IPv4 UDP 1900.
We consider it act as a beacon. A beacon SHALL keep broadcasting in an
constant interval. Taking one to five seconds as broadcasting interval
is RECOMMENDED.


.. _cdp-protocol-grammar:

Protocol Grammar
----------------------------------------------------------------------

The following ABNF grammar defines Component Discovery Protocol::

    cd-protocol      = header *body CRLF

    header           = "EP" SP UUID SP

    body             = verb *UUID

    verb             = hello
                     / ping
                     / pong

    hello            = "HELO"
    ping             = "PING"
    pong             = "PONG"

    UUID             = 16OCTET

The sender MUST reveal its ``UUID`` in header.


The Protocol Commands
----------------------------------------------------------------------

The *verb* described in section :ref:`cdp-protocol-grammar` are considered as
commands here.


``hello`` Command
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This command SHALL be used by a beacon to reveal its ``UUID`` at the local
area network. It MUST carry a ``UUID`` refered to sender itself.


.. _cdp-ping-command:

``ping`` Command
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This command is invoked when component need to check the other known component
still alive or not.

The following message format are valid:

- ``PING UUID``
- ``PING``

The former format carry a ``UUID`` refered to destination component.
If the ``UUID`` of the receiver do not match with message ``UUID``, we MAY
drop this message without response. Otherwise, the receiver SHOULD invoke a
:ref:`cdp-pong-command`.

The later format do not make anything follow it. Any receiver SHOULD invoke
a :ref:`cdp-pong-command` to original sender. In order to make components be
discovered actively, this format is RECOMMENDED to send via broadcasting.
Or in case of discovering all component on same endpoint, same ip,
for example, this format is also RECOMMENDED.


.. _cdp-pong-command:

``pong`` Command
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This command is invoked in order to reponse the :ref:`cdp-ping-command`.

The following message format is valid:

- ``PONG UUID``

This message MUST carry a ``UUID`` referd to the source of
:ref:`cdp-ping-command`.  The receiver MAY drop this message if the carried
``UUID`` do not match with receiver itself.


Security Aspects
----------------------------------------------------------------------



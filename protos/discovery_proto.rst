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


Protocol Grammar
----------------------------------------------------------------------

The following ABNF grammar define Component Discovery Protocol::

    cd-protocol      = header *body

    header           = "EP" SP verb %x02
    verb             = hello

    hello            = "HELLO"

    body             = UUID

    UUID             = 16OCTET

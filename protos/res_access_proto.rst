Resource Access Protocol
===============================================================================

In a LAN, our device and EasyPlugin Data Gateway can known each other via
:ref:`cd-proto`. The :ref:`device application` which connects to EasyPlugin
always comes and go. We will consider them as kind of `resource`.
They provide :ref:`device feature` (s) sometimes available and sometimes not.

In order to keep those resources well-managed, this specification will
regulate the process of following:

- Resource registration(creation)
- Resource deregistration(deletion)
- Resource meta data modification
- Resource meta data retrieval


:state: Draft


Preamble
----------------------------------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"
in this document are to be interpreted as described in :rfc:`2199`.


Resource Identity
----------------------------------------------------------------------

The basic unit of a resource is a :ref:`device application`.
We will use `UUID`_ introduced in :ref:`cd-proto`.

The different process on same physical device MUST have different `UUID`_.
Because the feature on they may diverge.

.. _UUID: https://en.wikipedia.org/wiki/Universally_unique_identifier

__ UUID_


Transportation Layer
----------------------------------------------------------------------


Transportation Endpoint
----------------------------------------------------------------------


Security Aspects
----------------------------------------------------------------------

.. _ra-proto:

Resource Access Protocol
===============================================================================

In a LAN, our device and IoTtalk Data Gateway can known each other via
:ref:`cd-proto`. The :ref:`device application` which connects to IoTtalk
always comes and go. We will consider them as kind of `resource`.
They provide :ref:`device feature` (s) sometimes available and sometimes not.

In order to keep those resources well-managed, this specification will
regulate the following *process*:

- Resource registration (creation)
- Resource deregistration (deletion)
- Resource metadata modification
- Resource metadata retrieval


:state: Draft


Preamble
----------------------------------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"
in this document are to be interpreted as described in :rfc:`2119`.


.. _rap-resource-id:

Resource Identity
----------------------------------------------------------------------

The basic unit of a resource is a :ref:`device application`.
We will use `UUID`_ introduced in :ref:`cd-proto` as resource identity.

The different process on same physical device MUST have different `UUID`_.
Because the feature on they may diverge.

.. _UUID: https://en.wikipedia.org/wiki/Universally_unique_identifier


Transportation Layer
----------------------------------------------------------------------

The message delivered via the transportation layer MUST be constructed
with two part:

- Header part
- Data part

The header part MUST indicate various protocol related metadata, including:

- The kind of *process* mentioned above.
- The URI endpoint for operating.

The data part is OPTIONAL and it indicate generic data container,
including but can more than:

- The arguments for the *process*.
- Optional data field for the URI endpoint.


Implementation Suggestion
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The RECOMMENDED transportation layer implementation is HTTP.
The HTTP support is widespread, the implementation on devices will become
easier.

In header part, the *process* can be mapped to the *HTTP Verb* like *GET*,
*POST*, or *DELETE*. The HTTP URL is also easy to design.
In data part, the JSON/XML content is suitable for deliver complex
data structure.


Transportation Endpoint
----------------------------------------------------------------------

The `REST-like`_ endpoint design on this protocol is RECOMMENDED.

Each endpoint has a valid set of *verbs*.
The univeral set *verbs* is
:math:`\{ \text{GET}, \text{POST}, \text{PUT}, \text{DELETE} \}`
which is inspired by HTTP verbs.


Registration Endpoint
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This endpoint indicate the creation of a *resource*.

::

    PUT /<id>

    # body
    <metadata>

Where ``<id>`` is the UUID of a resource
mentioned in :ref:`rap-resource-id`.

Where :ref:`<metadata> <rap-metadata>` section is OPTIONAL.
The device can register first, then the :ref:`metadata <rap-metadata>`
can be added later.


Rejection
**************************************************

We SHOULD reject the registration request if the :ref:`metadata <rap-metadata>`
is not recognized.


Deregistration Endpoint
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This endpoint indicate the deletion of a *resource*.

::

    DELETE /<id>

Where ``<id>`` is the UUID of a resource
mentioned in :ref:`rap-resource-id`.


Metadata Retrieval Endpoint
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This endpoint indicates the reading of resource
:ref:`metadata <rap-metadata>`.

::

    GET /<id>/<field locator>


Where ``<id>`` is the UUID of a resource
mentioned in :ref:`rap-resource-id`.

Where ``<field locator>`` is OPTIONAL.
It indicates the selector of the data field,
its format is implementation dependent.


Metadata Modification Endpoint
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This endpoint indicates the updating of resource
:ref:`metadata <rap-metadata>`.

::

    POST /<id>/<field locator>

    <metadata>


Where ``<id>`` is the UUID of a resource
mentioned in :ref:`rap-resource-id`.

Where ``<field locator>`` is OPTIONAL.
It indicates the selector of the data field,
its format is implementation dependent.


.. _REST-like: https://en.wikipedia.org/wiki/Representational_state_transfer


Rejection
**************************************************

We SHOULD reject the registration request if the :ref:`metadata <rap-metadata>`
is not recognized.


.. _rap-metadata:

Metadata
----------------------------------------------------------------------

:accept_protos: List of supported protocols. This field is OPTIONAL.

:idf_list: *idf* stands for *Input Device Feature*.
           This field is OPTIONAL.
           It's a list of pair (*feature*, *units*).

           :Feature: It has naming convension: ``([a-z][_a-z0-9]*)+``.
           :Units: It is a list of string or null value.  We MUST infer the
                   dimension of feature from shape of unit.

:odf_list: *odf* stands for *Output Device Feature*.
           This field is OPTIONAL.
           It's a list of pair (*feature*, *units*).

           :Feature: It has naming convension: ``([a-z][_a-z0-9]*)+``.
           :Units: It is a list of string or null value.  We MUST infer the
                   dimension of feature from shape of unit.

:name: Arbitrary string, it can be consider as comment.

:owner: Arbitrary string. This field is OPTIONAL.

:profile: A JSON object for storing arbitrary data. This field is OPTIONAL.


Security Aspects
----------------------------------------------------------------------

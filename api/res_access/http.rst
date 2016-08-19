Resource Access HTTP API
===============================================================================

This specification will introduce HTTP API via REST-like design for
:ref:`ra-proto`.


Endpoints
----------------------------------------------------------------------

/<id>
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

GET /<id>
**************************************************

Retrieving the :ref:`metadata <rap-metadata>` of ``<id>``.

Request Headers:
    - `Accept`_
        - *application/json; charset=utf-8*

Response Headers:
    - `Content-Type`_
        - *application/json; charset=utf-8*

Response JSON Object:
    - id (string): The requested UUID.
    - state (*string*): The state of the resource, *online* or *offline*.

Status Codes:
    - `200 OK`_
        - UUID exists
    - `404 Not Found`_
        - The requested UUID unknown

Request::

    GET /219e0050-10e0-48dd-9b99-e196acfb30c8 HTTP/1.1
    Accept: application/json

Response::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": "219e0050-10e0-48dd-9b99-e196acfb30c8",
        "state": "ok",
        "name": "BetaCat",
        "idf_list": [["meow", ["dB"]]],
        "accept_protos": ["MQTT", "WebSocket"],
        "rev": "cc35867e-1f74-47d3-88c9-7dcc374a5919"
    }


PUT /<id>
**************************************************

Request Headers:
    - `Accept`_
        - *application/json; charset=utf-8*
    - `Content-Type`_
        - *application/json; charset=utf-8*

Response Headers:
    - `Content-Type`_
        - *application/json; charset=utf-8*

Response JSON Object:
    - id (*string*): The requested UUID.
    - state (*string*): The state of the resource, *ok* or *error*.
    - reason (*string*, optional): The error message.
    - rev (*string*): the token required by deregistration.
      It stands for *revision*.
    - url (*json*)
    - ctrl_chans (*array*):We use two mqtt topics here, in order to achieve
      bidirectional communication. The ``i`` topic denote the uplink,
      client can send control channel request via this link.
      Also, the ``o`` topic denote the downlink, server will send control
      command via this channel.

Status Codes:
    - `200 OK`_
        - UUID registration accepted.
    - `400 Bad Request`_
        - Wrong `Content-Type`.
    - `403 Forbidden`_
        - Any content of metadata is not recognized.

Request::

    PUT /219e0050-10e0-48dd-9b99-e196acfb30c8 HTTP/1.1
    Accept: application/json

    {
        "name": "BetaCat",
        "idf_list": [["meow", ["dB"]]],
        "accept_protos": ["MQTT", "WebSocket"]
    }

Response::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": "219e0050-10e0-48dd-9b99-e196acfb30c8",
        "rev": "41997b1e-2850-43b5-b4b5-309d05307bf7",
        "state": "ok",
        "url": {
            "scheme": "mqtt",
            "host": "example.org",
            "port": 1883
        },
        "ctrl_chans": [
            "219e0050-10e0-48dd-9b99-e196acfb30c8/ctrl/i",
            "219e0050-10e0-48dd-9b99-e196acfb30c8/ctrl/o"
        ]
    }

Error Response::

    HTTP/1.1 403 Forbidden
    Content-Type: application/json

    {
        "id": "219e0050-10e0-48dd-9b99-e196acfb30c8",
        "state": "error",
        "reason": "feature not supported"
    }


DELETE /<id>
**************************************************

Request Headers:
    - `Accept`_
        - *application/json; charset=utf-8*

    - `Content-Type`_
        - *application/json; charset=utf-8*

Response Headers:
    - `Content-Type`_
        - *application/json; charset=utf-8*

Response JSON Object:
    - id (*string*): The requested UUID.
    - state (*string*): The state of the resource, *ok* or *error*.
    - reason (*string*, optional): The error message.

Status Codes:
    - `200 OK`_
        - UUID successfully unregistered.
    - `400 Bad Request`_
        - Wrong `Content-Type` or `revision` out-of-date.
    - `404 Not Found`_
        - UUID already unregistered or not found.

Request::

    DELETE /219e0050-10e0-48dd-9b99-e196acfb30c8 HTTP/1.1
    Accept: application/json
    Content-Type: application/json

    {
        "rev": "41997b1e-2850-43b5-b4b5-309d05307bf7"
    }

Response::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": "219e0050-10e0-48dd-9b99-e196acfb30c8",
        "state": "ok"
    }

Error Response::

    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
        "id": "219e0050-10e0-48dd-9b99-e196acfb30c8",
        "state": "error",
        "reason": "id not found"
    }


.. _Accept: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1
.. _Content-Type: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.17
.. _200 OK: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1
.. _400 Bad Request: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.1
.. _403 Forbidden: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.4
.. _404 Not Found: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5
.. _409 Conflict: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.10

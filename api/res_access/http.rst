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

Retrieving the metadata of ``<id>``.

Request Headers:
    - `Accept`_
        - *application/json*
        - *text/plain*

Response Headers:
    - `Content-Type`_
        - *application/json*
        - *text/plain; charset=utf-8*

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
        "state": "online"
    }

.. _Accept: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1
.. _Content-Type: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.17
.. _200 OK: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1
.. _404 Not Found: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5

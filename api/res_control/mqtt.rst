Resource Control MQTT API
===============================================================================

This specification will introduce MQTT API for :ref:`rc-proto`

A control message has following basic skeleton::

    {
        'msg_id': '...',
        'idf|odf': 'feature_name',
        'command': 'CMD',
        'topic': '...',
    }

And the expected response::

    {
        'msg_id': '...',
        'state': 'ok'
    }

Most of the control message is *device feature* level.

Signals
----------------------------------------------------------------------

Connect Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Example::

    {
        'msg_id': '99dd9b65-f7cf-4219-a4b2-60bc16e79670',
        'idf': 'meow',
        'command': 'CONNECT',
        'topic': 'iottalk/esm/5289c32a-bcb3-434d-80b6-de3a22dfc746/i'
    }

Response::

    {
        'msg_id': '99dd9b65-f7cf-4219-a4b2-60bc16e79670',
        'state': 'ok'
    }


Disconnect Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Example::

    {
        'msg_id': 'fd8d118c-11a0-4dc5-b8fe-e53a41149b09',
        'idf': 'meow',
        'command': 'DISCONNECT',
        'topic': 'iottalk/esm/5289c32a-bcb3-434d-80b6-de3a22dfc746/i'
    }

Response::

    {
        'msg_id': 'fd8d118c-11a0-4dc5-b8fe-e53a41149b09',
        'state': 'ok'
    }


Resume Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Example::

    {
        'msg_id': '9ea799c9-707b-4107-9903-686aa393e96d',
        'idf': 'meow',
        'command': 'RESUME',
    }

Response::

    {
        'msg_id': '9ea799c9-707b-4107-9903-686aa393e96d',
        'state': 'ok'
    }


Suspend Signal
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Example::

    {
        'msg_id': '3760aecf-e009-4c14-9a74-7c1800611763',
        'idf': 'meow',
        'command': 'SUSPEND',
    }

Response::

    {
        'msg_id': '3760aecf-e009-4c14-9a74-7c1800611763',
        'state': 'ok'
    }

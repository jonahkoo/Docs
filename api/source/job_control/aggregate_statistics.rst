Aggregate Statistics
====================

Get aggregate statistics for the user account.
The statistics can be aggregated for the requester's account and/or its sub-accounts,
grouped by week or month and filtered by a time range.

**HTTP Method**

.. http:get:: /api/job/aggregate_statistics

**Query String Parameters** — Required

+------------------+------------------------------------------------------------------------------+
| Name             | Details                                                                      |
+==================+==================+===========================================================+
| v                | `Description`    | The version of the API to use                             |
|                  +------------------+-----------------------------------------------------------+
|                  | `Allowed Values` | 1                                                         |
|                  +------------------+-----------------------------------------------------------+
|                  | `Example`        | ``v=1``                                                   |
+------------------+------------------+-----------------------------------------------------------+
| api_token        | `Description`    | The API token used for this session                       |
|                  +------------------+-----------------------------------------------------------+
|                  | `Allowed Values` | Hex String                                                |
|                  +------------------+-----------------------------------------------------------+
|                  | `Example`        | ``api_token=7ca5dc5c7cce449fb0fff719307e8f5f``            |
+------------------+------------------+-----------------------------------------------------------+

**Query String Parameters for filtering** — Optional

+-------------------------+-----------------------------------------------------------------------+
| Name                    | Details                                                               |
+=========================+==================+====================================================+
| account_id              | `Description`    | .. raw:: html                                      |
|                         |                  |                                                    |
|                         |                  |  Username of a sub account for which to</br>       |
|                         |                  |  return statistics. When unset, the call</br>      |
|                         |                  |  will return statistics for the requester's </br>  |
|                         |                  |  account. When set to *, the call will return</br> |
|                         |                  |  statistics for both requester's account</br>      |
|                         |                  |  and its sub-accounts.                             |
|                         |                  |                                                    |
|                         +------------------+----------------------------------------------------+
|                         | `Allowed Values` | String                                             |
|                         +------------------+----------------------------------------------------+
|                         | `Example`        | ``account_id=my_sub_account``                      |
+-------------------------+------------------+----------------------------------------------------+
| metrics                 | `Description`    | .. raw:: html                                      |
|                         |                  |                                                    |
|                         |                  |  List of metrics for which to calculate</br>       |
|                         |                  |  statistics. When unspecified, will return </br>   |
|                         |                  |  data for all available metrics.</br>              |
|                         |                  |  Currently supported metrics:</br>                 |
|                         |                  |  -`billable_minutes_total`</br>                    |
|                         |                  |  -`billable_minutes_mechanical`</br>               |
|                         |                  |  -`billable_minutes_premium`</br>                  |
|                         |                  |  -`billable_minutes_professional`</br>             |
|                         |                  |  -`billable_minutes_foreign_transcription`</br>    |
|                         |                  |  -`billable_minutes_translation`</br>              |
|                         |                  |  -`billable_minutes_english_transcription`         |
|                         |                  |                                                    |
|                         +------------------+----------------------------------------------------+
|                         | `Allowed Values` | JSON array of strings                              |
|                         +------------------+----------------------------------------------------+
|                         | `Example`        | ``metrics=["billable_minutes_total"]``             |
+-------------------------+------------------+----------------------------------------------------+
| group_by                | `Description`    | .. raw:: html                                      |
|                         |                  |                                                    |
|                         |                  |  Defines how to segment the calculations.</br>     |
|                         |                  |  When unspecified, will return a single</br>       |
|                         |                  |  segment over the given time range.                |
|                         |                  |                                                    |
|                         +------------------+----------------------------------------------------+
|                         | `Allowed Values` | ["week", "month"]                                  |
|                         +------------------+----------------------------------------------------+
|                         | `Example`        | ``group_by=month``                                 |
+-------------------------+------------------+----------------------------------------------------+
| start_date              | `Description`    | .. raw:: html                                      |
|                         |                  |                                                    |
|                         |                  |  Will calculate statistics for jobs returned</br>  |
|                         |                  |  after the given date. When unspecified,</br>      |
|                         |                  |  the date of the first returned job is</br>        |
|                         |                  |  used as the start date.                           |
|                         |                  |                                                    |
|                         +------------------+----------------------------------------------------+
|                         | `Allowed Values` | Date in ISO format.                                |
|                         +------------------+----------------------------------------------------+
|                         | `Example`        | ``start_date=2014-08-27T13:40:53``                 |
+-------------------------+------------------+----------------------------------------------------+
| end_date                | `Description`    | .. raw:: html                                      |
|                         |                  |                                                    |
|                         |                  |  Will calculate statistics for jobs returned</br>  |
|                         |                  |  before the given date. When unspecified,</br>     |
|                         |                  |  the current time is used as the end date.         |
|                         |                  |                                                    |
|                         +------------------+----------------------------------------------------+
|                         | `Allowed Values` | Date in ISO format.                                |
|                         +------------------+----------------------------------------------------+
|                         | `Example`        | ``end_date=2014-08-27T13:40:53``                   |
+-------------------------+------------------+----------------------------------------------------+

**Responses**

+-----------+------------------------------------------------------------------------------------------+
| HTTP Code | Details                                                                                  |
+===========+===============+==========================================================================+
| 200       | `Description` | Success                                                                  |
|           +---------------+--------------------------------------------------------------------------+
|           | `Contents`    | .. code-block:: javascript                                               |
|           |               |                                                                          |
|           |               |  JSON formatted statistics.                                              |
|           |               |                                                                          |
|           |               | .. container::                                                           |
|           |               |                                                                          |
|           |               |    See below for details.                                                |
|           |               |                                                                          |
+-----------+---------------+--------------------------------------------------------------------------+
| 400       | `Description` | An error occurred                                                        |
|           +---------------+--------------------------------------------------------------------------+
|           | `Contents`    | Error description (see :ref:`error-format-label` for details)            |
+-----------+---------------+--------------------------------------------------------------------------+

**Example Requests and Responses**

.. sourcecode:: http

    GET /api/job/aggregate_statistics?v=1&api_token=7ca5dc5c7cce449fb0fff719307e8f5f HTTP/1.1
    Host: api.cielo24.com

.. sourcecode:: http

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "data": [
            /* When group_by is unspecified, data is aggregated into a single block */
            {
                "billable_minutes_total": 372,
                /* Note: Total = Foreign + English + Translation */
                "billable_minutes_foreign_transcription": 13,
                "billable_minutes_english_transcription": 340,
                "billable_minutes_translation": 19,
                /* Note: Total = Professional + Premium + Mechanical */
                "billable_minutes_professional": 323,
                "billable_minutes_premium": 6
                "billable_minutes_mechanical": 43,
                "start_date": "2015-03-20T15:32:19.902607",
                "end_date": "2015-10-30T12:28:23.894872",
            }
        ],
        "start_date": null,
        "end_date": null
    }


.. sourcecode:: http

    GET /api/job/aggregate_statistics?v=1&api_token=7ca5dc5c7cce449fb0fff719307e8f5f HTTP/1.1
    &metrics=["billable_minutes_total","billable_minutes_professional","billable_minutes_english_transcription"]
    &start_date=2015-03-26T11:36:09.237373&end_date=2015-05-01T11:35:46.993607&group_by=week
    Host: api.cielo24.com

.. sourcecode:: http

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "data": [
            {
                "billable_minutes_professional": 4,
                "billable_minutes_total": 4,
                "billable_minutes_english_transcription": 4,
                "start_date": "2015-03-26T11:36:09.237373",
                "end_date": "2015-03-28T23:59:59.999999"
            },
            {
                "billable_minutes_professional": 10,
                "billable_minutes_total": 14,
                "billable_minutes_english_transcription": 14,
                "start_date": "2015-03-29T00:00:00",
                "end_date": "2015-04-04T23:59:59.999999"
            },
            {
                "billable_minutes_professional": 15,
                "billable_minutes_total": 25,
                "billable_minutes_english_transcription": 17,
                "start_date": "2015-04-05T00:00:00",
                "end_date": "2015-04-11T23:59:59.999999"
            },
            {
                "billable_minutes_professional": 17,
                "billable_minutes_total": 18,
                "billable_minutes_english_transcription": 15,
                "start_date": "2015-04-12T00:00:00",
                "end_date": "2015-04-18T23:59:59.999999"
            },
            {
                "billable_minutes_professional": 10,
                "billable_minutes_total": 10,
                "billable_minutes_english_transcription": 10,
                "start_date": "2015-04-19T00:00:00",
                "end_date": "2015-04-25T23:59:59.999999"
            },
            {
                "billable_minutes_professional": 38,
                "billable_minutes_total": 38,
                "billable_minutes_english_transcription": 38,
                "start_date": "2015-04-26T00:00:00",
                "end_date": "2015-05-01T11:35:46.993607"
            }
        ],
        "start_date": "2015-03-26T11:36:09.237373",
        "end_date": "2015-05-01T11:35:46.993607"
    }

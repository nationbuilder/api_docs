Events API
==========

Index Endpoint
--------------
Use this endpoint to list the events stored in the nation

GET /api/v1/sites/:site_slug/pages/events

### Parameters
* `limit` - max number of results to show per page
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.
* tags - tags present on events returned
* starting - earliest start time for events returned (inclusive)
* until - latest end time for events returned (exclusive)
* calendar_id - the id of the calendar from which events should be scoped

### Example

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/events?starting=2013-03-01
```

```json
{
  "next": "/api/v1/sites/foobar/pages/events?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m&starting=2013-03-01",
  "prev": null,
  "results": [
    {
      "id": 5,
      "slug": "the_event_of_the_year",
      "status": "expired",
      "site_slug": "foobar",
      "name": "The event of the year!",
      "headline": "The event of the year!",
      "title": "The event of the year!",
      "excerpt": null,
      "author_id": 65,
      "published_at": "2013-05-07T14:14:07-07:00",
      "external_id": null,
      "tags": ["funny"],
      "intro": "",
      "calendar_id": 1,
      "contact": {
        "name": "Event Creator",
        "phone": null,
        "show_phone": true,
        "email": "foobarevents@mailinator.com",
        "show_email": true
      },
      "start_time": "2013-05-08T18:00:00-00:00",
      "end_time": "2013-05-08T21:00:00-00:00",
      "time_zone": "Hawaii",
      "rsvp_form": {
        "phone": "hidden",
        "address": "hidden",
        "allow_guests": true,
        "accept_rsvps": true,
        "gather_volunteers": false
      },
      "capacity": 15,
      "show_guests": true,
      "venue": {
        "name": "",
        "address": {
          "address1": "",
          "address2": "",
          "address3": "",
          "city": "Pasadena",
          "state": "CA",
          "country_code": "US",
          "zip": "91101",
          "lat": "34.1427587",
          "lng": "-118.1386005"
        }
      },
      "autoresponse": {
        "broadcaster_id": null,
        "subject": null,
        "body": null
      },
      "shifts": [{
        "id": 1,
        "start_time": "2013-05-17T10:50:17-00:00",
        "end_time": "2013-05-17T11:55:17-00:00",
        "time_zone": "Hawaii",
        "goal": 15,
      }]
    },
    {
      "id": 3,
      "slug": "event",
      "status": "expired",
      "site_slug": "foobar",
      "name": "event",
      "headline": "event",
      "title": "event - Foobar Softwares",
      "excerpt": null,
      "author_id": 9,
      "published_at": "2013-02-28T20:43:36-08:00",
      "external_id": null,
      "tags": [],
      "intro": null,
      "calendar_id": null,
      "contact": {
        "name": null,
        "phone": null,
        "show_phone": true,
        "email": null,
        "show_email": true
      },
      "start_time": "2013-03-01T18:00:00-00:00",
      "end_time": "2013-03-01T21:00:00-00:00",
      "time_zone": "Hawaii",
      "rsvp_form": {
        "phone": "hidden",
        "address": "hidden",
        "allow_guests": true,
        "accept_rsvps": true,
        "gather_volunteers": false
      },
      "capacity": 0,
      "show_guests": true,
      "venue": {
        "name": null,
        "address": null
      },
      "autoresponse": {
        "broadcaster_id": 2,
        "subject": "RSVP confirmation for event",
        "body": null
      },
      "shifts": null
    }
  ]
}
```


Create Endpoint
---------------
Use this endpoint to create an event

POST /api/v1/sites/:site_slug/pages/events

### Parameters
* `event` - the event resource you wish to create

### Example

```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/events
```

```json
{
  "event": {
    "status": "unlisted",
    "name": "Fasting Day",
    "intro": "Take the 24hr nofoodchallenge!!!",
    "time_zone": "-07:00",
    "start_time": "2013-05-08T17:00:00-00:00",
    "end_time": "2013-05-08T19:00:00-00:00",
    "contact": {
      "name": "Byron Anderson",
      "contact_phone": "1234567890",
      "show_phone": true,
      "contact_email": "contact@venue.com",
      "email": "contact@venue.com",
      "show_email": true
    },
    "rsvp_form": {
      "phone": "optional",
      "address": "required",
      "allow_guests": true,
      "accept_rsvps": true,
      "gather_volunteers": true
    },
    "show_guests": true,
    "capacity": 80,
    "venue": {
      "name": "Ralphs Parking Lot",
      "address": {
        "address1": "123 Foo St",
        "city": "Pasadena",
        "state": "CA"
      }
    }
  }
}
```

Update Endpoint
---------------
Use the update endpoint to change the details of a pre-existing event page.

Show Endpoint
-------------
Get the details of a specific event.

GET /api/v1/sites/:site_slug/pages/events/:id

### Example

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/events/1
```

```json
{
  "event": {
    "id": 1,
    "slug": "foobar",
    "status": "unlisted",
    "site_slug": "foobar",
    "name": "foobar",
    "headline": "foobar",
    "title": "foobar - Foobar Softwares",
    "excerpt": null,
    "author_id": 1,
    "published_at": null,
    "external_id": null,
    "tags": ["funny"],
    "intro": null,
    "calendar_id": 1,
    "contact": {
      "name": null,
      "phone": null,
      "show_phone": true,
      "email": null,
      "show_email": true
    },
    "start_time": "2013-02-07T18:00:00-00:00",
    "end_time": "2013-02-07T21:00:00-00:00",
    "time_zone": "Hawaii",
    "rsvp_form": {
      "phone": "hidden",
      "address": "hidden",
      "allow_guests": true,
      "accept_rsvps": true,
      "gather_volunteers": false
    },
    "capacity": 0,
    "show_guests": true,
    "venue": {
      "name": null,
      "address": null
    },
    "autoresponse": {
      "broadcaster_id": 2,
      "subject": "RSVP confirmation for foobar",
      "body": null
    },
    shifts: null
  }
}
```

Delete Endpoint
---------------
Remove an event from a site.

DELETE /api/v1/sites/:site_slug/pages/events/:id

RSVP Index Endpoint
-------------------
Use this endpoint to list the RSVPs for an event

GET /api/v1/sites/:site_slug/pages/events/:event_id/rsvps

### Parameters

* `limit` - max number of results to show per page (optional, default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

Issuing this request:

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/events/1/rsvps
```

Should get you a response like this:

```json
{
  "next": "/api/v1/sites/foobar/pages/events/1/rsvps?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": 2,
      "event_id": 13,
      "person_id": 75406,
      "guests_count": 2,
      "volunteer": false,
      "private": true,
      "canceled": false,
      "attended": true
    }
  ]
}
```

RSVP Creation Endpoint
----------------------
Use this endpoint to create an RSVP for an event

POST /api/v1/sites/:site_slug/pages/events/:event_id/rsvps

### Parameters

* rsvp
    * person_id - id of the person RSVPing for the event
    * guests_count - number of guests the person is bringing with them
    * volunteer - whether the person wishes to be volunteer staff for the event
    * private - whether the RSVP should be considered private
    * canceled - whether the person canceled the RSVP
    * attended - whether the person actually attended the event
    * shift_ids - ids of shifts the person wishes to staff

### Example

Issuing a request like this:

```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/events/1/rsvps
```

With attached body like this:

```json
{
  "rsvp": {
    "person_id": 123,
    "guests_count": 2,
    "volunteer": true,
    "private": true,
    "canceled": false,
    "attended": true,
    "shift_ids": [1, 2, 3]
  }
}
```

Should get you a response like this:

```json
{
  "rsvp": {
    "id": 5,
    "event_id": 1,
    "person_id": 123,
    "guests_count": 2,
    "volunteer": false,
    "private": true,
    "canceled": false,
    "attended": true,
    "shift_ids": [1, 2, 3]
  }
}
```

RSVP Update Endpoint
--------------------
Update the RSVP details, to set whether they cancel or attend, for example.
Acts similarly to creation endpoint.

PUT /api/v1/sites/:site_slug/pages/events/:event_id/rsvps/:id

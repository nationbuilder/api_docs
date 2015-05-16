Webhooks API
============

Use the webhooks API to manage callback requests that you wish to receive on the following types of events:

| Name                      | ID                    |
|---------------------------|-----------------------|
| Person created            | `person_created`      |
| Person changed            | `person_changed`      |
| Person contacted          | `person_contacted`    |
| Person destroyed          | `person_destroyed`    |
| Person merged             | `person_merged`       |
| People destroyed in batch | `people_destroyed`    |
| Donation succeeded        | `donation_succeeded`  |
| Donation changed          | `donation_changed`    |
| Donation canceled         | `donation_canceled`   |

Each webhook instance has a single URL and event type. A webhook sends an HTTP POST with a message body containing data about the person or donation created. The POST is sent to the URL whenever the event occurs.

Index endpoint
--------------
Returns a paginated list of the webhooks the nation has already registered with this endpoint

### Attributes

* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.


### Example

```
GET https://foobar.nationbuilder.com/api/v1/webhooks
```

Should get you a 200 response with body like this:

```json
{
  "next": "/api/v1/webhooks?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": "51f6d14dba6d1d31c0000003",
      "version": 2,
      "url": "http://requestb.in/1hupspb1",
      "event": "person_contact"
    }
  ]
}
```

Show Endpoint
-------------
Show the details of an individual webhook, retrieved by its id

### Example

```
GET https://foobar.nationbuilder.com/api/v1/webhooks/51f6d14dba6d1d31c0000003
```

Should get you a 200 response with body like this:

```json
{
  "webhook": {
    "id": "51f6d14dba6d1d31c0000003",
    "version": 2,
    "url": "http://requestb.in/1hupspb1",
    "event": "person_contact"
  }
}
```

Create Endpoint
---------------
Use this endpoint to register a webhook to fire on the occurance of a certain event.

### Attributes

* `version` - the payload version you want to receive. Choose from 1, 2, 3 or 4.
* `url` - the URL you want to have the webhook fire towards
* `event` - the event you want to observe (listed at top of page)

### Example

Issuing this request:

```
POST https://foobar.nationbuilder.com/api/v1/webhooks
```

With this body:

```json
{
  "webhook": {
    "version": 2,
    "url": "https://example.com/webhook_reception",
    "event": "person_contact"
  }
}
```

Should create the webhook, respond with 200 and a body like this:

```json
{
  "webhook": {
    "id": "4ff7600b2cf0512fc7000002",
    "version": 2,
    "url": "https://example.com/webhook_reception",
    "event": "person_contact"
  }
}
```

Destroy Endpoint
----------------
Remove a webhook to have NationBuilder stop sending events to the endpoint

### Example

```
DELETE https://foobar.nationbuilder.com/api/v1/webhooks
```

Should remove the webhook and return an empty response with status code 204.


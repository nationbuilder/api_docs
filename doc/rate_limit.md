NationBuilder API Rate Limit Policy
===================================

Updated 19 August 2013

Our rate limiting policy is a per-access token limit.

The access token rate limit applies on a per-interval basis: each access token is permitted 250 requests to a nation every 10 seconds.

You can gather realtime information about the rate limit by examining the headers we provide in API HTTP responses.

These headers are:

* X-RateLimit-Limit - the length of the rate limiting interval, usually 10 seconds
* X-RateLimit-Remaining - the remaining number of requests in the rate limiting interval
* X-RateLimit-Reset - the date that the rate limit interval resets, expressed in epoch time

For example:

```
X-RateLimit-Limit: 10s
X-RateLimit-Remaining: 249
X-RateLimit-Reset: 1377043200
```

Please contact NationBuilder Support (help@nationbuilder.com) if you run up against our rate limiting and wish to request more.

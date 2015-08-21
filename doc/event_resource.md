| name         | accessibility | sample                               | description                                                                                                  |
| ------------ | ------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| slug         | optional      | happy_labor_day                      | Page SEO friendly portion of URL                                                                             |
| path         | optional      | /billsmith/happy_labor_day           | URL path that locates the page on the website                                                                |
| status       | required      | published                            | unlisted or published                                                                                        |
| site_slug    | readonly      | foobar                               | The slug of the website the page is contained on                                                             |
| name         | required      | Happy Labor Day!                     | A name for the page                                                                                          |
| headline     | optional      | Celebrate the Labor movement with us | the headline of the page                                                                                     |
| title        | optional      | Happy Labor Day                      | A title for the page                                                                                         |
| excerpt      | optional      |                                      | Excerpt of the content of the page to show on search engine results                                          |
| author_id    | optional      | 12345                                | The author's id in the nation                                                                                |
| published_at | optional      | 2013-09-23T01:39:19-07:00            | When the page was published                                                                                  |
| external_id  | optional      | 14812                                | id of the page from another tool                                                                             |
| intro        | optional      | Lets change the world!               | introductory information about the event                                                                     |
| calendar_id  | optional      | 12345                                | ID of the calendar on which this event should appear                                                         |
| contact      | optional      |                                      | Information about the point of contact for the event                                                         |
| start_time   | required      | 2013-09-23T06:00:00-00:00            | When the event starts                                                                                        |
| end_time     | required      | 2013-09-23T07:00:00-00:00            | When the event ends                                                                                          |
| time_zone    | optional      | Hawaii                               | The time zone associated with the start and end times                                                        |
| rsvp_form    | optional      |                                      | details about the form users will fill out to (leave out and the event will not accept RSVPs)                |
| capacity     | optional      | 30                                   | The event's capacity for attendees                                                                           |
| show_guests  | optional      | true                                 | whether to show the guest list publically                                                                    |
| venue        | optional      |                                      | The details about the location of the event                                                                  |
| autoresponse | optional      |                                      | The details of the email to send to a person after they RSVP (leave out to not send RSVP confirmation email) |
| shifts       | optional      |                                      | shifts to display on the RSVP form                                                                           |
| tags         | optional      | ["funny", "true"]                    | list of tags                                                                                                 |

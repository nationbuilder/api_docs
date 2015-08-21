Surveys API
===========

Use the Survey Pages API to interface with surveys stored in NationBuilder

Index Endpoint
--------------
Retrieve a paginated list of existing surveys

`GET /api/v1/sites/:site_slug/pages/surveys`

### Parameters

* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/surveys
```

You will receive a response with status 200 and this data:

```json
{
  "next": "/api/v1/sites/foobar/pages/surveys?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": 1,
      "slug": "survey",
      "site_slug": "foobar",
      "name": "Survey",
      "status": "published",
      "questions": [
        {
          "id": 1,
          "prompt": "What issue is more important to you?",
          "external_id": null,
          "slug": "important_issue",
          "type": "multiple",
          "status": "published",
          "choices": [
            {
              "id": 2,
              "name": "Daily mail service to all homes. Yeah right....",
              "feedback": "really now...?"
            },
            {
              "id": 3,
              "name": "Opposition to any change in our naturalization laws",
              "feedback": null
            },
            {
              "id": 9,
              "name": "foobar",
              "feedback": "foobar"
            },
            {
              "id": 1,
              "name": "Kansas should be admitted as a state",
              "feedback": null
            }
          ]
        },
        {
          "id": 4,
          "prompt": "Do you support high speed trains to the sea?",
          "external_id": null,
          "slug": "trains2",
          "type": "multiple",
          "status": "published",
          "choices": [
            {
              "id": 10,
              "name": "Yes, it is urgently needed.",
              "feedback": null
            },
            {
              "id": 11,
              "name": "Maybe, if it doesn't cost an arm and a leg.",
              "feedback": null
            },
            {
              "id": 12,
              "name": "No, I never even go to the sea.",
              "feedback": null
            }
          ]
        }
      ],
      "status": "published"
    }
  ]
}
```

Create Endpoint
---------------

Creates a survey page with the attributes and questions provided.

```
POST /api/v1/sites/:site_slug/pages/surveys
```

### Parameters

* `slug` - the path at which to place the page.  Must be unique, and there are some restrictions for namespace collisions. (Optional- will be computed from name if not present)
* `status` - published or unlisted, depending on whether you want to page to be available immediately (required)
* `tags` - an array of tags to add to a person who responds to this survey (optional)
* `author_id` - the NationBuilder id author of the blog (optional)
* `name` - internal name, how the page will be referred to in lists in the control panel (required)
* `title` - Title of the page, shows up as tab name, for example (optional, defaults to the name)
* `headline` - Heading on the page (optional, defaults to the name)
* `excerpt` - meta attribute for SEO - description (optional)
* `external_id` - the unique identifier for this resource in an external service (optional)
* `questions` - array of question resources that represent the questions to ask when a person takes the survey
    * `prompt` - (required) the question string
    * `external_id` - (optional) an external identification string for this question
    * `tags` - (optional) array of tags to add to people who respond (in the case of yes/no questions, to add to those that answer 'yes')
    * `no_tags` - (optional) array of tags to add to people who respond "no" if this is a yes/no question
    * `type` - (required) the type of response the question expects (`multiple` for multiple choice, `yes_no` for a yes/no question, `text` for a free response question)
    * `status` - (required) `unlisted` or `published`
    * `slug` - (required) url portion to represent this question, this must be unique across all your nation's survey questions
    * `choices` - Choices available for a multiple choice question
        * `name` - (required) the string for the choice
        * `tags` - (optional) array of tags to add to a person who chooses this option
        * `feedback` - (optional) feedback to show to the user if this choice is selected

### Example

```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/surveys
```

### Request Body

```json
{
  "survey": {
    "slug": "survey",
    "name": "Survey",
    "tags": ["funny"],
    "questions": [{
      "prompt": "What issue is more important to you?",
      "external_id": null,
      "slug": "important_issue",
      "type": "multiple",
      "status": "published",
      "choices": [{
        "name": "Daily mail service to all homes. Yeah right....",
        "feedback": "really now...?"
      }, {
        "name": "Opposition to any change in our naturalization laws",
        "feedback": null
      }, {
        "name": "foobar",
        "feedback": "foobar"
      }, {
        "name": "Kansas should be admitted as a state",
        "feedback": null
      }]
    }]
  }
}
```

Please note that questions and choice ordering will match the request's ordering.

You will receive a response of status code 200 and a body like this:

```json
{
  "survey": {
    "id": 5,
    "slug": "survey",
    "status": "unlisted",
    "site_slug": "foobar",
    "name": "Survey",
    "headline": null,
    "title": null,
    "excerpt": null,
    "author_id": null,
    "external_id": null,
    "tags": ["funny"],
    "questions": [
      {
        "id": 9,
        "prompt": "What issue is more important to you?",
        "slug": "important_issue",
        "type": "multiple",
        "status": "published",
        "choices": [
          {
            "id": 3,
            "name": "Daily mail service to all homes. Yeah right....",
            "feedback": "really now...?"
          },
          {
            "id": 4,
            "name": "Opposition to any change in our naturalization laws",
            "feedback": null
          },
          {
            "id": 5,
            "name": "foobar",
            "feedback": "foobar"
          },
          {
            "id": 6,
            "name": "Kansas should be admitted as a state",
            "feedback": null
          }
        ]
      }
    ]
  }
}
```

Update Endpoint
---------------

`PUT /api/v1/sites/foobar/pages/surveys/:id`

### Example

```
PUT https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/surveys/5
```

### Request Body

```json
{
  "survey": {
    "slug": "survey",
    "site_slug": "foobar",
    "name": "Survey",
    "tags": ["funny"],
    "questions": [{
      "prompt": "Are you a political junkie?",
      "type": "yes_no",
      "slug": "politics_junk",
      "status": "published"
    }, {
      "id": 9,
      "prompt": "What issue is more important to you?",
      "slug": "important_issue",
      "type": "multiple",
      "status": "published",
      "choices": [{
        "id": 3,
        "name": "Daily mail service to all homes. Yeah right....",
        "feedback": "really now...?"
      }, {
        "id": 4,
        "name": "Opposition to any change in our naturalization laws",
        "feedback": null
      }, {
        "id": 6,
        "name": "Kansas should be admitted as a state",
        "feedback": null
      }]
    }]
  }
}
```

Note: When updating a survey, Nested resources such as questions and choices are updated, created, or deleted according to the following rules:

* Existing nested resources not mentioned in the response body will be deleted.
* Nested resource is updated if an `id` attribute is supplied and the nested resource is found in your nation's database.
* If a nested resource has an `id` attribute specified but is not found in your nation's database, the API call will fail and respond with status code 404.
* The nested resource is created if no `id` attribute is supplied.


You will get a response with code 200 like this:

```json
{
  "survey": {
    "id": 5,
    "slug": "survey",
    "status": "unlisted",
    "site_slug": "foobar",
    "name": "Survey",
    "headline": null,
    "title": null,
    "excerpt": null,
    "author_id": null,
    "external_id": null,
    "tags": ["funny"],
    "questions": [
      {
        "id": 10,
        "prompt": "Are you a political junkie?",
        "external_id": null,
        "slug": "politics_junk",
        "type": "yes_no",
        "status": "published",
        "choices": [

        ]
      },
      {
        "id": 9,
        "prompt": "What issue is more important to you?",
        "external_id": null,
        "slug": "important_issue",
        "type": "multiple",
        "status": "published",
        "choices": [
          {
            "id": 3,
            "name": "Daily mail service to all homes. Yeah right....",
            "feedback": "really now...?"
          },
          {
            "id": 4,
            "name": "Opposition to any change in our naturalization laws",
            "feedback": null
          },
          {
            "id": 6,
            "name": "Kansas should be admitted as a state",
            "feedback": null
          }
        ]
      }
    ]
  }
}
```

Destroy Endpoint
----------------

Destroy the survey provided.  This does not destroy the associated questions.

```
DELETE /api/v1/sites/:site_slug/pages/surveys/:id
```

Survey Response Index Endpoint
------------------------------

List survey responses, ordered by their creation time.

```
GET /api/v1/survey_responses
```

### Parameters
* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.
* `start_time` - starting time for results, in [unix time](http://en.wikipedia.org/wiki/Unix_time)
* `end_time` - ending time for results, also in unix time
* `survey_id` - scope the responses to just those made for a specific survey

### Example

```
GET https://foobar.nationbuilder.com/api/v1/survey_responses
```

```json
{
  "next": "/api/v1/survey_responses?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": 2,
      "survey_id": 3,
      "person_id": 17738,
      "created_at":"2013-07-24T08:36:50-07:00",
      "updated_at":"2013-07-24T08:36:50-07:00",
      "question_responses": [
        {
          "id": 4,
          "response": "yes",
          "question_id": 4
        }
      ]
    },
    {
      "id": 1,
      "survey_id": 1,
      "person_id": 1,
      "created_at": "2013-07-24T08:38:08-07:00",
      "updated_at": "2013-07-24T08:38:08-07:00",
      "question_responses": [
        {
          "id": 1,
          "response": 2,
          "question_id": 1
        },
        {
          "id": 2,
          "response": 4,
          "question_id": 2
        },
        {
          "id": 3,
          "response": 7,
          "question_id": 3
        }
      ]
    }
  ]
}
```

Survey Response Creation Endpoint
---------------------------------

Use this endpoint to register the responses a person has to a survey.  The answers will be attached to their profile page.

Note: Answering questions requires the surveyed person to have a first name and last name recorded.  If the person record does not have this information attached, a validation error will be returned from this call.

POST /api/v1/survey_responses


### Parameters

#### Survey Response Parameters
* `survey_id` - id of the survey the person answered
* `person_id` - id of the person who answered the survey
* `question_responses` - an array of objects representing the responses to individual questions that the surveyed person provided

#### Question Response Parameters
* `question_id` - id of the question being answered
* `response` - the response provided to the question. Response can be a string or id.  If the survey question being responded to is a multiple choice question, the response will be the choice with the id provided.  If the question is a yes/no question, then the response can be either "yes" or "no".  If the question is a text question, the response will be stored as the provided string.

### Example

```
POST https://foobar.nationbuilder.com/api/v1/survey_responses
```

```json
{
  "survey_response": {
    "survey_id": 1,
    "person_id": 19,
    "question_responses": [{
      "question_id": 12093814098,
      "response": 3
    }, {
      "question_id": 12093814099,
      "response": "No way man, that isn't cool"
    }, {
      "question_id": 12093814100,
      "response": "yes"
    }]
  }
}
```

The survey response will be made, and you should receive a response with status 200 and body like this:

```
{
  "survey_response": {
    "id": 3014",
    "survey_id": 1,
    "person_id": 19,
    "question_responses": [{
      "id": 12497,
      "question_id": 12093814098,
      "response": 3
    }, {
      "id": 12498,
      "question_id": 12093814099,
      "response": "No way man, that isn't cool"
    }, {
      "id": 12499,
      "question_id": 12093814100,
      "response": "yes"
    }]
  }
}
```


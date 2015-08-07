Lists API
=========

The lists API gives you fine-grained control over the lists in your nation.
This API can be used to create and modify lists, and it can also be used
to perform batch operations over the people that it contains.

Index Endpoint
--------------

The index endpoint shows a paginated list of custom lists.

```
GET /api/v1/lists
```

### Parameters
* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

This request:

```
GET https://foobar.nationbuilder.com/api/v1/lists
```

Should result in this response:

```json
{
  "next": "/api/v1/lists?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": 2,
      "name": "New Voters",
      "slug": "newvoters",
      "author_id": 1,
      "count": 5
    },
    {
      "id": 11,
      "name": "Famous Minnesotans",
      "slug": "minnesotans",
      "author_id": 15236,
      "count": 21
    },
    {
      "id": 9,
      "name": "Donors",
      "slug": "donors",
      "author_id": 15236,
      "count": 101
    }
  ]
}
```

People Endpoint
---------------

The people endpoint shows the people in a list.

```
GET /api/v1/lists/:id/people
```

### Parameters
* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

This request:

```
GET https://foobar.nationbuilder.com/api/v1/lists/11/people
```

Should result in this response:

```json
{
  "next": "/api/v1/lists/11/people?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "id": 9148,
      "first_name": "Jesse",
      "last_name": "Ventura",
      "email": "jv@mn.gov",
      ...
    },
    {
      "id": 15232,
      "first_name": "Garrison",
      "last_name": "Keillor",
      "email": "gk@mpr.org",
      ...
    }
  ]
}
```

Note: The above only highlights a subset of the fields available for a person.
For more information about which fields are included, visit the documentation
for the people API [here](http://nationbuilder.com/people_api).

Create Endpoint
---------------

Creates an empty list with the given attributes.

```POST /api/v1/lists```

### Parameters

* `list` - the attributes of the list you want to create
  * `name` - the name of the list
  * `slug` - a unique identifier for the list
  * `author_id` - the author of the list

### Example

Issuing this request:

```
POST https://foobar.nationbuilder.com/api/v1/lists
```

With this post body:

```json
{
  "list": {
    "name": "My List",
    "slug": "mylist",
    "author_id": 17736
  }
}
```

Should result in this response:

```json
{
  "list_resource": {
    "id": 12,
    "name": "My List",
    "slug": "mylist",
    "author_id": 17736,
    "count": 0
  }
}
```

Update Endpoint
---------------

Updates a list to have the given attributes.

```
PUT /api/v1/lists/:id
```

Destroy Endpoint
----------------

Destroys the indicated list.

```
DELETE /api/v1/lists/:id
```

Add People Endpoint
-------------------
Adds people to a list.

### Parameters
* `people_ids`: A JSON list containing ids of people you want added to the list. Up to 100,000 ids are accepted.

### Example

Issuing this request:

```
POST https://foobar.nationbuilder.com/api/v1/lists/1/people
```

with this request body:

```json
{
  "people_ids": [1, 2, 3]
}
```

results in:

```json
{
  "id": 1,
  "name": "my list",
  "slug": "my-list",
  "author_id": 2,
  "count": 3
}
```

Delete People Endpoint
-------------------
Deletes a list of people from a list asynchronously. After this call returns, you can expect
the people to be removed from the list after a few minutes.

### Parameters
* `people_ids`: A JSON list containing ids of people you want removed from a list. Up to 100,000 ids are accepted.

### Example

Issuing this request:

```
DELETE https://foobar.nationbuilder.com/api/v1/lists/1/people
```

with this request body:

```json
{
  "people_ids": [1, 2, 3]
}
```

results in:

```json
{
  "id": 1,
  "name": "my list",
  "slug": "my-list",
  "author_id": 2,
  "count": 0
}
```

Add Tag Endpoint
-----------------
Use this endpoint to apply a tag to the people contained in a list.
Note: this endpoint returns a HTTP 204 status code, but the tag is
not applied immediately. For larger lists, this operation takes many minutes.

```
POST /api/v1/lists/:list_id/tag/:tag_name
```

Remove Tag Endpoint
-----------------
Use this endpoint to delete a tag from the people contained in a list.
Note: this endpoint returns a HTTP 204 status code, but the tag is not deleted
immediately. For larger lists, this operation takes many minutes.

```
DELETE /api/v1/lists/:list_id/tag/:tag_name
```


Listing Creation Endpoint (DEPRECATED)
-------------------------
Adds a person to a list.

```
POST /api/v1/lists/:list_id/listings
```

### Parameters
* `person_id`: id of the person to add to the list

### Example

Issuing this request:

```
POST https://foobar.nationbuilder.com/api/v1/lists/1/listings
```

```json
{
  "listing": {
    "person_id": 1057
  }
}
```

Results in this response:

```json
{
  "listing": {
    "id": 14,
    "person_id": 1057,
    "list_id": 1
  }
}
```

Listing Deletion Endpoint (DEPRECATED)
-------------------------
Deletes a person from a list.

```
DELETE /api/v1/lists/:list_id/listings/:id
```

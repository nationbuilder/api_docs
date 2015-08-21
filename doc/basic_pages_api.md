Basic Pages API
===============
Use the Basic Pages API to create and interact with basic HTML pages on a site in NationBuilder

Index Endpoint
--------------
Show a list of the basic pages in the system
GET /api/v1/sites/:site_slug/pages/basic_pages

### Attributes
* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

If you make a request like this:

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar-site/pages/basic_pages
```

Then you should get a 200 response with a body like this:

```json
{
  "next": "/api/v1/sites/foobar-site/pages/basic_pages?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "slug": "about",
      "status": "published",
      "site_slug": "foobar-site",
      "name": "About",
      "headline": "About Foobar Softwares",
      "title": "About Foobar Softwares",
      "excerpt": null,
      "author_id": null,
      "external_id": null,
      "tags": ["funny"],
      "id": 10,
      "content": "<p>This is where you tell everyone about Foobar Softwares.</p>"
    },
    {
      "slug": "bla",
      "status": "published",
      "site_slug": "foobar-site",
      "name": "bla",
      "headline": "bla",
      "title": "bla - Foobar Softwares",
      "excerpt": "asdf",
      "author_id": null,
      "external_id": null,
      "tags": ["funny"],
      "id": 11,
      "content": "<p>afadsfds</p>"
    }
  ]
}
```

Create Endpoint
---------------
Create a basic page for a site in NationBuilder
POST /api/v1/sites/:site_slug/pages/basic_pages

### Attributes:
* basic_page attributes - the properties you wish to attach to the basic page being made
    * slug - the path at which to place the page.  Must be unique, and there are some restrictions for namespace collisions. (Optional- will be computed from name if not present)
    * content - the content html to have on the page.  There is a whitelist of html elements and attributes that are allowed.
    * status - published or unlisted, depending on whether you want to page to be available immediately (required)
    * name - internal name, how the page will be referred to in lists in the control panel (required)
    * title - Title of the page, shows up as tab name, for example (optional, defaults to the name)
    * headline - Heading on the page (optional, defaults to the name)
    * excerpt - meta attribute for SEO - description (optional)
    * external_id - the unique identifier for this resource in an external service (optional)
    * tags - list of tags (optional)

### Example

If you make a request like this:
```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar-site/pages/basic_pages
```

With a body like this:

```json
{
  "basic_page": {
    "name": "My Page",
    "content": "<p>My page content</p>",
    "status": "published"
  }
}
```

You should get a 200 response with a body like this:
```json
{
  "basic_page": {
    "slug": "my_page",
    "status": "published",
    "site_slug": "foobar-site",
    "name": "My Page",
    "headline": "My Page",
    "title": "My Page - Foobar Softwares",
    "excerpt": null,
    "id": 12,
    "content": "<p>My page content</p>"
  }
}
```

Then a new basic page will be entered into NationBuilder, and available to view immediately

Update Endpoint
---------------

Update the attributes of a basic page.  Note that this is an update of all properties of the page, partial updates are not supported.
PUT /api/v1/sites/:site_slug/pages/basic_pages/:id

### Example

Suppose you wanted to change the title of the page made in the documentation for the Create endpoint to be "New Title".  To do this, you would issue a request like this:

```
PUT http://foobar.nationbuilder.com/api/v1/sites/foobar-site/pages/basic_pages/12
```

With a body like this:

```json
{
  "basic_page": {
    "slug": "my_page",
    "status": "published",
    "name": "My Page",
    "headline": "My Page",
    "title": "New Title",
    "excerpt": null,
    "content": "<p>My page content</p>"
  }
}
```

That should update the page to have the new title, and return a response code of 200 with body like this:

```json
{
  "basic_page": {
    "slug": "my_page",
    "status": "published",
    "site_slug": "foobar-site",
    "name": "My Page",
    "headline": "My Page",
    "title": "New Title",
    "excerpt": null,
    "id": 12,
    "content": "<p>My page content</p>"
  }
}
```


Destroy Endpoint
----------------

Remove a basic page from NationBuilder
DELETE /api/v1/sites/:site_slug/pages/basic_pages/:id

### Example

Issuing a request like this:

```
DELETE http://foobar.nationbuilder.com/api/v1/sites/foobar-site/pages/basic_pages/12
```

Should remove the page from NationBuilder and respond with HTTP status code 204.

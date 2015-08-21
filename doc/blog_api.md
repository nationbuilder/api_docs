Blogs API
===============
Use the Blogs API to create and manage blogs on a site in NationBuilder.  You can attach posts to this blog via the Blog Posts API

Index Endpoint
--------------
Show a list of the blogs in the system
GET /api/v1/sites/:site_slug/pages/blogs

### Attributes
* `limit` - max number of results to show in one page of results (default 10, max 100).
* `__nonce` - generated pagination nonce. Do not modify.
* `__token` - generated pagination token. Do not modify.

### Example

If you make a request like this:

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs
```

Then you should get a 200 response with a body like this:

```json
{
  "next": "/api/v1/sites/foobar/pages/blogs?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "slug": "mindsell",
      "status": "published",
      "site_slug": "foobar",
      "name": "MindSell",
      "headline": "blogs for the thinking man",
      "title": "MindSell",
      "excerpt": null,
      "author_id": 5,
      "external_id": "34598732345",
      "tags": ["funny"],
      "id": 10
    },
    {
      "slug": "cropthought",
      "status": "published",
      "site_slug": "foobar",
      "name": "Crop Thought",
      "headline": "Only the cream",
      "title": "Crop Thought - Foobar Softwares",
      "excerpt": null,
      "author_id": 5,
      "external_id": null,
      "tags": ["healthy"],
      "id": 11
    }
  ]
}
```

Show Endpoint
-------------
Show the details of a blog.
GET /api/v1/sites/:site_slug/pages/blogs/:id

### Example
When you issue a request like this:
```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/10
```

Then you should get a 200 response with a body like this:

```json
{
  "blog": {
    "slug": "mindsell",
    "status": "published",
    "site_slug": "foobar",
    "name": "MindSell",
    "headline": "blogs for the thinking man",
    "title": "MindSell",
    "excerpt": null,
    "author_id": 5,
    "external_id": "34598732345",
    "tags": ["funny"],
    "id": 10
  }
}
```

Create Endpoint
---------------
Create a blog for a site in NationBuilder
POST /api/v1/sites/:site_slug/pages/blogs

### Attributes:
* blog attributes - the properties you wish to attach to the blog to be made
    * slug - the path at which to place the page.  Must be unique, and there are some restrictions for namespace collisions. (Optional- will be computed from name if not present)
    * status - published or unlisted, depending on whether you want to page to be available immediately (required)
    * author_id - the NationBuilder id author of the blog (optional)
    * name - internal name, how the page will be referred to in lists in the control panel (required)
    * title - Title of the page, shows up as tab name, for example (optional, defaults to the name)
    * headline - Heading on the page (optional, defaults to the name)
    * excerpt - meta attribute for SEO - description (optional)
    * external_id - the unique identifier for this resource in an external service (optional)
    * tags - list of tags (optional)

### Example

If you make a request like this:
```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs
```

With a body like this:

```json
{
  "blog": {
    "name": "My Blog",
    "status": "published"
  }
}
```

You should get a 200 response with a body like this:
```json
{
  "blog": {
    "slug": "my_blog",
    "status": "published",
    "site_slug": "foobar",
    "name": "My Blog",
    "headline": "My Blog",
    "title": "My Blog - Foobar Softwares",
    "excerpt": null,
    "author_id": null,
    "external_id": null,
    "tags": ["funny"],
    "id": 12
  }
}
```

Then a new, empty blog will be entered into NationBuilder, and available to view immediately

Update Endpoint
---------------

Update the attributes of a blog.  Note that this is an update of all properties of the page, partial updates are not supported.
PUT /api/v1/sites/:site_slug/pages/blogs/:id

### Example

Suppose you wanted to change the title of the page made in the documentation for the Create endpoint to be "New Title".  To do this, you would issue a request like this:

```
PUT http://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/12
```

With a body like this:

```json
{
  "blog": {
    "slug": "my_page",
    "status": "published",
    "name": "My Page",
    "headline": "My Page",
    "title": "New Title",
    "excerpt": null
  }
}
```

That should update the page to have the new title, and return a response code of 200 with body like this:

```json
{
  "blog": {
    "slug": "my_page",
    "status": "published",
    "site_slug": "foobar",
    "name": "My Page",
    "headline": "My Page",
    "title": "New Title",
    "excerpt": null,
    "author_id": null,
    "external_id": null,
    "tags": ["funny"],
    "id": 12
  }
}
```


Destroy Endpoint
----------------

Remove a blog from NationBuilder.  Note that this will not destroy associated blog posts.
DELETE /api/v1/sites/:site_slug/pages/blogs/:id

### Example

Issuing a request like this:

```
DELETE http://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/12
```

Should remove the page from NationBuilder and respond with HTTP status code 204.

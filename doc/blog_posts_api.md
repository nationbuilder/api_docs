# Blog Posts API
Use the Blogs API to create and manage blog posts for a NationBuilder blog.

## Index Endpoint
Show a list of the posts a blog has in the system GET /api/v1/sites/:site_slug/pages/blogs/:blog_id/posts

### Attributes
- `limit` - max number of results to show in one page of results (default 10, max 100).
- `__nonce` - generated pagination nonce. Do not modify.
- `__token` - generated pagination token. Do not modify.

### Example
If you make a request like this:

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/10/posts
```

Then you should get a 200 response with a body like this:

```json
{
  "next": "/api/v1/sites/foobar/pages/blogs/10/posts?__nonce=3OUjEzI6iyybc1F3sk6YrQ&__token=ADGvBW9wM69kUiss1KqTIyVeQ5M6OwiL6ttexRFnHK9m",
  "prev": null,
  "results": [
    {
      "slug": "will_wars",
      "status": "published",
      "site_slug": "foobar",
      "name": "Will Wars",
      "headline": "Will Wars",
      "title": "Will Wars - Foobar Softwares",
      "excerpt": null,
      "author_id": 5,
      "external_id": null,
      "tags": ["funny"],
      "published_at": "2012-12-07T11:24:00-08:00",
      "id": 10,
      "content_before_flip": "<p>Psychopathy is more common than you would expect</p>"
    },
    {
      "slug": "perspective_misalignments",
      "status": "published",
      "site_slug": "foobar",
      "name": "Perspective Misalignments",
      "headline": "Perspective Misalignments",
      "title": "Perspective Misalignments - Foobar Softwares",
      "excerpt": null,
      "author_id": 5,
      "external_id": null,
      "tags": ["smart"],
      "published_at": "2013-04-25T10:47:06-07:00",
      "id": 11,
      "content_before_flip": "<p>Some people just don't see eye to eye</p>"
    }
  ]
}
```

## Match Endpoint
Map an external service id to a NationBuilder blog post id GET /api/v1/sites/:site_slug/pages/blogs/:blog_id/match

### Attributes
- external_id - the external id

### Example
I have a wordpress blog that I have imported into NationBuilder via this API, and I want to edit one of the posts that was imported, to which I had attached the external id "wp_1234".  To edit the page I need the NationBuilder id, but I did not write it down.  To overcome this lack of information I can issue a request like this:

```
GET https://foobar.nationbuilder.com/api/v1/sites/foobar/blogs/13/match?external_id=wp_1234
```

And get back a response like this:

```json
{
  "id": 15
}
```

## Show Endpoint
Show the details of an individual blog post, including the content after the page change GET /api/v1/sites/:site_slug/pages/blogs/:blog_id/posts/:id

### Example
When you make a request like this:

```
GET https:///foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/10/posts/11
```

Then you should get a 200 response with a body like this:

```json
{
  "blog_post": {
    "slug": "perspective_misalignments",
    "status": "published",
    "author_id": 5,
    "site_slug": "foobar",
    "name": "Perspective Misalignments",
    "headline": "Perspective Misalignments",
    "title": "Perspective Misalignments - Foobar Softwares",
    "excerpt": null,
    "id": 11,
    "content_before_flip": "<p>Some people just don't see eye to eye</p>",
    "content_after_flip": "<p>Lorem ipsum dolor sit amet</p>",
    "external_id": nil,
    "tags": ["funny"],
    "published_at": "2013-04-25T10:47:06-07:00"
  }
}
```

## Create Endpoint
Create a blog post in NationBuilder POST /api/v1/sites/:site_slug/pages/blogs/:blog_id/posts

### Attributes:
- blog attributes - the properties you wish to attach to the blog to be made
  - slug - the path at which to place the page.  Must be unique, and there are some restrictions for namespace collisions. (Optional- will be computed from name if not present)
  - status - published or unlisted, depending on whether you want to page to be available immediately (required)
  - author_id - the NationBuilder id author of the blog post (optional)
  - name - internal name, how the page will be referred to in lists in the control panel (required)
  - title - Title of the page, shows up as tab name, for example (optional, defaults to the name)
  - headline - Heading on the page (optional, defaults to the name)
  - excerpt - meta attribute for SEO - description (optional)
  - external_id - the unique identifier for this resource in an external service (optional)
  - tags - list of tags (optional)
  - published_at - when the blog was published (optional)
  - content_before_flip - the content of the blog post to be shown on the posts index, typically used as teaser content.  Put the entire blog post content into this field to have it.  (one form of content is required). Content is sanitized before persistence.
  - content_after_flip - content of the blog post to be shown only on the full post page.  The full post page combines the content_before_flip to the content_after_flip

### Example
If you make a request like this:

```
POST https://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/10/posts
```

With a body like this:

```json
{
  "blog_post": {
    "name": "You will never guess",
    "slug": "puzzle",
    "status": "published",
    "content_before_flip": "<p>You will never guess what is after the flip</p>",
    "content_after_flip": "<p>The answer is words. Words were after the flip.</p>",
    "tags": ["funny", "true"],
    "published_at": "2013-04-25T10:47:06-07:00"
  }
}
```

You should get a 200 response with a body like this:

```json
{
  "blog": {
    "slug": "puzzle",
    "status": "published",
    "site_slug": "foobar",
    "name": "You will never guess",
    "headline": "You will never guess",
    "title": "You will never guess - Foobar Softwares",
    "excerpt": null,
    "author_id": null,
    "external_id": null,
    "tags": ["funny"],
    "published_at": "2013-04-25T10:47:06-07:00",
    "id": 16,
    "content_before_flip": "<p>You will never guess what is after the flip</p>",
    "content_after_flip": "<p>The answer is words. Words were after the flip.</p>",
    "tags": ["funny", "true"]
  }
}
```

Then the blog post will be entered into NationBuilder, and available to view immediately

## Update Endpoint
Update the attributes of a blog.  Note that this is an update of all properties of the page, partial updates are not supported. PUT /api/v1/sites/:site_slug/pages/blogs/:id

### Example

```
PUT http://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/12/posts/16
```

With a body like this:

```json
{
  "blog_post": {
    "slug": "my_page",
    "status": "published",
    "name": "My Page",
    "headline": "My Page",
    "title": "New Title",
    "excerpt": null,
    "content_before_flip": "new content",
    "content_after_flip": "same old content",
    "tags": ["funny"],
    "published_at": "2013-04-25T10:47:06-07:00"
  }
}
```

That should update the page to have the new title, and return a response code of 200 with body like this:

```json
{
  "blog_post": {
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
    "published_at": "2013-04-25T10:47:06-07:00",
    "id": 12,
    "content_before_flip": "new content",
    "content_after_flip": "same old content",
  }
}
```

## Destroy Endpoint
Remove a blog from NationBuilder DELETE /api/v1/sites/:site_slug/pages/blogs/:blog_id/posts/:id

### Example
Issuing a request like this:

```
DELETE http://foobar.nationbuilder.com/api/v1/sites/foobar/pages/blogs/12/posts/16
```

Should remove the blog post from NationBuilder and respond with HTTP status code 204.

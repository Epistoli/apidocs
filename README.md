# Epistoli API

This document is a short introduction to [Epistoli][epistoli]'s API.

Epistoli's API is quite simple so far. Its only goal is to allow you to save
bookmarks in association to a particular newsletter.

We plan to extend the current set of methods to also allow free extraction of
*your* contents, as well as the enhanced versions that we provide as a service.

[Follow us on Twitter][epistolist] to get the latest news, or don't hesitate to
get in touch by email at [support@episto.li][support_email].

[epistoli]: https://episto.li/
[epistolist]: https://twitter.com/epistolist
[support_email]: mailto:support@episto.li?subject=API%20Question

# Getting started

We do not provide libraries to interface with our API. However, feel free to
develop and publish bindings for your favorite platform. We will be happy to
recommend mature third-party implementations!

The API methods described in this document all use the current base API
endpoint: `https://episto.li/api/v1`. Hence, when we write about `/bookmarks`,
the full URL for this endpoint is really: `https://episto.li/api/v1/bookmarks`.

Epistoli's API is consumed exclusively via HTTPS, and uses JSON to respond.

## Authentication

You need an API token which is accessible from the *Configure* page of each
newsletter you manage: look for the *API* integration, and there is your
private token.

This token is strictly personal and confidential. Do **NOT** share it! We can
not guarantee the privacy of your data if you shared this token.

The API expects that you authenticate your calls using this private token.
When sending requests to the API, either:

 - use the `X-Token` HTTP header,
 - or send a `token` parameter within other query parameters.

## API Responses

The API uses HTTP status codes to indicate success, and server or client errors
accordingly. In addition, every API response's body contains a JSON object,
with an `ok` key, and a boolean value.

At the very least, it is:

```json
{
    "ok": true
}
```

In case of error, `ok` is false, and we provide an `error` key, with a string
containing an error code.

These are the expected common errors that the API could return:

 - `invalid_params`: Received parameters are invalid, or incorrectly formatted.
 - `not_authed`: No authentication token was provided.
 - `invalid_auth`: Invalid authentication token.

For example an unauthorized call yield a `403` HTTP status code, with this
body:

```json
{
    "ok": false,
    "error": "invalid_auth"
}
```

However, other errors can be returned in the case where the service is
down, or other unexpected factors affect processing. Regardless,
callers should **always** check the value of the `ok` param in the
response.


# API Methods

## Newsletters

### List newsletters

**Method:**

```
GET /newsletters
```

**Parameters:**

None.

**Response:**

When successful, the response will contain a `newsletters` key, as an
`Array` of Newsletter objects:

```json
{
    "ok": true,
    "newsletters": [
        {
            "id": "foo-bar",
            "name": "Foo Bar",
            "description": "Some description",
            "created_at": "2016-03-24T10:00:00.123Z"
        },
        {
            "id": "another-letter",
            "name": "Another Letter",
            "description": "Some description",
            "created_at": "2016-03-24T10:00:00.123Z"
        }
    ]
}
```

## Bookmarks

### Save a bookmark

**Method:**

```
POST /bookmarks
```

**Parameters:**

 - `url` (String), the URL you wish to save **required**,
 - `newsletter_id` (String), the ID of the newsletter to which attach your bookmark **required**,
 - `title` (String) a title for your bookmarked document,
 - `description` (String) a short description,
 - `icon` (String) a URL to an icon image,
 - `avatar` (String) a URL to an image of the author of a bookmarked document,
 - `image` (String) a URL to an image illustrating the bookmarked document,
 - `tags` (String) a list of tags separated by a comma.

**Response:**

When successful, the response will contain a `bookmark` object:

```json
{
    "ok": true,
    "bookmark": {
        "id": "1234abcd-1234-1234-1234-1234abcd",
        "created_at": "ISO-8601 date",
        "url": "http://example.com/...",
        "title": "A title if available",
        "description": "A description, if you set one",
        ...
    }
}
```

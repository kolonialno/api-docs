# Kolonial.no API

The API is currently not publicly available, but feel free to contact us if you have a great idea and want early access! You can reach us by mail at `tech at kolonial dot no`.

We only allow pre-approved API clients at this time, and each client needs a unique `User-Agent` and `Client-Token`. This is a work in progress and no API stability is guaranteed at this time.

And btw, we're hiring! [Read more about our technology](https://kolonial.no/om/teknologi/) :raised_hands:


## Quick start

    curl -H "Accept: application/json" -H "Content-Type: application/json" -H "User-Agent: <YOUR-USER-AGENT>/1.0" -H "X-Client-Token: <YOUR-CLIENT-TOKEN>" https://kolonial.no/api/v1/products/9329/


## Anonymous requests


### Search

The search endpoint can be used for both text queries and barcodes.

    GET /api/v1/search/?q=banan


## Authenticated requests

Some endpoints require that the user is authenticated.

### Session token

Acquire a session token with the login endpoint:


    POST /api/v1/user/login/
    {"username": "", "password": ""}

If successful, returns a `sessionid` which can be used for subsequent requests. This is valid for a long time.

To access authenticated URLs, set HTTP header `Cookie: sessionid=<token>`.


### Authenticated endpoints

Returns the current user's cart with full product contents:

    GET /api/v1/cart/


Update cart contents:

    POST /api/v1/cart/items/
    {"items": [{"product_id": 9329, "quantity": 2}]}


## Deprecated fields

These fields will be removed at a later date, please make use of the new fields as soon as possible:

- Product entries
 - `price` -> `gross_price`
 - `unit_price` -> `gross_unit_price`
 - `image_url` -> `images`

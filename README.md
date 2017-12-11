# Kolonial.no API

The API is currently not publicly available, but feel free to contact us if you have a great idea and want early access! You can reach us by mail at `api at kolonial dot no`.

We only allow pre-approved API clients at this time, and each client needs a unique `User-Agent` and `Client-Token`. This is a work in progress and no API stability is guaranteed at this time.

And btw, we're hiring! [Read more about our technology](https://kolonial.no/om/teknologi/) :raised_hands:


## Quick start

    curl -H "Accept: application/json" -H "Content-Type: application/json" -H "User-Agent: <YOUR-USER-AGENT>/1.0" -H "X-Client-Token: <YOUR-CLIENT-TOKEN>" https://kolonial.no/api/v1/products/9329/


## Usage

All requests require that the `User-Agent` and `X-Client-Token` headers are set. These are unique for the developers participating in the beta, so please do not share these. That said, we would really love if you open sourced your code!

We return JSON on all endpoints, and expect it as input. Remember to set the `Accept` and `Content-Type` headers accordingly.

### Authentication

Some endpoints requires that the user is authenticated. To acquire a session token, use the login endpoint and save the returned `sessionid` for use with subsequent requests. The session is valid for a very long time.

To access authenticated URLs, set the HTTP header `Cookie: sessionid=<token>`.

    POST /api/v1/user/login/
        {"username": "", "password": ""}

    POST /api/v1/user/logout/


## Endpoints

### Products

The product category ID can be used to obtain all products in a given _child_ category:

    GET /api/v1/productcategories/
    GET /api/v1/productcategories/<product_category_id>/

Extended product information:

    GET /api/v1/products/<product_id>/


### Search

The main search endpoint can be used for both text queries and numeric barcodes.

    GET /api/v1/search/?q=mango
    GET /api/v1/search/recipes/?q=pasta


### Recipes

These are some of the recipe related endpoints:

    GET /api/v1/recipes/
    GET /api/v1/recipes/<recipe_id>/
    GET /api/v1/recipes/plans/current/

These endpoints are personal for the user and requires authentication:


    GET  /api/v1/recipes/likes/
    GET  /api/v1/recipes/purchased/
    POST /api/v1/recipes/<recipe_id>/like-toggle/


### Cart

Manipulating the cart contents requires authentication. The `quantity` field adjusts the quantity currently in the cart.

    GET /api/v1/cart/

    POST /api/v1/cart/items/
        {"items": [{
            "product_id": 9329, "quantity": 2,
            "product_id": 15163, "quantity": -1
        }]}

    POST /api/v1/cart/clear/


## Deprecated fields

These fields will be removed soon, please make use of the new fields as soon as possible:

### Product entries

- `price` -> `gross_price`
- `unit_price` -> `gross_unit_price`
- `image_url` -> `images`

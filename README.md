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

    GET /api/v1/recipe-tags/
    GET /api/v1/recipe-tags/<recipe_tag_id>/
    GET /api/v1/recipes/plans/current/

    GET /api/v1/recipes/<recipe_id>/

These endpoints are personal for the user and requires authentication:


    GET  /api/v1/recipes/likes/
    GET  /api/v1/recipes/purchased/
    POST /api/v1/recipes/<recipe_id>/like-toggle/


### Cart

Manipulating the cart contents requires authentication. The `quantity` field adjusts the quantity currently in the cart.

    GET /api/v1/cart/

    POST /api/v1/cart/items/
        {
            "items": [{
                "product_id": 9329,
                "quantity": 2,
            }, {
                "product_id": 15163,
                "quantity": -1
            }]
        }

    POST /api/v1/cart/clear/


### Shopping lists (Product List)

Shopping lists (technically called product lists) can be manipulated through authenticated API calls.

#### List all product lists

Paginated response. Page size is 50 lists.

    GET /api/v1/product-lists/

    Response:
    {
      "next": null,
      "previous": null,
      "results": [
        {
          "id": <id>,
          "title": "My taco list",
          "description": "Collection of taco products",
          "url": "/handlelister/<id>/",
          "number_of_items": 1,
          "images": [
            {
              "thumbnail": {
                "url": "..."
              }
            }
          ]
        }
      ]
    }

#### Show product list items/details

    GET /api/v1/product-lists/<id>/

    Response:
    {
      "id": <id>,
      "title": "My taco list",
      "description": "Collection of taco products",
      "url": "/handlelister/<id>/",
      "number_of_items": 1,
      "items": [
        {
          "quantity": 1
          "product": {
            ...
          },
        }
      ]
    }

#### Create new product list

    POST /api/v1/product-lists/new/

    Payload:
    {
      "title": "My new list",
      "description": "An optional description"
    }

    Response:
    {
      "id": <id>,
      "title": "My new list",
      "description": "An optional description",
      "url": "/handlelister/<id>/",
      "number_of_items": 0,
      "items": []
    }


#### Change title and/or description

    POST /api/v1/product-lists/<id>/update/

    Payload:
    {
      "title": "My updated list title",
      "description": "An new description"
    }

    Response:
    {
      "id": <id>,
      "title": "My updated list title",
      "description": "A new description",
      "url": "/handlelister/<id>/",
      "number_of_items": 0,
      "items": []
    }

#### Delete an existing product list

    POST /api/v1/product-lists/<id>/delete/

    Response:
    {
      "success": true
    }

#### Add new product to list

    POST /api/v1/product-lists/<id>/products/

    Payload:
    {
      "product_id": <id>,
      "quantity": 1
    }

    Response:
    {
      "id": <id>,
      "title": "Example list title",
      "description": "",
      "url": "/handlelister/<id>/",
      "number_of_items": 1,
      "items": [...]
    }

#### Decrease quantity of product in list

Note: Notice that we use the same endpoint (`products/`) to manipulate all
items in the product list.

Also note that decreasing the quantity to 0 (zero) does not delete the product
from the list. In order to delete the product you need to pass `delete: true`
in the payload (see below).

    POST /api/v1/product-lists/<id>/products/

    Payload:
    {
      "product_id": <id>,
      "quantity": -1
    }

    Response:
    {
      "id": <id>,
      "title": "Example list title",
      "description": "",
      "url": "/handlelister/<id>/",
      "number_of_items": 1,
      "items": [...]
    }

#### Remove product from list

Note: Notice that we use the same endpoint (`products/`) to manipulate all
items in the product list.

    POST /api/v1/product-lists/<id>/products/

    Payload:
    {
      "product_id": <id>,
      "quantity": -1,
      "delete": true
    }

    Response:
    {
      "id": <id>,
      "title": "Example list title",
      "description": "",
      "url": "/handlelister/<id>/",
      "number_of_items": 0,
      "items": []
    }

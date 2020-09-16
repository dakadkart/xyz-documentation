<<<<<<< HEAD
## Work with Spaces

### Create a Space
=======
# Work with Spaces

## Create a Space
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/postSpace)*

```HTTP
POST /spaces
```

As it is a POST request, it has to have at least the following body.

```JSON
{
    "title": "My Demo Space",
    "description": "A description which may contain ***markdown*** syntax"
}
```

#### Response

```JSON
{
    "id": "x-demospace",
    "title": "My Demo Space",
    "description": "A description which may contain ***markdown*** syntax"
}
```

<<<<<<< HEAD
### Read a Specific Space
=======
## Read a Specific Space
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Spaces/getSpace)*

```HTTP
GET /spaces/{spaceId}
```

#### Response

```JSON
{
  "id": "{spaceId}",
  "title": "My Demo Space",
  "description": "A description which may contain ***markdown*** syntax"
}
```

## Read all Spaces

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Spaces/getSpaces)*

```HTTP
GET /spaces
```

#### Response

```JSON
[
    {
        "id": "x-demospace",
        "title": "My Demo Space",
        "description": "A description which may contain ***markdown*** syntax"
    },
    {
        "id": "x-trees",
        "title": "A public space",
        "description": "All the old oaks in Berlin"
    }
]
```

<<<<<<< HEAD
### Update a Space
=======
## Update a Space
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/patchSpace)*

```HTTP
PATCH /spaces/{spaceId}
```

which requires a body like the following:

```JSON
{
    "title": "My Demo Space",
    "description": "**Altered** Description"
}
```

#### Response

```JSON
{
    "title": "My Demo Space",
    "description": "**Altered** Description"
}
```

<<<<<<< HEAD
### Delete a Space
=======
## Delete a Space
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/deleteSpace)*

```HTTP
DELETE /spaces/{spaceId}
```

A successful response to this request is the following

#### Response

```HTTP
HTTP/1.1 204 No Content
``

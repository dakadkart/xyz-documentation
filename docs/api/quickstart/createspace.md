## Create a Space

Here you see the request and response for creating a new space:

## Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces)*

```HTTP
POST /spaces
```

with the following body:

```JSON
{
    "title": "My Demo Space",
    "description": "My Demo *Space* description"
}
```

<<<<<<< HEAD
> #### Info 
> The Description can contain formatting in markdown format.

=======
> #### Info
>
> The Description can contain formatting in markdown format.
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

## Response

```JSON
{
    "id": "{spaceId}",
    "title": "My Demo Space",
    "description": "** My Demo *Space* description"
}
```

<<<<<<< HEAD

> #### Info 
> The ID is a unique, randomly generated identifier and is mandatory as an argument in 
> subsequent requests.
=======
> #### Info
>
> The ID is a unique, randomly generated identifier and is mandatory as an argument in
> subsequent requests.
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

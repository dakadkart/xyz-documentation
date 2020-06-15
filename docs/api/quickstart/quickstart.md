# Quick Start

## Acquiring Credentials

All users of the HERE XYZ Hub must get authentication and authorization credentials. These credentials are assigned per application, and a bearer token is obtained by using the app_id and app_code issued by the HERE Developer Portal

The bearer token is provided as a value of the “Authorization” header parameter using the “Bearer” authentication scheme:

```HTTP
Authorization: Bearer <token>
```

or alternatively, as a URI query parameter:

```HTTP
access_token=<token>
```

To obtain the credentials for an application, please visit <http://developer.here.com/plans> to register with HERE.

You can read more about tokens, where to get and how to use them on the [Generate tokens](../getting-token.md) page.

## Reading Spaces

Once you have acquired your credentials. you can find out which spaces you have access to.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces)*

```HTTP
GET /spaces
```

### Response

```JSON
[
    {
        "id": "x-demospace",
        "title": "My Demo Space",
        "description": "Description as markdown"
    }
]
```

## Create a Space

Here you see the request and response for creating a new space:

### Request

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

!!! Info "Note:  The Description can contain formatting in markdown format."

### Response

```JSON
{
    "id": "{spaceId}",
    "title": "My Demo Space",
    "description": "** My Demo *Space* description"
}
```

!!! Info "Note:  The ID is a unique, randomly generated identifier and is mandatory as an argument in subsequent requests"

## Writing to a Space

Having created the new space, you can write to it. The following example adds a collection of features.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
POST /spaces/{spaceId}/features
```

The POST request requires a body like the following example:


```JSON
{
    "type":"FeatureCollection",
    "features":
    [
	    {
	    	"type":"Feature",
	    	"properties":
	    	{
	    		"name":"Baldy Mountain"
	    	},
	    	"geometry":
	    	{
	    		"type":"Point",
	    		"coordinates":[-100.728, 51.4686]
	    	}
	    }
	]
}
```

### Response

The response is a FeatureCollection, containing all created features.

```JSON
{
  "features": [
    {
      "geometry": {
        "type": "Point",
        "coordinates": [
          -100.728,
          51.4686
        ]
      },
      "id": "HsOsZ0FXUI",
      "type": "Feature",
      "properties": {
        "name": "Baldy Mountain",
        "@ns:com:here:xyz": {
          "createdAt": 1529855978032,
          "space": "{spaceId}",
          "tags": [
            "canada",
            "mountain"
          ],
          "updatedAt": 1529855978032
        }
      },
      "bbox": [
        -100.728,
        51.4686,
        -100.728,
        51.4686
      ]
    }
  ],
  "type": "FeatureCollection"
}
```

When you upload a feature to XYZ Hub, we automatically add a property *@ns:com:here:xyz* to it. The following information is recorded in this property:

+ *createdAt* - date and time the feature was created in milliseconds since 01.01.1970
+ *updatedAt* - date and time the feature was updated in milliseconds since 01.01.1970
+ *space* - random unique space ID, created at space creation, a string
+ *tags* - the tags you added to the space, an array of strings

We use the namespace to store the additional information in the object without interfering with the properties you provided.

## Use Activity-Log

!!! Note "Your account needs access to the XYZ Pro Services."

1. Create a new space with the activity-log listener and enableUUID set to true:

```HTTP
POST /spaces
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces/postSpace)*

```JSON
{
  "title": "Activity-Log Test",
  "enableUUID": true,
  "listeners": [
    {
      "id": "activity-log",
      "eventTypes": [
        "ModifySpaceEvent.request",
        "..."
      ]
    }
  ]
}
```

1. Check spaces. You will find the just created one, as well as a new one, that has a title like: Activity log for space \<newSpaceId\>

```HTTP
GET /spaces
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces/getSpaces)*

```JSON
[
 {
   "id": "<newSpaceId>",
   "title": "Activity-Log-Test",
   "description": null,
   "enableUUID": true,
   "createdAt": 1575271893917,
   "updatedAt": 1575271897152
 },
 {
   "id": "<activityLogSpaceId>",
   "title": "activity-log for space <newSpaceId>",
   "description": "This is an automatically created space for the history of space __<newSpaceId>__.  \nCreated on 2019-12-02 at 07:31  \n***\nModified features will be stored in this space by their original _uuid_.  \nThe original namespace properties of XYZ will be stored within the value 'original' of the namespace '@ns:com:here:xyz:log'.  \nIMPORTANT Deleting this space while activity-log is enabled, causes the absence of history.  \n***",
   "createdAt": 1575271894028,
   "updatedAt": 1575271894028,
   "searchableProperties": {
     "@ns:com:here:xyz:log.id": true,
     "@ns:com:here:xyz:log.invalidatedAt": true,
     "@ns:com:here:xyz:log.original.updatedAt": true
   }
 }
]
```

1. Post something into your newly created space:

```HTTP
PUT /spaces/<newSpaceId>/features
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features/putFeatures)*

```JSON
{
 "type":"FeatureCollection",
 "features":[
   {
     "id": "newFeatureId",
     "type": "Feature",
     "geometry": {
       "type": "Point",
       "coordinates": [1,0]
     }
   }
 ]
}
```

Check into your activity log space:

```HTTP
GET /spaces/<activityLogSpaceId>/iterate
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/iterateFeatures)*

This results in something like this

```JSON
{
 "type": "FeatureCollection",
 "features": [
   {
     "id": "<uuidOfFeature>",
     "type": "Feature",
     "properties": {
       "@ns:com:here:xyz": {
         "tags": [],
         "space": "<activityLogSpaceId>",
         "createdAt": 1575275435631,
         "updatedAt": 1575275435631
       },
       "@ns:com:here:xyz:log": {
         "id": "newFeatureId",
         "action": "SAVE",
         "original": {
           "space": "<newSpaceId>",
           "createdAt": 1575275435508,
           "updatedAt": 1575275435508
         },
         "invalidatedAt": 9223372036854776000
       }
     },
     "geometry": {
       "type": "Point",
       "coordinates": [
         1,
         0
       ]
     }
   }
 ]
}
```

### Search for specific feature

You can search the Activity Log for a specific feature using its original id. The id of a feature is a String, so ensure that it is quoted for the property search.
This request returns an unsorted list of all revisions of the object.

```HTTP
GET /spaces/<activityLogSpaceId>/search?p.@ns:com:here:xyz:log.id="newFeatureId"
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

Response

```JSON
{
  "type": "Feature",
  "id": "<uuidOfFeature>",
  "properties": {
    "@ns:com:here:xyz": {
      "tags": [],
      "space": "<activityLogSpaceId>",
      "createdAt": 1575275435631,
      "updatedAt": 1575275435631
    },
    "@ns:com:here:xyz:log": {
      "id": "newFeatureId",
      "action": "SAVE",
      "original": {
        "space": "<newSpaceId>",
        "createdAt": 1575275435508,
        "updatedAt": 1575275435508
      },
      "invalidatedAt": 9223372036854776000
    }
  },
  "geometry": {
    "type": "Point",
    "coordinates": [
      1,
      0
    ]
  }
}
```

### Look up a specific revision of a feature

You can search for a specific revision of a feature using the *uuid* of the feature:

```HTTP
GET /spaces/<activityLogSpaceId>/features/<uuidOfFeature>
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeature)*

Response:

```JSON
{
  "type": "Feature",
  "id": "<uuidOfFeature>",
  "geometry": {
    "type": "Point",
    "bbox": null,
    "coordinates": [
      1,
      0
    ]
  },
  "properties": {
    "@ns:com:here:xyz": {
      "space": "<activityLogSpaceId>",
      "createdAt": 1575275435631,
      "updatedAt": 1575275435631,
      "uuid": null,
      "puuid": null,
      "muuid": null,
      "tags": [],
      "_inputPosition": null
    },
    "@ns:com:here:xyz:log": {
      "id": "newFeatureId",
      "action": "SAVE",
      "original": {
        "space": "<newSpaceId>",
        "createdAt": 1575275435508,
        "updatedAt": 1575275435508
      },
      "invalidatedAt": 9223372036854776000
    }
  }
}
```

### Activity Log for a certain point in time

You can search for a specific point in time by looking at the *createdAt* and *invalidatedAt* timestamps:

```HTTP
GET /spaces/<activityLogSpaceId>/search?p.@ns:com:here:xyz:log.original.updatedAt=lte=1575275435508&p.@ns:com:here:xyz:log.invalidatedAt=gt=1575275435508
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

Response body:

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "<uuidOfFeature>",
      "type": "Feature",
      "properties": {
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<activityLogSpaceId>",
          "createdAt": 1575275435631,
          "updatedAt": 1575275435631
        },
        "@ns:com:here:xyz:log": {
          "id": "newFeatureId",
          "action": "SAVE",
          "original": {
            "space": "<newSpaceId>",
            "createdAt": 1575275435508,
            "updatedAt": 1575275435508
          },
          "invalidatedAt": 9223372036854776000
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    }
  ]
}
```

For further information head over to the [developer guide](../devguide/activitylogguide.md) on Activity-Log.

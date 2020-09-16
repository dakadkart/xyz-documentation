# Manage Tokens

<<<<<<< HEAD
HERE XYZ Hub APIs use project tokens that are tied to your Account or HERE Accounts SSO identity. Make sure to keep your credentials safe and don't include them directly in any web pages as they are the key to generate new access tokens.
=======
HERE Data Hub APIs use project tokens that are tied to your Account or HERE Accounts SSO identity. Make sure to keep your credentials safe and don't include them directly in any web pages as they are the key to generate new access tokens.
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

Usually you would use two tokens:

* **token for working** - this has read&write permissions on your spaces and you should keep this to yourself
* **token for sharing** - this should be generated with read-only permissions and can be included in web pages for publishing your data

## Token Administration

Log into the **Data Hub Token Generator** site with your `Email address` and `Password` or `HERE Accounts SSO identity`:

[`https://xyz.api.here.com/token-ui/`](https://xyz.api.here.com/token-ui/)

<<<<<<< HEAD
![](images/getting-tokens.png)
=======
[![Login Screen](images/start-token-login.png)](https://xyz.api.here.com/token-ui/)
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

## Assign Access Rights

Once logged in, you see the list of spaces that are available to your account and the permissions that are available
to be encoded in the token.

Available permissions for the token are:

* ReadFeatures
* CreateFeatures
* UpdateFeatures
* DeleteFeatures
* ManageSpaces

From the list, select the spaces the token should have access to. Note that some spaces can only be read and not written to.
It's a good practice to keep the number, rights and layers to the minimum needed for your project.

<<<<<<< HEAD
![](images/generate-tokens.png)
=======
![Assign Rights](images/generate-tokens.png)
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7

## Generate Token

Next click on **Generate Token** to see you token:

<<<<<<< HEAD
![](images/token-list.png)


> #### Note
=======
![List of Generated Tokens](images/token-list.png)

> #### Note
>
>>>>>>> 540352cf2039f82e2b4422ef0a2c63cce25bd4c7
> You can revoke the token at any time by just deleting it on the **Manage Token** tab.


# OAuth - Open Authorization

## Introduction
A lot of API’s require an **OAuth** authentication before their services can be requested.  For personal applications, we can use our personal access tokens. However, while building multi user applications where a number of users need to be authorized regularly, we normally use an OAuth integration. In this lesson, we shall look into OAuth, how it works and what are some of the benefits that this approach offers.

## Objectives
You will be able to:

* Understand and explain the role OAuth plays when working with a 3rd Party API

### What is OAuth?

> **OAuth stands for Open Authorization.**

OAuth is a process through which an application or website can access private user data from another website. The OAuth process works in a series of steps that must be fulfilled in the right order for a successful authorization. 

Prior to using OAuth, we must also register our application with the authorizer and get our **credentials** to use during the process. We need to  set up some information about the application, like the app's name or website, and most importantly, **a redirect URI**. The authorizer later uses this to contact the requesting app and tell them that the user said yes. 

> A URI (Uniform Resource Identifier) is a string that refers to a resource. The most common are URLs, which identify the resource by giving its location on the Web.

After registration, The first step is the **authorization**. Here, we send our users to the authorization server to ask for some permissions with our scope (permissions) that we would like to have. The user can see everything being requested on his behalf and confirm that they would like to grant our application access for those permissions.

 
The second step is the **redirect**. Redirect URI are a critical part of the OAuth flow. After a user successfully authorizes an application, the authorization server redirects the user back to app with either an **authorization code** in the URL. Because the redirect URL will contain sensitive information, it is critical that the service doesn’t redirect the user to arbitrary locations. The authorization code is used by our application in the final act of getting the access token. 


The final step is **acquisition**. This is where we finally receive our **access token** from service provider so we can process API requests for our user. We use the authorization code we received in the redirect to our redirect url and our own application secret (which is acquired during initial registration) in order to get our user’s access token. The access token can then be used to make API calls on behalf of our user.

This architecture is summarized in the image below:

<img src="oa3.png" width=600>

Following example shows a scenario where you may want to access a user's dropbox acccount for storing photo/media as a part of service you provide. 


### OAuth with DropBox

#### Registering the app
First we need to register our app with Dropbox. This process of registering the app includes choosing which permission our app needs, and specifying an app name etc. After creating and registering the app, we can set up the authorization process in our app. The Dropbox SDKs will take care of some of the OAuth 2 process automatically for us. 

#### Authorizing users
In order for our app to access our users' dropbox accounts, we need to have each user authenticate with Dropbox to both verify their identity and give our app permission to access their data on Dropbox. So in essence, **our app will access the Dropbox service on behalf of our users.** 

Dropbox uses **OAuth 2** specifications. [Details on OAuth2 can be viewed at the official website](https://oauth.net/2/). Once completed by a user, the OAuth process returns an access token to our app. 

> **An access token is a special string generated by authrizer that we need to send with 
each subsequent API request to uniquely identify both the app and the  user.**

Have a look at an example scenario for an Image/gallery app that wants to access its users' dropbox accounts for accessing or storing new images. 

<img src="oa1.png" width=600>

The key benefit of this appraco is that our app doesn't need to store or transmit the user's Dropbox password.The user will be redirected to Dropbox to authorize our app to access their Dropbox data. After the user has approved our app, they'll be sent back to the app with an authorization code. At this point our app will exchange the authorization code for an access token which can be used to make subsequent requests to the Dropbox API for downloading/uploading contents etc. OAuth also allows the user to authorize only a limited set of permissions and the user may choose to stop access at any time. This makes OAuth a safer and more secure form of API authorization for end users.

### Dropbox access for web apps

For a web app, the first step in the OAuth process is to redirect the user to a Dropbox webpage. Typically the user takes some action on our site, such as clicking a button and gets redirected to a particular Dropbox authorization URL. Dropbox authorization URL is specific to the app and is composed of app key, redirect URI, response type, and state. It looks something like:
```
 https://www.dropbox.com/oauth2/authorize?client_id=...&redirect_uri=...). 
```
User are required login to Dropbox and presented with a screen to authorize the app trying to access their Dropbox data. After their approval, users are redirected from Dropbox back to the app using the redirect URI provided in the Dropbox authorization URL. The redirect back to the app includes an authorization code from Dropbox which is used by the app to exchange it for an access token. This request to exchange the authorization code for an access token takes places behind-the-scenes with a call to the /token API endpoint and is not visible to your end users. This setup is shown in the image below.

<img src="oa2.png" width=600>

## Benefits of Using OAuth

OAuth was created as a response to the direct authentication process as in HTTP authentication, where the user is prompted for a username and password. Basic Authentication is still used as a primitive form of API authentication for server-side applications: instead of sending a username and password to the server with each request, the user sends an API key ID and secret. Before OAuth, sites prompted user to enter their username and password directly into a form and sites would log into the user account. 

OAuth is a delegated authorization framework for REST/APIs enabling apps to obtain limited access (scopes) to a user’s data without giving away a user’s password. It decouples authentication from authorization. OAuth supports multiple use cases addressing different access levels and device compatibilities. It supports server-to-server apps, browser-based apps, mobile/native apps, and consoles/TVs.

<img src="oa4.png" width=600>

## Further reading 

* [OAuth 2.0](https://oauth.net/2/)
* [How to dance the OAuth: a step-by-step lesson](https://medium.freecodecamp.org/how-to-dance-the-oauth-a-step-by-step-lesson-fd2364d89742)
* [OAuth, wherefore art thou?](https://medium.com/square-corner-blog/oauth-wherefore-art-thou-b7034098a0fd)

## Summary 

In this lesson we looked at the OAuth process for gaining access to user owned resources. We saw how we authorize our apps to access resources with a high level of confidentiality and security that OAith offers. We looked at an example of how this might work with the dropbox API and also some extra reading to see more examples of this process. Next, we shall put this into practice by seeing how this process works with Twitter API. 
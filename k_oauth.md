---
layout: post
title: OAuth
description: An SSO Implementation
image: 
nav-menu: true
---

OAuth is a delegated authorization framework for REST/APIs. It enables apps to obtain limited access (scopes) to a user’s data without giving away a user’s password. It decouples authentication from authorization and supports multiple use cases addressing different device capabilities. It supports server-to-server apps, browser-based apps, mobile/native apps, and consoles/TVs  

OAuth is built on the following central components:

* Scopes and Consent
* Actors
* Clients
* Tokens
* Authorization Server
* Flows

### Scopes and Consent
Bundles of permissions asked for by the client when requesting a token-  
<p align="center">
	<img src="https://user-images.githubusercontent.com/83104921/115947107-21691b80-a4e3-11eb-95a0-83e5efcbba25.png" width="50%">
</p>
Using this, the user has the ability to allow/revoke consent of his information to the applications 

### Actors
* Resource Owner: owns the data in the resource server. Example, I own the resource of my twitter profile.
* Resource Server: The API which stores data the application wants to access
* Client: the application that wants to access your data
* Authorization Server: The main engine of OAuth

### Clients
Clients can be public and confidential. 
Confidential clients can be trusted to store a secret.   
Public clients are browsers, mobile apps, and IoT devices.

### Tokens
The user uses tokens to access the Resource Server.
There are two types of Tokens:
* Access tokens-short lived
* Refresh token-long lived

#### Access Tokens
 Access Tokens are used to access APIs. You don’t need a confidential client to get an access token. Because these tokens can be short lived and scale out, they can’t be revoked, you just have to wait for them to time out.
 
#### Refresh TOkens
 To get a refresh token, applications typically require confidential clients with authentication.You use refresh tokens to get new acess tokens  Each time you refresh your access token you get a new cryptographically signed token. Key rotation is built into the system.
Tokens are retrieved from endpoints on the authorization server.

### Authorization Server
Tokens are retrieved from endpoints on the authorization server
Authorization Server has:
* Authorize Endpoint
* Token Endpoint
* Introspection Endpoint
* Revocation Endpoint

Once the user takes that authorization grant and hands that to the application, the client application no longer needs to use the browser to complete the OAuth flow to get the tokens.

### Flows
<p align="center">
	<img src="https://user-images.githubusercontent.com/83104921/115947573-2f6c6b80-a4e6-11eb-9dd1-0114f3077f67.png" width="60%">
</p>
#### Front Channel
Front Channel Flow where you authorize via user agent might look as follows:

* Resource Owner starts flow to delegate access to protected resource
* Client sends authorization request with desired scopes via browser redirect to the Authorize Endpoint on the Authorization Server
* Authorization Server returns a consent dialog. Here user allows application for the scopes if user has already authenticated (cached session cookie) else it redirects to Resource Server to login.
* The authorization grant is passed back to the application via browser redirect. This all happens on the front channel.

<p align="center">
	<img src="https://user-images.githubusercontent.com/83104921/115947537-ec11fd00-a4e5-11eb-8c5f-e2363619ee2e.png" width="35%">
</p>

On wire (HTTP raw):    
Request	
```    
GET https://accounts.google.com/o/oauth2/auth?scope=gmail.insert gmail.send   
&redirect_uri=https://app.example.com/oauth2/callback    
&response_type=code&client_id=812741506391    
&state=af0ifjsldkj   
```

Response	
```   
HTTP/1.1 302 Found      
Location: https://app.example.com/oauth2/callback?    
code=MsCeLvIaQm6bTrgtp7&state=af0ifjsldkj 
```

#### End Channel Flow

<p align="center">
	<img src="https://user-images.githubusercontent.com/83104921/115947724-2d56dc80-a4e7-11eb-961b-f7a436ac3376.png" width="35%">
</p>

The Client application sends an access token request to the token endpoint on the Authorization Server with confidential client credentials and client id. This process exchanges an Authorization Code Grant for an Access Token and (optionally) a Refresh Token. Client accesses a protected resource with Access Token.   
ON wire (HTTP raw)  :
   
**Request**
``` 
POST /oauth2/v3/token HTTP/1.1    
Host: www.googleapis.com     
Content-Type: application/x-www-form-urlencoded    
code=MsCeLvIaQm6bTrgtp7&client_id=812741506391&client_secret={client_secret}&redirect_uri=https://app.example.com/oauth2/callback&grant_type=authorization_code     
```


**Response**
```	   
{
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA"
}
```


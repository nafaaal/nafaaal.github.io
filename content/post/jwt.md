+++
title = "JSON Web Tokens"
date = "2022-10-18"
cover = "img/jwt_0.png"
description = "what are JAY DUBLEW TEES?????"
contentTypeName = "projects"
+++
&nbsp;

 Before we dive into what a JWT is, lets look at why they are needed in the first place.

The foundation of any data transfer over the internet is the Hyper Text Transfer Protocol (HTTP). This protocol is just a way to transfer data between a client and the server. The request is made by a client, typically using a web browser which the server would acknowledge and respond back with the requested resource.

&nbsp;

![Client Server Model](/img/jwt_1.png)

&nbsp;

The HTTP protocol itself does not remember the history of previous requests. So if someone logs into to a website in one request, how does the server know who that person is in the subsequent requests? 

This is where JSON Web Tokens come in. It is used to identify and track user activity, allowing the server to know who they are communicating with, without having to send their credentials with each and every single request. 

> Note: JWTS are not the only method to achieve this. Session IDs are also a popular option.

When you first succesfully log in to a website, the server will generate a JSON Web Token, which is sent back as a response to the client.

The client then needs to send this token back in future requests so that the server knows who the client is. This is handled by the web browser and is automatically set as a HTTP header for upcoming requests.

&nbsp;

![Client Server Model](/img/jwt_2.png)

&nbsp;

A JSON Web Token consists of 3 parts.

1) Header
2) Payload
3) Signature

They are seperated with a `.` in the form: 

- `hhhhhhhhhh.pppppppppp.ssssssssss`

&nbsp;

### The Header
It only consists of the type of hash used to sign the token, alongside what type of token it is; which is a JWT in this case.

```json
{
    "alg":"HS256",
    "typ:":"jwt"
}
```

&nbsp;

### The Payload
The payload contains some data or "claims" which can be linked to the client, which the server uses to verify whether the user has permissions to perform an action they may have requested.
```json
{
    "username":"qx209931",
    "ticket_no":"17261",
    "account_type": "superuser"
}
```

&nbsp;

### The Signature
The header and the payload are each encoded into Base64Url and is concatenated in the form `base64urlencode(hhhhhhhhhh)+ "." +base64urlencode(pppppppppp)`. The resulting string is then hashed using the algorithm specified in the header with a secret private key which the server only has access to.

In future requests, when the client send the JWT with their request, the server will compute the hash again and compare it with the signature of the recieved JWT to ensure its authenticity.

![JWT Server Compare](/img/jwt_3.png)

&nbsp;

<!-- ## How secure are JWTs?

Since the payload and the headers are only encoded using Base64-URL, it can be decoded by anyone who has access to the token. Therefore you should not put any sensitive data in the payload of the token. -->


> The encoded JWT in the banner image is encrypted using the HS256 algorithm, with the private key `bPeShVkYp3s6v9y$B&E)H@McQfTjWnZr`

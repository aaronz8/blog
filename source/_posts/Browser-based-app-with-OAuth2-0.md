title: Browser based app with OAuth2.0
date: 2015-03-06 14:23:05
tags:
---

First party browser based apps are often registered, but untrusted clients.
But, does it matter if it's untrusted? Even if the redirect_uri is to an untrusted domain, the only way a client can get a valid accesstoken is by having a user log in with valid username/password.



  +----------+
  | Resource |
  |  Owner   |
  |          |
  +----------+
       v
       |    Resource Owner
      (A) Password Credentials
       |
       v
  +---------+                                  +---------------+
  |         |>--(B)---- Resource Owner ------->|               |
  |         |         Password Credentials     | Authorization |
  | Client  |                                  |     Server    |
  |         |<--(C)---- Access Token ---------<|               |
  |         |        (w/ Refresh Token)        |               |
  |         |                                  |               |
  |         |>--(Da)---    API Call    ------->|               |
  |         |         (w/ Access Token)        |               |
  |         |                                  |               |
  |         |>--(Db)--  Refresh Token  ------->|               |
  |         |<--(Eb)--- Access Token ---------<|               |
  |         |        (w/ Refresh Token)        |               |
  +---------+                                  +---------------+

         Figure 5: Resource Owner Password Credentials Flow

The flow illustrated in Figure 5 includes the following steps:

(A)  The resource owner provides the client with its username and
     password.

(B)  The client requests an access token from the authorization
     server's token endpoint by including the credentials received
     from the resource owner.  When making the request, the client
     authenticates with the authorization server.
     Includes encrypted accesstoken payload with:
        _user, _client, created, token, ipAddr
     Includes encrypted refreshtoken payload with:
        _user, _client, created, token, ipAddr

(C)  The authorization server authenticates the client and validates
     the resource owner credentials, and if valid, issues an access
     token.

(Da) API Call happens before Access Token expiration

(Db) The client requests an access token from the authorization
     server's token endpoint by including the refresh token received
     from before.
(Eb) The authorization server authenticates the client and validates
     the resource owner credentials, and if valid, issues a new 
     access token.



AccessToken only:
expires: 2 weeks
hacker gains control without user knowing. has access for 2 weeks. Needs to log back in with username/pass after 2 weeks.

Access+Refresh Token:
expires: 2 hours
hacker gains control without user knowing. has access for 2 hours but also has refreshtoken.


Better explanation:
http://stackoverflow.com/a/12885823/1925955
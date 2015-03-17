title: Oauth2 for 1st party clients
date: 2015-03-15 23:06:14
tags:
---
A comparison of flows and tradeoffs between *implicit flow* and *resource owner password credentials flow*.

Terms are defined as follows:
  - react: browser side (untrusted)
  - server: client server accessing the oauth service (untrusted unless first party)
  - auth server: our oauth2 server

---------------------

## Implicit flow

1. auth server login
2. redirect to react with auth code
3. react sends auth code to server
4. server sends auth code and clientid/secret to auth server
5. server receives access token (jwt) for user
6. server send jwt to react

### Authorized client
1. react sends jwt to server on every request
2a. server accepts valid jwt, sends response (done)
2b. server rejects expired jwt, sends refresh token and clientid/secret to auth server
3. go to previous 5

### Unauthorized client
1. client sends jwt to auth server
2. Auth server rejects invalid clientid/secret

### What sees what
- react sees auth code, jwt (stores jwt only)
- server sees auth code, clientid/secret, access token

#### Results:
- jwt can only access this server since clientid/secret not known so can't access auth server directly, thus doesn't compromise other apps
- react doesn't see login credentials and access token
- server doesn't see login credentials


---------------------

## Resource owner password credentials flow

1. react sends login to server
2. server sends login + clientid/secret to auth server
3. server receives access token (jwt with exp) for user <!-- (maybe unsign at this point and re-sign during future requests to prevent client bypassing server to reach auth-server. maybe moot because sending this access token to auth server requires clientid+secret associated with the access token) -->
4. server sends jwt (user id etc, possibly same as prev step) to react

Authorized client - same as implicit flow

Unauthorized client - same as implicit flow

### What sees what
- react sees login and jwt (stores jwt only)
- server sees login, clientid/secret, access token

#### Results:
- jwt can only access this server since clientid/secret not known so can't access auth server directly, thus doesn't compromise other apps
- react doesn't see access token


---------------------

1st party apps = ROPCF since server is trusted with user login credentials and does not save login credentials in browser
3rd party apps = IF since server is not trusted with user login credentials

TODO: Only allow 1st party apps' clientid/secrets to exchange username/password for accesstoken.
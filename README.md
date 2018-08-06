# [WIP] Airloq
Authentication and authorization for [Apollo](https://www.apollographql.com/)

###Authentication
The user says who they are

###Authorization
The user has access to what they're requesting
 - **R**ole **B**ased **A**ccess **C**ontrols (RBAC): Access based on roles (one or many)
 - **M**andatory **A**ccess **C**ontrols (MAC): Access based on levels (one level and everything below)
 - **D**iscrectionary **A**ccess **C**ontrols (DAC): Access based on identity (granted to a specific user or users)
_Typicially, the user needs at least one of the roles plus mac access of >= value or have exlpicit dac access_

## Concept
 1. Client request is made via `login` mutation (username, password, 2FA)
 2. Upon authentication, Server issues encrypted [JWT](https://jwt.io/) with private claims containining authorization metadata to the client
 3. Client sends encrypted JWT along with every graphql query
 4. Server uses the private claims data to determine proper authorization response

### JWT private claims example
```javascript
{
  "roles": ["analyst","manager"]
  "level": "superSekret"
}
```

### Airloq Type
A `User` needs (`rbac` + `mac`) || `dac` to acess the resource
```javascript
type Airloq {
  rbac: [ Role ] // Needs at least one role to access (plus mac)
  mac: String    // Needs equal or greater MAC to access (plus rbac)
  dac: [ User ]  // Needs to be one of the users to access
}
```
_Maybe there is a need for a custom Airloq scalar type for each existing type to allow field level authorization?_

## Deauthorization
If needed, a token can be written to a deauthroization store to be blacklisted until its natural deauthorization occurs

# [WIP] Airloq
RBAC, MAC, DAC security middleware for [Apollo](https://www.apollographql.com/)

 - **R**ole **B**ased **A**ccess **C**ontrols (RBAC): Access based on roles (one or many)
 - **M**andatory **A**ccess **C**ontrols (MAC): Access based on levels (one level and everything below)
 - **D**iscrectionary **A**ccess **C**ontrols (DAC): Access based on identity (granted to a specific user or users)

## Concept
 1. Issue encrypted [JWT](https://jwt.io/) with private claims containing access control metadata from the server to the client after authentication
 2. Client sends encrypted JWT along with every graphql query
 3. Server uses the private claims data to determine authorization

### Private claims example
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
_Maybe there is a need for a custom Airloq scalar type for every existing type?_

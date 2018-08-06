# [WIP] Airloq
RBAC, MAC, DAC security middleware for [Apollo](https://www.apollographql.com/)

**R**ole **B**ased **A**ccess **C**ontrols (RBAC): Access based on roles (one or many)
**M**andatory **A**ccess **C**ontrols (MAC): Access based on levels (one level and everything below)
**D**iscrectionary **A**ccess **C**ontrols (DAC): Access based on identity (granted to a specific user or users)

## Concept
 - Issue encrypted [JWT](https://jwt.io/) with private claims containing access control metadata from the server to the client after authentication
 - Client sends encrypted JWT along with every graphql query
 - Server uses the private claims data to determine authorization

### Private claims example
```javascript
{
  "roles": ["analyst","manager"]
  "mac": "superSekret"
}
```

### Airloq Type
A `User` needs (`rbac` + `mac`) || `dac` to acess the 
```javascript
type Airloq {
  rbac: [ Role ] // Needs at least one role to access (plus mac)
  mac: String    // Needs equal or greater MAC to access (plus rbac)
  dac: [ User ]  // Needs to be one of the users to access
}
```

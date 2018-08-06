# [WIP] Airloq
Registration, authentication and authorization for [GraphQL](https://graphql.org)

### Registration
The user tells you who they are
 - Username & password: 'ol trusty
 - Registration token: control signups via registration token distribution
 - Email verification: verify the new user has access to an established email

### Authentication
The user says who they are
 - Username & password: the classics
 - Two factor authentication (2FA): one time use token that is typically valid for less than a minute and usually requires an external authorized physical device to generate

### Authorization
The user has access to what they're requesting
 - **R**ole **B**ased **A**ccess **C**ontrols (RBAC): Access based on roles (one or many)
 - **M**andatory **A**ccess **C**ontrols (MAC): Access based on levels (one level and everything below)
 - **D**iscrectionary **A**ccess **C**ontrols (DAC): Access based on identity (granted to a specific user or users)
_Typicially, the user needs at least one of the roles plus mac access of >= value or just have exlpicit dac access_

### Unauthorization
If the user is deemed unauthorized the `resolver`s can respond in a few different ways
 - Silent: return `null` or `[]`
 - Message: return an unauthorization message (e.g `"You do not have access to this data"`)
 - Mask: return the data with some or all of it masked (e.g `***-**-6789`)

## Concept
 1. Client request is made via `login` mutation (username, password, 2FA)
 2. Upon authentication, Server issues encrypted [JWT](https://jwt.io/) to the client
 3. Client sends encrypted JWT along with all other GraphQL `query` or `mutation` requests
 4. Server uses JWT to fetch the current user `context` to determine authorization state
 5. Server then applies authorization/unauthorization strategies as necessary via `resolver`s

### Airloq Type
BYO authorization schema or use the provided Airloq type to extend your users

A user needs (`rbac` + `mac`) || `dac` to acess the resource
```javascript
//Example user type
type User {
  ...
  authorization: Airloq
}

type Airloq {
  rbac: [ Role ] // Needs at least one role to access (plus mac)
  mac: String    // Needs equal or greater MAC to access (plus rbac)
  dac: [ User ]  // Needs to be one of the users to access
}
```

## Deauthorization
If needed, a token can be written to a deauthroization store to be blacklisted until its natural deauthorization occurs. You may need to track active tokens going out to have on hand for arbitrary deauthorization.

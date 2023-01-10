# OMS API Documentation

## Build package

## Package Structure

```
|- src
|   |- api
|   |- mappings
|   |- modules
|   |- types
|   |- util
|- index.js
```

## Modules
- ### Product
- ### Stock
- ### User

## Requirements

- Node v16.x.x

## Usage

1. Install OMS API package in the application
```js
npm i @hotwax/oms-api
```

2. Define an init method on app load that will be used to set the initial configuration for the package.
In case of vue project, you can define it inside mounted hook in `App.vue` file
```js
import { init } from '@hotwax/oms-api'
...
...
init(token, instanceURL, cacheAge)
```

2. Add following method calls to clear the token and instance url when app unmounts. Also, you can use the same method whenever user logout from the app.
In case of vue project, you can define it inside unmounted hook in `App.vue` file
```js
import { resetConfig } from '@hotwax/oms-api'
...
...
resetConfig()
```

3. Update the token or instance URL whenever needed by using the following methods:
  - `updateToken`: For updating the token value
  - `updateInstanceUrl`: For updating the instanceURL / backendURL.

4. Now you can use any method from the package by directly importing it from `@hotwax/oms-api`

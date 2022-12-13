# Permissions

## Role-based access control (RBAC) OR Permission-based access control (PBAC)




## References
1. [How Claims Based Authentication works - Pluralsight - YouTube](https://youtu.be/_DJUvkbcT8E)
2. [What is Claim ClaimsIdentity and ClaimsPrincipal YouTube] (https://youtu.be/wOM84_Ogcvw)






## Implementation


## Defining app permissions based upong server permissions

Set of app permisions and server permissions are identified distictly. A set of permission are defined at the app level, when the user has certain permission certain actions and routes are enabled. Certain rules are defined with which app permissions are identified based upon the server permissions.

Why not use the server permissions directly?
When using the server permissions directly, we may have following drawbacks:
1. We may to handle case when there are multiple permissions that allow certain actions.
2. We are moving away from the headless approach. We are tightly coupling the app with the backend permissions set. Different backend systems may have different set of permissions that allows user to do different set of operations. One of the backend system may have 2 permissions that allows certain permission while another one only single. 



Defined rules for identifying the permissions for any user. Different backend systems may have different set of permissions, and thus such rules could be defined for different backend easily
```
const permissionRules = {
    "APP_JOB_SKIP": "COMMON_ADMIN OR COMMON_UPDATE",
    "APP_JOB_CANCEL": "COMMON_ADMIN OR COMMON_DELETE"
} as any;
```

App permissons will be identified based upon the server permissions from server.

```
const prepareAppPermissions = (serverPermissions: any) => {
    const serverPermissionsInput = serverPermissions.reduce((serverPermissionsInput: any, permission: any) => {
        serverPermissionsInput[permission] = true;
        return serverPermissionsInput;
    }, {})
    const permissions = Object.keys(permissionRules).reduce((permissions: any, rule: any) => {
        if (getEvaluator(permissionRules[rule])(serverPermissionsInput)) {
            permissions.push(rule);
        }
        return permissions;
    }, [])
    const { can, rules } = new AbilityBuilder<ClaimBasedAbility>(PureAbility);
    permissions.map((permission: any) => {
        can(permission);
    })
    return rules;
}
```

What if there are 100s of permissions at server, will the code work efficiently?

Tested code console.time and console.timeEnd. 

1. Added 20 rules and 60k permissions. Took less than 100ms
2. 

https://developer.mozilla.org/en-US/docs/Web/API/console/timeEnd


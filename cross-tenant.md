

At least P1 License is required for this feature (both source and target) 

Docs: [Configure cross-tenant synchronization using Microsoft Graph API](https://learn.microsoft.com/en-us/azure/active-directory/multi-tenant-organizations/cross-tenant-synchronization-configure-graph)


-------------
TARGET TENANT
- Policy.Read.All
- Policy.ReadWrite.CrossTenantAccess
-------------

- Source tenant ID: {source-tenant-guid} (companyone)
- Target tenant ID: {target-tenant-guid} (companytwo)

```
POST https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners
{
  "tenantId": "{source-tenant-guid}"
}
```

```
PUT https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/{source-tenant-guid}/identitySynchronization
{
   "displayName": "from-companyone-sync",
   "userSyncInbound": 
    {
      "isSyncAllowed": true
    }
}
```

```
PATCH https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/{source-tenant-guid}
{
    "inboundTrust": null,
    "automaticUserConsentSettings":
    {
        "inboundAllowed": true
    }
}
```

-------------
SOURCE TENANT
- Policy.Read.All
- Policy.ReadWrite.CrossTenantAccess
- Application.ReadWrite.All
- Directory.ReadWrite.All
-------------

- Source tenant ID: {source-tenant-guid} (companyone)
- Target tenant ID: {target-tenant-guid} (companytwo)

```		
POST https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners
{
  "tenantId": "{target-tenant-guid}"
}
```

```
PATCH https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/{target-tenant-guid}
{
    "automaticUserConsentSettings":
    {
        "outboundAllowed": true
    }
}
```

```
POST https://graph.microsoft.com/beta/applicationTemplates/518e5f48-1fc8-4c48-9387-9fdf28b0dfe7/instantiate
{
  "displayName": "to-companytwo-sync"
}
```

## Test the connection to the target tenant

Use the **ObjectId** from the instantiate request result

```
POST https://graph.microsoft.com/beta/servicePrincipals/{objectId}/synchronization/jobs/validateCredentials
{
    "useSavedCredentials": false,
    "templateId": "Azure2Azure",
    "credentials": [
        {
            "key": "CompanyId",
            "value": "376a1f89-b02f-4a85-8252-2974d1984d14"
        },
        {
            "key": "AuthenticationType",
            "value": "SyncPolicy"
        }
    ]
}
```

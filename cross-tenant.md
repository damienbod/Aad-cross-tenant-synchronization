

At least P1 License is required for this feature (both source and target) 

Docs: [Configure cross-tenant synchronization using Microsoft Graph API](https://learn.microsoft.com/en-us/azure/active-directory/multi-tenant-organizations/cross-tenant-synchronization-configure-graph)


-------------
TARGET TENANT
- Policy.Read.All
- Policy.ReadWrite.CrossTenantAccess
-------------

- Source tenant ID: f3301478-744c-453b-833c-1140827c9e67
- Target tenant ID: 55e8d121-bb42-49c7-a9d8-1c410a7be6cb

```
POST https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners
{
  "tenantId": "f3301478-744c-453b-833c-1140827c9e67"
}
```

```
PUT https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/f3301478-744c-453b-833c-1140827c9e67/identitySynchronization
{
   "displayName": "companyone",
   "userSyncInbound": 
    {
      "isSyncAllowed": true
    }
}
```

```
PATCH https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/f3301478-744c-453b-833c-1140827c9e67
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

- Source tenant ID: f3301478-744c-453b-833c-1140827c9e67
- Target tenant ID: 55e8d121-bb42-49c7-a9d8-1c410a7be6cb

```		
POST https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners
{
  "tenantId": "55e8d121-bb42-49c7-a9d8-1c410a7be6cb"
}
```

```
PATCH https://graph.microsoft.com/beta/policies/crossTenantAccessPolicy/partners/55e8d121-bb42-49c7-a9d8-1c410a7be6cb
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
  "displayName": "companyone"
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

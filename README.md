
# A common architecture deployed with Bicep
We think a common architecture for Azure customers is Web App + Data + Key Vault + Monitoring. How easy it to deploy this with Bicep?

1. Install Bicep CLI and VS Code extension
2. Use examples from Bicep repo to copy/paste for starting point
   - web-app-sql-database: https://github.com/Azure/bicep/blob/main/docs/examples/201/web-app-sql-database/main.bicep
   - key-vault-create: https://github.com/Azure/bicep/blob/main/docs/examples/101/key-vault-create/main.bicep
3. Arrange/re-order, update value for tenantID param, and variables to preference
4. Use Bicep CLI to Run Bicep Build command: ``` Bicep Build ./Infrastructure.bicep ``` to generate ARM Template (Infrastructure._json_)
5. Disregard the warning: ``` Warning BCP081: Resource type "Microsoft.Web/sites/config@2020-06-01" does not have types available. ```
   Issue being tracked here: https://github.com/Azure/bicep/issues/657
6. Create resource group to deploy to using Azure CLI: ``` az group create --name CommonArchitectureResourceGroup --location centralus ```
6. Deploy to resource group using Azure CLI: ``` az deployment group create -f ./Infrastructure.json -g CommonArchitectureResourceGroup ```
7. Enter parameters values for sqlAdministratorLogin and sqlAdministratorPassword at command line
8. Get error: 

        Deployment failed. Correlation ID: 8152fa50-16bd-421b-b84a-be875de84c16. {
        "code": "ForbiddenByRbac",
        "message": "[ForbiddenByRbac (Forbidden)] Caller is not authorized to perform action on resource.\r\nIf role assignments, deny assignments or role definitions were changed recently, please observe propagation time.\r\nCaller: name=KeyVault/ManagementPlane;appid=04b07795-8ddb-461a-bbee-02f9e1bf7b46;oid=7fe210a2-f3ee-494c-a862-831ee35dc03e;iss=https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/\r\nAction: 'Microsoft.KeyVault/vaults/keys/read'\r\nResource: '/subscriptions/315a2d36-a47c-4d75-be4d-69c2633e7dcf/resourcegroups/commonarchitectureresourcegroup/providers/microsoft.keyvault/vaults/keyvaultrbxgcb4f2wnmw/keys/prodkey'\r\nAssignment: (not found)\r\nVault: keyVaultrbxgcb4f2wnmw;location=centralus\r\n"

9. Use Portal to add yourself contributor role assignment  
10. Deployment complete!
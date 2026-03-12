Hi all,

Based on our discussion, I want to align the final deployment prerequisites for the Data Science Landing Zone and the AML add-on so we do not miss any networking or platform dependencies at deployment time.

My understanding so far is:

* The Data Science core stack will create the AML associated baseline resources in our Terraform deployment:

  * Key Vault
  * Storage Account
  * Application Insights
  * ACR
  * Log Analytics as optional / good to have
* These associated services are expected to be private-only, but their private connectivity will be handled through the AML managed VNet, not through private endpoints in the spoke VNet we are creating for the landing zone.
* The AML workspace itself will require a private endpoint in the DSLZ spoke VNet that we are creating through Terraform.
* Central Private DNS support will be required for the AML workspace private endpoint.

To make sure we are fully ready, could you please confirm the below items:

1. AML workspace DNS

* Please confirm whether the required central Private DNS zones for AML workspace private link already exist, and if yes, please share the resource IDs.
* If they do not already exist, please create them centrally and link them as required for our DSLZ spoke VNet / DNS architecture.
* Please also confirm any DNS forwarding / resolver requirements we need to align with.

2. AML managed VNet private connectivity

* Please confirm that Storage Account, Key Vault, and ACR private connectivity will be through AML managed private endpoints in the AML managed VNet.
* Please confirm whether the same private-only expectation also applies to Application Insights and Log Analytics.

3. Monitoring private connectivity

* If Application Insights and Log Analytics must also be private-only, please confirm whether an Azure Monitor Private Link Scope (AMPLS) already exists.
* If not, please confirm whether a new AMPLS needs to be created.
* Please confirm which Application Insights component and Log Analytics workspace should be attached to it.
* Please confirm which private endpoint / VNet model should be used for AMPLS in this design.

4. Managed private endpoint approvals / RBAC

* Please confirm who will approve the managed private endpoint connections from AML managed VNet to Storage, Key Vault, and ACR.
* Please confirm whether the AML workspace managed identity needs explicit permissions on those target resources.
* If yes, please confirm whether Cloud Services will grant those permissions or whether we need to raise a separate request.

5. ACR-specific confirmation

* Please confirm whether AML should use the ACR created by our core stack or a shared/custom ACR.
* If a shared/custom ACR is required, please share the ACR resource ID.
* Please confirm expected ACR SKU and whether public network access should be disabled.
* Please also confirm whether any build/deployment agent connectivity considerations need to be accounted for.

6. Central backend / deployment access

* For completeness, please confirm the central Terraform backend details and that the Azure DevOps service connection has the required access across the Data Science subscription, hub subscription, and central tfstate subscription.

Once these are confirmed, we should have the full set of prerequisites required to proceed cleanly.

Thanks,
Shahzad

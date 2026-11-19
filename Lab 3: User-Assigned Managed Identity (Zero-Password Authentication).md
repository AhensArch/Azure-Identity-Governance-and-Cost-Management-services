## Lab 3: User-Assigned Managed Identity (Zero-Password Authentication)

**Goal:** Provision a standalone User-Assigned Managed Identity, grant it scoped RBAC permissions over a Resource Group, and inspect its Entra ID service principal identifiers—all at **$0.00** cost.

### Step 1: Create the User-Assigned Managed Identity ($0.00)

1. In the Azure Portal top search bar, type **Managed Identities** and select **Managed Identities** under **Services**.
2. Click **+ Create** (or **+ Add**) at the top left.
3. Fill in the **Basics** tab:
   - **Subscription:** `2026-ahens-test`
   - **Resource group:** Click **Create new**, enter `rg-identity-lab`, and click **OK**.
   - **Region:** **UAE North** (or **Central India**)
   - **Name:** `id-app-backend` (must be 3–24 alphanumeric characters, hyphens, or underscores if you ever attach it to a VM).
4. Click **Review + create** > **Create**.

### Step 2: Inspect the Underlying Entra ID Identifiers

1. Click **Go to resource** once deployment finishes.
2. On the **Overview** page (or **Properties** in the left menu), locate and note these two IDs:
   - **Client ID:** Used by application SDKs or Azure CLI (`az login --identity --username <client-id>`) to request tokens without storing any secret or password.
   - **Object (principal) ID:** The underlying Microsoft Entra ID Service Principal ID that Azure RBAC uses to grant permissions.

### Step 3: Grant Scoped RBAC Permissions to the Managed Identity

1. In `id-app-backend`'s left menu, click **Azure role assignments** (or open `rg-identity-lab` > **Access control (IAM)**).
2. Click **+ Add role assignment**:
   - **Scope:** Select **Resource group** and choose `rg-identity-lab`.
   - **Role:** Select **Reader**.
3. Click **Save**.
4. **Verify in IAM:** Open `rg-identity-lab` > **Access control (IAM)** > **Role assignments** and confirm `id-app-backend` appears under the **Reader** role alongside any inherited subscription owners.

### Step 4: Clean Up (or Keep for Terraform)

- **Cost:** **$0.00** whether you keep it or delete it, since Managed Identities have no creation or hourly fee.
- **To clean up:** Open `rg-identity-lab` and click **Delete resource group** (deleting the resource group automatically removes `id-app-backend` and its role assignment).

### Optional CLI Equivalent (`az identity`)

If you want to replicate or verify this from your terminal:

```bash
# 1. Create resource group and User-Assigned Managed Identity ($0)
az group create --name rg-identity-lab --location uaenorth
az identity create --name id-app-backend --resource-group rg-identity-lab

# 2. Retrieve Principal ID and Client ID
az identity show --name id-app-backend --resource-group rg-identity-lab \
  --query "{clientId:clientId, principalId:principalId}" -o table

# 3. Clean up when finished
az group delete --name rg-identity-lab --yes --no-wait

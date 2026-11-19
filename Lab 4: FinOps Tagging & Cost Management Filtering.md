## Lab 4: FinOps Tagging & Cost Management Filtering

**Goal:** Apply FinOps cost-attribution tags to resource groups, assign a policy for tag inheritance on child resources, confirm Cost Management tag inheritance is enabled, practice filtering and grouping in Cost analysis, and clean up all lab resources cleanly.

### Step 1: Create the Dev and Prod Resource Groups ($0.00)

1. Search for **Resource groups** in the Azure Portal top search bar and select **Resource groups**.
2. Click **+ Create**, set the subscription to `2026-ahens-test`, name the first resource group `rg-finops-dev`, choose a region such as **(Middle East) UAE North**, and click **Review + create** > **Create**.
3. Repeat to create a second resource group named `rg-finops-prod` in the same region.

- **Verification:** Both `rg-finops-dev` and `rg-finops-prod` appear in the **Resource groups** list.

### Step 2: Apply FinOps Tags to Each Resource Group ($0.00)

1. Open `rg-finops-dev`, select **Tags** in the left navigation pane, enter `Environment = Dev` and `Owner = Sneha`, and click **Apply**.
2. Open `rg-finops-prod`, select **Tags** in the left navigation pane, enter `Environment = Prod`, and click **Apply**.

- **Verification:** Each resource group's **Overview** page displays its saved tags under **Tags**.

### Step 3: Assign the Tag Inheritance Policy on `rg-finops-prod` ($0.00)

1. Search for **Policy** in the top search bar, select **Policy**, go to **Authoring** > **Assignments** on the left menu, and click **Assign policy**.
2. Under **Basics**:
   - Click the **`...`** next to **Scope**, select your `2026-ahens-test` subscription and `rg-finops-prod`, and click **Select**.
   - Click the **`...`** next to **Policy definition**, search for **Inherit a tag from the resource group** (or **Inherit a tag from the resource group if missing**), select it, and click **Select**.
3. Click **Next** to **Parameters**, enter `Environment` in **Tag Name**, and click **Review + create** > **Create**.

- **Verification:** The assignment appears under **Policy** > **Assignments** scoped to `rg-finops-prod` with `Environment` set as the tag name. *(Newly assigned policies take a few minutes to evaluate, and **Inherit a tag from the resource group** applies when resources inside the scope are created or updated.)*

### Step 4: Verify Tag Inheritance & Filter in Cost Analysis ($0.00)

1. Open **Cost Management** (or **Cost Management + Billing** > **Cost Management** > **Cost analysis**) scoped to `2026-ahens-test`.
2. Under **Settings** (or **Manage subscription** / **Configuration**), confirm **Tag inheritance** is **Enabled** so resource group tags can propagate to child usage records within up to 24 hours.
3. Open the **Accumulated costs** view (under **All views** or **Recent**).
4. Click **+ Add filter**, choose **Tag**, select key `Environment` and value `Prod` (or `Dev`).
5. Change the **Group by** dropdown (above the chart) from **None** to **Resource group** (or a tag key). *(With `$0.00` spend in the period, the chart will show no reported cost and `--` for forecast, while confirming the filter and grouping controls are configured.)*

- **Verification:** The `Environment` tag filter is active and **Group by** reflects your chosen dimension (**Resource group** or tag).

### Step 5: Clean Up All Lab 4 Resources ($0.00)

1. Go to **Policy** > **Assignments**, locate the **Inherit a tag from the resource group** assignment on `rg-finops-prod`, open the **`...`** menu on the right, and click **Delete assignment**.
2. Open **Resource groups**, select `rg-finops-dev`, click **Delete resource group**, type `rg-finops-dev` to confirm, and click **Delete**.
3. Repeat the deletion step for `rg-finops-prod`.

- **Verification:** Neither `rg-finops-dev` nor `rg-finops-prod` remains in **Resource groups**, and the policy assignment is removed from **Policy** > **Assignments**.

### Quick CLI Alternative for Cleanup (`az`)

If you prefer removing the Lab 4 resource groups from your terminal after checking the portal:

```bash
az group delete --name rg-finops-dev --yes --no-wait
az group delete --name rg-finops-prod --yes --no-wait

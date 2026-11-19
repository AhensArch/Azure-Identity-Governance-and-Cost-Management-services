## Lab 2 — Azure Policy Guardrails & Resource Locks ($0.00)

Test governance rules that block unapproved deployments at validation time—meaning zero resources get created and zero charges occur.

### 1. Create the Lab Resource Group

1. Search for **Resource groups** in the top search bar and click **+ Create**.
2. Name it `rg-policy-lab`, choose **UAE North** (or **Central India**), and click **Review + create** > **Create**.

### 2. Assign the Allowed Locations Policy

1. Search for **Policy** in the top bar and open it.
2. On the left menu under **Authoring**, click **Assignments** > **Assign policy**.
3. Click the **`...`** next to **Scope**, select your subscription (`2026-ahens-test`) and `rg-policy-lab`, then click **Select**.
4. Click the **`...`** next to **Policy definition**, search for **Allowed locations**, select it, and click **Select**.
5. Click **Next** to go to the **Parameters** tab, open the **Allowed locations** dropdown, and select only **UAE North**.
6. Click **Review + create** > **Create**.

> **Note:** Azure Policy assignments take about 2–5 minutes to take effect.

### 3. Test the Policy Block (Validation-Only — $0)

1. Search for **Storage accounts** > **+ Create**.
2. Select `rg-policy-lab`, enter any unique lowercase name (e.g., `stpolicytest2026ahens`), and deliberately set **Region** to **(US) East US**.
3. Click **Review + create**. Azure validation will fail with `RequestDisallowedByPolicy`, proving your guardrail blocked a non-compliant deployment before any billable resource was created.
4. Do not fix the region—simply close the Storage Account blade.

### 4. Test a Delete Lock

1. Open `rg-policy-lab`, scroll down the left menu to **Settings** > **Locks**, and click **+ Add**.
2. Name it `BlockDelete`, set **Lock type** to **Delete**, and click **OK**.
3. Go back to `rg-policy-lab` > **Overview** and click **Delete resource group**—Azure will block deletion because of the lock.

### 5. Clean Up Lab 2

1. Go back to `rg-policy-lab` > **Settings** > **Locks** and delete `BlockDelete`.
2. Delete `rg-policy-lab` (and delete the policy assignment in **Policy** > **Assignments** if you want a clean slate).

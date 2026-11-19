# Lab 1: Role-Based Access Control (RBAC) & Custom Roles

## Goal

Practice least-privilege access control by creating a custom role and assigning permissions at the resource group scope.

## Cost

**$0.00** - Azure RBAC and resource groups are 100% free.

## Steps

1. Create an empty resource group named `rg-rbac-lab`.

2. In **Microsoft Entra ID**, go to **Users** and create a test user:

   ```text
   lab-junior-ops@<your-domain>.onmicrosoft.com
   ```

3. Open `rg-rbac-lab` and go to **Access control (IAM)**.

4. Assign the built-in **Reader** role to `lab-junior-ops`.

### Custom role challenge

In `rg-rbac-lab`, go to **Access control (IAM)** and complete the following:

1. Click **+ Add** > **Add custom role**.
2. Name the role `VM-Restarter`.
3. Grant only these permissions:

   - `Microsoft.Compute/virtualMachines/start/action`
   - `Microsoft.Compute/virtualMachines/restart/action`
   - `Microsoft.Compute/virtualMachines/read`

4. Go to **Access control (IAM)** > **Check access**.
5. Verify `lab-junior-ops`'s effective permissions on the resource group.

## Cleanup

1. Delete the test user from Microsoft Entra ID.
2. Delete `rg-rbac-lab`.

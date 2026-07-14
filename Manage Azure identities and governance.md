# AZ-104: Manage Azure Identity and Governance
## Advanced Practice Set — 50 Questions (Exam-Tough Edition)

Each question is followed immediately by the correct answer and a detailed explanation, as in an official exam review guide. This set skews toward scenario-based, multi-condition, and "gotcha" questions similar to what appears on the real AZ-104 exam.

---

**Q1.** A subscription has an RBAC Deny assignment blocking `Microsoft.Compute/virtualMachines/delete` at the resource group scope. A user has the Owner role at the subscription scope. What happens when they try to delete a VM in that resource group?

**Answer: C) The deletion is blocked.**

A) Allowed because Owner overrides deny
B) Allowed because deny assignments don't apply to Owners
C) Blocked — deny assignments always take precedence over any Allow role assignment, including Owner
D) Blocked only if the user is a guest

**Explanation:** Deny assignments are evaluated before and take absolute precedence over Allow (role) assignments in Azure RBAC, regardless of role or scope level. Even Owner or Global Administrator-elevated access cannot override an active deny assignment. Deny assignments are typically created by Azure Blueprints or Managed Apps, not manually by most users.

---

**Q2.** You configure Conditional Access with "Require MFA" targeting "All users" and "All cloud apps," but a break-glass emergency access account keeps getting locked out during testing. What is Microsoft's recommended mitigation?

**Answer: B) Exclude the break-glass account from the Conditional Access policy.**

A) Disable Conditional Access tenant-wide
B) Exclude the break-glass account from the Conditional Access policy
C) Add the account to PIM eligible assignments
D) Convert the account to a guest user

**Explanation:** Microsoft's best practice explicitly recommends excluding at least two cloud-only, highly secured emergency access ("break-glass") accounts from all Conditional Access policies to prevent a total lockout scenario (e.g., MFA provider outage), and to monitor sign-ins to these accounts closely.

---

**Q3.** A management group named "Corp" contains subscriptions Sub-A and Sub-B. A policy with Deny effect requiring the "Environment" tag is assigned at "Corp." Sub-A has its own policy assignment with Audit effect for the same built-in definition, assigned directly at the subscription. What is the effective behavior for untagged resource creation in Sub-A?

**Answer: A) Creation is denied.**

A) Creation is denied — the more restrictive (Deny) effect from the higher-scope assignment still applies
B) Creation is allowed and only audited, because the subscription-level assignment overrides the management group
C) Creation is allowed because policies don't inherit downward
D) The policies conflict and neither is enforced

**Explanation:** Azure Policy assignments are cumulative across scope levels — resources must satisfy every applicable policy from every scope in their hierarchy (management group, subscription, resource group). A lower-scope Audit assignment does not override or exempt resources from a higher-scope Deny assignment; both apply, and Deny is the more restrictive outcome, so it wins for enforcement purposes.

---

**Q4.** Your custom RBAC role definition includes: `"Actions": ["Microsoft.Compute/virtualMachines/*"], "NotActions": ["Microsoft.Compute/virtualMachines/delete"], "DataActions": [], "NotDataActions": []`. A user assigned this role has Contributor at a higher scope for the same resource. Can they delete the VM?

**Answer: B) Yes.**

A) No, NotActions permanently blocks the action regardless of other assignments
B) Yes — if another role assignment (like Contributor) independently grants the permission, the user retains it; NotActions only subtracts from that specific role, not from all combined effective permissions
C) No, because NotActions always takes precedence over all other roles
D) It depends on the resource lock state

**Explanation:** A key exam trap: `NotActions` excludes permissions from a *specific role definition only*. Effective permissions are the union of all Allow permissions from all assigned roles, minus explicit Deny assignments (not NotActions). If a separate role assignment (e.g., Contributor) grants delete rights, the user can still delete the VM — NotActions is not equivalent to a Deny assignment.

---

**Q5.** You need a group whose membership rule is `(user.department -eq "Finance") and (user.country -eq "US")`. Which licensing and group type combination is required?

**Answer: C) Entra ID P1 (or higher) + Dynamic User security or Microsoft 365 group.**

A) Entra ID Free + Assigned security group
B) Entra ID Free + Dynamic User group
C) Entra ID P1 (or higher) + Dynamic User security or Microsoft 365 group
D) Entra ID P2 only + Distribution group

**Explanation:** Dynamic membership rules (user or device based) require at least Entra ID Premium P1. Distribution groups do not support dynamic membership at all. P2 is not required — P1 is sufficient for dynamic groups (P2 adds PIM/Identity Protection/Access Reviews).

---

**Q6.** An Administrative Unit (AU) named "West Region" contains a set of users. A Helpdesk Administrator role is scoped to that AU. Can the scoped admin reset passwords for users outside the AU?

**Answer: B) No.**

A) Yes, AU-scoped roles still have tenant-wide effect
B) No — AU-scoped role assignments restrict the administrator's permissions to only the members of that administrative unit
C) Yes, but only for guest users outside the AU
D) No, AU-scoped roles have zero effective permissions

**Explanation:** Administrative Units let you delegate Entra ID admin roles (like Helpdesk Administrator or User Administrator) to a subset of the directory. When a role is assigned scoped to an AU, the administrator's rights apply only to objects (users, groups) that are members of that AU — not tenant-wide.

---

**Q7.** A subscription owner applies a `ReadOnly` lock to a storage account. An automated process attempts to rotate the storage account's access keys via `Microsoft.Storage/storageAccounts/regenerateKey`. What happens?

**Answer: A) The operation fails.**

A) The operation fails — regenerating keys is a POST action treated as a write operation and is blocked by ReadOnly locks
B) The operation succeeds because key rotation isn't a "write"
C) The operation succeeds because locks don't apply to data plane operations
D) The operation fails only if applied via PowerShell

**Explanation:** A commonly tested nuance: `ReadOnly` locks block essentially all POST/PUT/PATCH/DELETE operations, including actions that might seem like maintenance tasks (key regeneration, restarting a VM, scaling a resource). This frequently breaks automation and is a well-known "gotcha" with ReadOnly locks — Microsoft explicitly documents this caveat for Storage Accounts, App Service Plans, and other resources.

---

**Q8.** You assign a user the "Global Reader" Entra ID role. Which of the following can they do?

**Answer: B) View all administrative configuration and settings tenant-wide, read-only, without being able to make changes.**

A) Reset passwords for all users
B) View all administrative configuration and settings tenant-wide, read-only, without being able to make changes
C) Modify Conditional Access policies
D) Assign roles to other users

**Explanation:** Global Reader is the read-only counterpart to Global Administrator — it can view (but not edit) everything a Global Admin can see, including Conditional Access policies, audit logs, and service configurations. It's designed for auditors/compliance staff who need visibility without modification rights.

---

**Q9.** A policy definition uses effect `"deployIfNotExists"` with a `roleDefinitionId` specified for a managed identity. What is this roleDefinitionId used for?

**Answer: C) Granting the system-assigned managed identity (created for the policy assignment) the permissions needed to perform the remediation deployment.**

A) Assigning a role to the user who triggers policy evaluation
B) Defining which users can view the policy's compliance results
C) Granting the system-assigned managed identity (created for the policy assignment) the permissions needed to perform the remediation deployment
D) Restricting which subscriptions the policy can target

**Explanation:** `DeployIfNotExists` (and `Modify`) policies require a managed identity to actually execute the remediation deployment on the user's behalf. The `roleDefinitionId` in the policy definition specifies what RBAC role that identity is granted (e.g., Contributor) at the assignment scope, so it can create the missing resource or property.

---

**Q10.** Which statement about Azure Policy exemptions is correct?

**Answer: B) Exemptions can be time-bound and specify a category of "Waiver" (non-compliant but excused) or "Mitigated" (compensating control in place).**

A) Exemptions permanently delete the policy assignment
B) Exemptions can be time-bound and specify a category of "Waiver" (non-compliant but excused) or "Mitigated" (compensating control in place)
C) Exemptions apply only to Audit-effect policies
D) Exemptions require modifying the original policy definition

**Explanation:** Policy exemptions let you exclude a specific resource hierarchy scope from a policy or initiative assignment, with an expiration date and a category explaining the reasoning — useful for tracking known, approved exceptions without altering compliance reporting for the rest of the estate.

---

**Q11.** Two Conditional Access policies target the same user: Policy 1 grants access requiring MFA; Policy 2 blocks access entirely for the same conditions. What is the result?

**Answer: A) Access is blocked.**

A) Access is blocked — "Block access" always takes precedence over "Grant access" controls when multiple policies apply
B) Access is granted with MFA because more specific policies win
C) The policies must be manually merged by an admin
D) Access is granted because Block requires a higher license tier

**Explanation:** When multiple Conditional Access policies apply to a sign-in and one policy is set to "Block access," that decision wins regardless of any other policy that would otherwise grant access — Block always overrides Grant.

---

**Q12.** You need to give a vendor's automated deployment pipeline access to create resources in a resource group without using a user account. Which identity type is most appropriate, and how should credentials be handled to avoid storing secrets?

**Answer: C) A user-assigned managed identity, since it avoids secret management and can be assigned to the pipeline's compute resource.**

A) A guest user account with a shared password
B) A service principal with a client secret stored in the pipeline YAML
C) A user-assigned managed identity, since it avoids secret management and can be assigned to the pipeline's compute resource
D) An Azure AD B2B invitation

**Explanation:** Managed identities (system- or user-assigned) eliminate the need to manage credentials manually, as Azure handles secret rotation internally. For pipelines running from Azure compute (like Azure DevOps agents on an Azure VM or Automation Account), a user-assigned managed identity is preferable when the identity needs to be shared/reused. A service principal with a secret in code is a common anti-pattern.

---

**Q13.** Your management group hierarchy is: Root → Contoso → Prod / Dev (child management groups) → subscriptions. An RBAC Owner role is assigned at "Contoso." A DeployIfNotExists policy is assigned at "Prod" only. Does a user with Owner at Contoso see the Prod policy's compliance data by default?

**Answer: A) Yes, because RBAC and Policy inheritance flow downward, and Owner at a parent scope includes read access to child scope policy states.**

A) Yes, because RBAC and Policy inheritance flow downward, and Owner at a parent scope includes read access to child scope policy states
B) No, because policies do not inherit across management groups
C) No, because Owner does not include `Microsoft.Authorization/policyAssignments/read`
D) Only if explicitly re-assigned at the Prod management group

**Explanation:** Both Azure RBAC role assignments and Azure Policy assignments inherit downward through the management group hierarchy. A role assigned at a parent management group applies to all child management groups, subscriptions, and resources beneath it — so Owner at Contoso includes full visibility (and control) over everything in Prod and Dev.

---

**Q14.** A tenant has Security Defaults enabled. The admin also wants to configure a custom Conditional Access policy. What must happen first?

**Answer: B) Security Defaults must be disabled before Conditional Access policies can be created and enabled.**

A) Nothing — they work together automatically
B) Security Defaults must be disabled before Conditional Access policies can be created and enabled
C) Security Defaults automatically converts into a Conditional Access policy
D) Conditional Access requires deleting all users first

**Explanation:** Security Defaults and Conditional Access are mutually exclusive — Security Defaults is an all-or-nothing baseline, while Conditional Access offers granular control. Microsoft requires disabling Security Defaults before enabling any custom Conditional Access policies (which also require Entra ID P1/P2 licensing).

---

**Q15.** Which of the following actions is NOT possible using only Azure RBAC (without Entra ID role assignment)?

**Answer: C) Creating new user accounts in the directory.**

A) Granting a user Contributor access to a resource group
B) Creating a custom role scoped to a subscription
C) Creating new user accounts in the directory
D) Assigning the Reader role at a management group

**Explanation:** User account creation is a directory-level (Entra ID) operation controlled by Entra ID roles (like User Administrator), not Azure RBAC. Azure RBAC governs access to Azure resources (compute, storage, networking, management groups) — it cannot create or manage identity objects themselves.

---

**Q16.** You configure an access review for a Microsoft 365 group's guest members, set to recur quarterly, with "auto-apply results" enabled and "if reviewers don't respond: Remove access." A reviewer fails to respond by the review's end date. What happens to the guest user?

**Answer: A) The guest's group membership is automatically removed.**

A) The guest's group membership is automatically removed
B) The review pauses indefinitely awaiting a response
C) The guest is converted to a member
D) Nothing happens until an admin manually intervenes

**Explanation:** When "auto-apply results" is enabled and the fallback action for non-response is set to "Remove access," Entra ID automatically removes the guest's membership if no reviewer responds by the deadline — this automation is a key differentiator of Access Reviews (Entra ID Governance feature) from manual processes.

---

**Q17.** A resource has two Azure Policy assignments with `Modify` effect targeting the same tag but with conflicting values (one sets `Environment=Prod`, another sets `Environment=Dev`). How does Azure Policy resolve this conflict?

**Answer: D) The behavior is effectively non-deterministic/last-evaluated-wins; Microsoft recommends avoiding overlapping Modify policies with conflicting operations.**

A) Azure automatically merges both values into an array
B) The Deny effect always overrides Modify
C) The policy with the lower alphabetical name always wins
D) The behavior is effectively non-deterministic/last-evaluated-wins; Microsoft recommends avoiding overlapping Modify policies with conflicting operations

**Explanation:** This is an advanced, tougher exam-style question. Microsoft's guidance is that overlapping `Modify` (or `Append`) policy assignments that set conflicting values for the same field can produce unpredictable results, since evaluation order among assignments at the same scope isn't guaranteed. Best practice is to design policies so they don't overlap in what they modify.

---

**Q18.** A custom role has `"AssignableScopes": ["/subscriptions/{sub-id}/resourceGroups/RG1"]`. Can this role be assigned at the subscription level?

**Answer: B) No.**

A) Yes, AssignableScopes only restricts where it appears in the portal
B) No — a role can only be assigned at or below the scopes listed in AssignableScopes; it cannot be assigned at a broader (parent) scope
C) Yes, if the assigner has Owner at the subscription
D) Yes, but only via PowerShell

**Explanation:** `AssignableScopes` strictly defines the boundary of where a custom role can be assigned. Since RG1 is narrower than the subscription, you cannot assign this role at the subscription (a broader scope than what's listed) — you would need to edit AssignableScopes to include the subscription scope first.

---

**Q19.** In Privileged Identity Management (PIM), a user has an "Eligible" assignment for the Contributor role on a subscription, with a maximum activation duration of 8 hours and MFA required on activation. After activating, can the user's session be extended past 8 hours without reactivating?

**Answer: B) No — once the maximum activation duration expires, the role is automatically deactivated and must be reactivated (with MFA) if still eligible.**

A) Yes, PIM automatically renews active sessions
B) No — once the maximum activation duration expires, the role is automatically deactivated and must be reactivated (with MFA) if still eligible
C) Yes, if the user is also a Global Administrator
D) No, the role is permanently revoked and requires re-approval from scratch by an approver every time regardless of settings

**Explanation:** PIM activations are time-bound per the policy's maximum duration setting. When the duration expires, the elevated role is automatically deactivated (removed from the user's active assignments). If they remain "Eligible," they can reactivate again (going through MFA/approval as configured) — this is different from losing eligibility entirely.

---

**Q20.** Which built-in role allows a user to create and manage all Entra ID Conditional Access policies but NOT reset any user's password?

**Answer: A) Conditional Access Administrator.**

A) Conditional Access Administrator
B) Security Administrator
C) User Administrator
D) Global Administrator

**Explanation:** Conditional Access Administrator is a narrowly scoped role that can create/manage Conditional Access policies but has no rights over password resets or general user management. Security Administrator has broader security configuration rights (including Conditional Access) but also no password reset rights for regular users — however, the most precise, exam-correct answer for a role dedicated exclusively to Conditional Access is Conditional Access Administrator.

---

**Q21.** A management group "Finance" has an RBAC assignment granting the "Reader" role to a group called "Finance-Auditors." A new subscription is moved into the "Finance" management group after the assignment was created. Do members of Finance-Auditors automatically gain Reader on the new subscription?

**Answer: A) Yes.**

A) Yes — role assignments at a management group scope automatically apply to any subscription moved into that management group, since inheritance is evaluated dynamically
B) No — the assignment must be manually reapplied to the new subscription
C) Yes, but only after 24 hours of Azure AD synchronization
D) No — moving a subscription resets all inherited assignments

**Explanation:** RBAC (and Policy) inheritance in management groups is dynamic and structural — it's based on the current hierarchy at evaluation time, not a one-time copy. As soon as a subscription is moved under a management group, it immediately inherits all role assignments and policy assignments from that management group and its ancestors.

---

**Q22.** You want to block the creation of Network Security Groups entirely in a subscription, but allow all other resource types. Which is the most appropriate governance tool?

**Answer: C) An Azure Policy with a Deny effect targeting `Microsoft.Network/networkSecurityGroups`.**

A) A resource lock at the subscription
B) RBAC Deny assignment
C) An Azure Policy with a Deny effect targeting `Microsoft.Network/networkSecurityGroups`
D) A Conditional Access policy

**Explanation:** Azure Policy with a Deny effect is the standard governance mechanism for restricting which resource types can be deployed. Resource locks don't control resource *type* creation restrictions (they protect existing resources from modification/deletion). RBAC deny assignments are typically reserved for Blueprints/Managed Apps and aren't the standard user-facing tool for this scenario.

---

**Q23.** A guest user (B2B) is granted Contributor access to a resource group. The guest's home tenant deletes their account. What happens to their access in your tenant?

**Answer: B) The guest's access effectively breaks because authentication depends on the home tenant; the guest object remains in your tenant until manually removed but can no longer authenticate.**

A) Access continues indefinitely since it's cached
B) The guest's access effectively breaks because authentication depends on the home tenant; the guest object remains in your tenant until manually removed but can no longer authenticate
C) The guest is automatically converted to a cloud-only member account
D) Your tenant automatically deletes the guest object within 1 hour

**Explanation:** B2B guest authentication relies on the guest's home identity provider. If the home account is deleted, the guest can no longer sign in via that identity, effectively locking them out — but the guest object and any role assignments remain as stale entries in your tenant's directory until an administrator cleans them up (often surfaced via Access Reviews).

---

**Q24.** Which of the following correctly orders Azure Policy effects from LEAST to MOST restrictive/intrusive in typical governance strategy (start with monitoring only, end with blocking)?

**Answer: B) Audit → Append → Modify → Deny**

A) Deny → Modify → Append → Audit
B) Audit → Append → Modify → Deny
C) Modify → Deny → Audit → Append
D) Append → Audit → Deny → Modify

**Explanation:** A common governance rollout strategy: start with `Audit` (report only, no blocking) to assess impact, then use `Append`/`Modify` to auto-remediate metadata like tags without blocking deployments, and finally graduate to `Deny` once you're confident it won't disrupt legitimate operations.

---

**Q25.** Your organization requires that Storage Accounts cannot be deleted, but their access keys should still be rotatable and their properties updatable. Which lock type satisfies this exact requirement, and what's the key caveat regarding key rotation?

**Answer: A) CanNotDelete — this allows write/rotate operations but blocks the delete action.**

A) CanNotDelete — this allows write/rotate operations but blocks the delete action
B) ReadOnly — this allows delete but blocks all writes
C) ReadOnly — because key rotation is treated as a delete operation
D) CanNotDelete — but key rotation is also blocked because it's treated as an implicit delete-recreate

**Explanation:** Unlike ReadOnly (which blocks nearly all POST actions including key regeneration, as seen in Q7), `CanNotDelete` only blocks the delete operation while permitting normal write and management operations, including key rotation — making it the correct choice here.

---

**Q26.** An RBAC role assignment is made to a group. A user is added to that group. How long can it take, in the worst case, for the new group member's RBAC permissions to take effect for Azure Resource Manager operations?

**Answer: C) Up to approximately 30 minutes to a few hours, since Azure RBAC group membership token/cache refresh isn't always instantaneous.**

A) Instantaneous, always under 5 seconds
B) Exactly 24 hours, per Microsoft SLA
C) Up to approximately 30 minutes to a few hours, since Azure RBAC group membership token/cache refresh isn't always instantaneous
D) 7 days, matching Entra ID Connect sync cycles

**Explanation:** This is a known real-world/exam nuance: Azure RBAC group-based access can experience propagation delay because access tokens and internal caches don't refresh instantly. Microsoft's own documentation acknowledges this can take up to a few hours in some cases, which is why time-sensitive access changes are sometimes tested directly on individual users instead of via nested group changes.

---

**Q27.** You create a policy initiative combining 5 individual policy definitions, each targeting different resource types, all with Deny effects. You assign the initiative at a resource group scope. A deployment violates 2 of the 5 policies simultaneously. What happens?

**Answer: A) The deployment fails, and typically the error reports on the first violated policy encountered (though tooling may show all violations depending on the client).**

A) The deployment fails, and typically the error reports on the first violated policy encountered (though tooling may show all violations depending on the client)
B) The deployment succeeds because initiatives only block if ALL policies are violated
C) The deployment succeeds with a warning
D) The deployment is queued for manual approval

**Explanation:** With Deny-effect policies in an initiative, ANY single violated policy is sufficient to block the deployment — not all of them need to be violated. This differs from a common misconception that initiatives require full non-compliance across every included policy before blocking occurs.

---

**Q28.** A user with the Entra ID "License Administrator" role tries to create a new Conditional Access policy. Are they able to?

**Answer: B) No.**

A) Yes, license administration includes CA policy rights
B) No — License Administrator can only manage license assignments for users/groups, not Conditional Access policies
C) Yes, but only for policies affecting licensed users
D) Yes, if they are also assigned User Administrator

**Explanation:** License Administrator is a narrowly scoped role limited to managing product licenses and service plans for users and groups. It has no rights over Conditional Access, which requires the Conditional Access Administrator, Security Administrator, or Global Administrator role.

---

**Q29.** You want to grant a partner consultant temporary access to your subscription's resources for 30 days without creating a permanent guest account that must be manually removed later. What PIM feature addresses this most precisely?

**Answer: C) A PIM "Eligible" assignment with a defined end date/expiration on the assignment itself.**

A) A permanent Contributor assignment with a calendar reminder to remove it
B) A Conditional Access policy with a session control
C) A PIM "Eligible" assignment with a defined end date/expiration on the assignment itself
D) An access review with no automation

**Explanation:** PIM allows both eligible and active assignments to have a defined start and end date/time. Setting an expiration directly on the assignment automatically revokes access after the period — this is more precise and less error-prone than manual reminders or basic access reviews without automation.

---

**Q30.** A subscription-level Deny policy blocks creation of Public IP addresses (`Microsoft.Network/publicIPAddresses`). An Azure Bastion deployment (which internally provisions a Public IP as a managed dependency) is attempted. What happens?

**Answer: A) The Bastion deployment fails because the underlying Public IP resource creation is blocked by policy, even though the user isn't directly creating the Public IP.**

A) The Bastion deployment fails because the underlying Public IP resource creation is blocked by policy, even though the user isn't directly creating the Public IP
B) The Bastion deployment succeeds because Azure PaaS dependencies are exempt from policy
C) The policy only applies to user-initiated ARM calls, not internal service calls
D) Bastion automatically requests a policy exemption

**Explanation:** Azure Policy evaluates every resource creation request submitted through Azure Resource Manager, regardless of whether it's a direct user action or an implicit dependency of another service. If a policy denies Public IP creation, any deployment (including PaaS services like Bastion, VPN Gateway, or Application Gateway) that requires provisioning one as a sub-resource will fail unless explicitly exempted.

---

**Q31.** Which statement about combining Azure RBAC "Contributor" role with a resource-level "ReadOnly" lock is correct?

**Answer: B) The lock overrides the role — even Contributor/Owner cannot modify or delete the resource while the ReadOnly lock is active.**

A) The RBAC role takes precedence since it's more specific
B) The lock overrides the role — even Contributor/Owner cannot modify or delete the resource while the ReadOnly lock is active
C) They apply to different operation types and never conflict
D) The most recently applied of the two wins

**Explanation:** Resource locks operate independently of and above RBAC permission evaluation. A ReadOnly lock blocks write/delete operations for literally everyone, including Owners, regardless of their RBAC-granted permissions — this is one of the most frequently tested "gotchas" in the AZ-104 governance domain.

---

**Q32.** You need to give a team the ability to view cost and billing data for a subscription but not make any changes to resources. Which built-in RBAC role is most appropriate?

**Answer: C) Cost Management Reader.**

A) Reader
B) Billing Administrator (Entra ID role)
C) Cost Management Reader
D) Monitoring Reader

**Explanation:** `Cost Management Reader` is a built-in Azure RBAC role specifically scoped to viewing cost and billing information without resource-level read access. While `Reader` grants broader read access to all resources (not focused on cost), and `Billing Administrator` is an Entra ID directory role for managing billing/purchases (broader than just viewing), Cost Management Reader is the precise fit.

---

**Q33.** A dynamic device group uses the rule `(device.deviceOSType -eq "Windows") and (device.deviceTrustType -eq "ServerAD")`. Which devices will be included?

**Answer: B) Windows devices that are hybrid Entra ID joined (joined to on-premises AD and synced/registered to Entra ID).**

A) All Windows devices, including personal ones
B) Windows devices that are hybrid Entra ID joined (joined to on-premises AD and synced/registered to Entra ID)
C) Only Windows Server operating systems
D) Cloud-only Entra ID joined devices

**Explanation:** `deviceTrustType -eq "ServerAD"` specifically identifies Hybrid Entra ID joined devices (those with an on-premises AD join that are also registered in Entra ID) — not to be confused with "AzureAD" (cloud-only joined) or "Workplace" (registered/BYOD) trust types.

---

**Q34.** You attempt to move a subscription from Management Group A to Management Group B. Management Group A has a Deny policy on resource creation; Management Group B does not. What happens to existing non-compliant resources created under A's policy after the move?

**Answer: C) Existing resources are unaffected retroactively — policies (Deny in particular) only prevent NEW non-compliant actions at evaluation time; they don't retroactively delete or alter previously created resources.**

A) They are automatically deleted for non-compliance
B) They are automatically remediated to comply with B's policies
C) Existing resources are unaffected retroactively — policies (Deny in particular) only prevent NEW non-compliant actions at evaluation time; they don't retroactively delete or alter previously created resources
D) The move is blocked until all resources are compliant

**Explanation:** Deny policies only block create/update operations that would violate them at the time of the request — they have no retroactive effect on already-existing resources. Moving the subscription doesn't trigger deletion or forced remediation; it simply changes which policies apply going forward.

---

**Q35.** Which two conditions must both be true for a user to successfully activate a PIM "Eligible" role assignment when the role's activation settings require justification, MFA, and approval?

**Answer: A) The user must provide a valid justification and pass MFA, AND a designated approver must approve the request before the role becomes active.**

A) The user must provide a valid justification and pass MFA, AND a designated approver must approve the request before the role becomes active
B) The user only needs MFA; justification and approval are optional metadata
C) The user must be a Global Administrator to self-approve
D) Approval happens automatically after 1 hour if no approver responds

**Explanation:** When approval is required in a PIM activation policy, the role does NOT become active immediately upon the user's request — even after satisfying MFA and providing justification, the request sits pending until an approver explicitly approves it. Only then does the assignment become active for the configured duration.

---

**Q36.** A user is a member of two Entra ID groups, Group1 (assigned Reader at Subscription X) and Group2 (assigned a Deny assignment blocking write operations at Resource Group Y within Subscription X). What is the user's effective permission for writing to a resource in Resource Group Y?

**Answer: B) Write is denied, because the Deny assignment from Group2 applies regardless of the Reader role from Group1, and deny always overrides allow.**

A) Write is allowed since Reader from Group1 applies at a broader scope
B) Write is denied, because the Deny assignment from Group2 applies regardless of the Reader role from Group1, and deny always overrides allow
C) The permissions are averaged, resulting in read-only with occasional write bursts
D) The user must choose which group's permission applies

**Explanation:** Reinforcing the core precedence rule: Deny assignments always override Allow role assignments, regardless of which scope, group, or role granted the Allow. Even though Reader doesn't grant write anyway, this question tests understanding that Deny is absolute and independent of any Allow-granting role.

---

**Q37.** Your company wants a lightweight way to standardize resource groups, RBAC assignments, policy assignments, and ARM templates together as a single versioned, deployable package across new subscriptions, with tracked assignment history and the ability to lock down deployed resources from accidental modification. Historically, which Azure service was purpose-built for this (even though it's now in a deprecated/legacy state, and the exam sometimes still references it)?

**Answer: A) Azure Blueprints.**

A) Azure Blueprints
B) Azure Policy alone
C) Azure Resource Manager templates alone
D) Azure Automation

**Explanation:** Azure Blueprints packaged ARM templates, RBAC assignments, and Policy assignments together as a trackable, versioned artifact, and included "resourceGroups" locking to protect blueprint-deployed resources from later modification. Note: Blueprints is deprecated in favor of Template Specs + Deployment Stacks, but AZ-104 exam content (as of recent versions) may still reference it conceptually — check current exam objectives.

---

**Q38.** You need to grant a user the ability to create and manage Azure Policy assignments and definitions at the subscription scope, but nothing else. Which built-in RBAC role fits best?

**Answer: C) Resource Policy Contributor.**

A) Contributor
B) Owner
C) Resource Policy Contributor
D) Policy Insights Contributor (does not exist as a standard built-in role)

**Explanation:** `Resource Policy Contributor` is the precise built-in role for managing (creating/assigning) Azure Policy definitions, initiatives, and assignments without granting broader resource management rights that Contributor or Owner would include.

---

**Q39.** During a PIM activation with "Require Azure MFA" enabled, a user activates their eligible role from a session where they already completed MFA earlier that day (via Conditional Access). Will PIM require them to re-authenticate with MFA again?

**Answer: A) It depends on token freshness/claims — PIM checks whether the MFA claim in the current session token is fresh enough; if the existing MFA claim is still valid and recognized, re-prompting may be skipped, otherwise MFA is required again.**

A) It depends on token freshness/claims — PIM checks whether the MFA claim in the current session token is fresh enough; if the existing MFA claim is still valid and recognized, re-prompting may be skipped, otherwise MFA is required again
B) MFA is always re-required regardless of prior authentication
C) MFA is never required again once completed once per day
D) PIM ignores MFA claims from Conditional Access entirely and always uses its own separate prompt

**Explanation:** This is an advanced/tricky nuance often tested at higher difficulty: PIM's MFA requirement can be satisfied by a sufficiently fresh MFA claim already present in the user's token (e.g., from a Conditional Access-enforced MFA earlier in the session), but this isn't guaranteed and depends on claim freshness and configuration — it's not a strict "always/never."

---

**Q40.** A resource group contains a virtual machine with a `CanNotDelete` lock applied directly on the VM resource. The resource group itself has no lock. Can a user with Owner permissions delete the entire resource group?

**Answer: B) No — the resource group deletion will fail/be blocked because it cannot delete the locked VM inside it, even without a lock on the resource group itself.**

A) Yes, resource group deletion bypasses individual resource locks
B) No — the resource group deletion will fail/be blocked because it cannot delete the locked VM inside it, even without a lock on the resource group itself
C) Yes, but only the VM will be skipped while everything else deletes
D) The lock must first be manually removed by a different subscription

**Explanation:** Locks on individual resources are enforced even during a cascading resource group deletion. If any resource within the group has a `CanNotDelete` (or `ReadOnly`) lock, the resource group deletion operation will fail until that lock is removed — locks aren't bypassed just because the parent container itself is unlocked.

---

**Q41.** Which of the following is true regarding Entra ID roles versus Azure RBAC roles when it comes to the "Global Administrator" role and Azure subscription access?

**Answer: C) A Global Administrator can elevate themselves to gain Azure RBAC User Access Administrator rights at the root management group scope via a specific toggle, but does not have Azure resource access by default.**

A) Global Administrator automatically has Owner on all Azure subscriptions by default
B) Global Administrator has no way to gain Azure resource access
C) A Global Administrator can elevate themselves to gain Azure RBAC User Access Administrator rights at the root management group scope via a specific toggle, but does not have Azure resource access by default
D) Global Administrator and Owner are the exact same role, just named differently across products

**Explanation:** By default, Entra ID Global Administrators do NOT have access to Azure resources/subscriptions. However, Entra ID provides an explicit setting ("Access management for Azure resources," found in Entra ID properties) that a Global Admin can toggle to grant themselves the User Access Administrator RBAC role at the root (`/`) scope, enabling them to then assign any RBAC role anywhere in the tenant's Azure resources.

---

**Q42.** You configure a Conditional Access policy requiring a compliant device for accessing a specific application. A user signs in from a personal, unmanaged device. What is the outcome, assuming no other grant controls are configured as "OR" alternatives?

**Answer: B) Access is blocked, because the device does not satisfy the compliance requirement and no alternative satisfying control exists.**

A) Access is granted with a warning banner
B) Access is blocked, because the device does not satisfy the compliance requirement and no alternative satisfying control exists
C) Access is granted after MFA regardless of device compliance
D) Access is granted only for read operations

**Explanation:** Grant controls in Conditional Access can be configured to require ALL selected controls or ANY ONE of selected controls (OR logic). If "Require device to be marked as compliant" is the only control and it's not met, and there's no alternate "OR" path (like an app-enforced restriction or MFA fallback configured explicitly as an alternative), access is blocked outright.

---

**Q43.** A tag `CostCenter` is applied at the resource group level but NOT on individual resources within it. When viewing Cost Management reports filtered by the `CostCenter` tag, will costs from resources inside that resource group be included?

**Answer: B) No — tags are not automatically inherited by resources from their parent resource group for cost reporting purposes; each resource needs its own tag (or a policy to propagate it) to be correctly attributed.**

A) Yes, tags automatically cascade from resource group to resources for billing purposes
B) No — tags are not automatically inherited by resources from their parent resource group for cost reporting purposes; each resource needs its own tag (or a policy to propagate it) to be correctly attributed
C) Yes, but only for PaaS resources
D) Yes, if the resource group tag was applied before resource creation

**Explanation:** Unlike RBAC and Policy, tags do NOT automatically inherit from resource groups (or subscriptions) down to individual resources. This is a frequently tested distinction — you must use an Azure Policy with `Modify` effect (inherit tag policies) to explicitly propagate tags, or apply them manually/via IaC to each resource for accurate cost attribution.

---

**Q44.** A user is assigned the "Reports Reader" Entra ID role. What can they specifically access that a plain Global Reader could also access, but a regular Member user cannot?

**Answer: A) Sign-in and audit log reports and usage data insights, without broader administrative read access across the whole tenant configuration.**

A) Sign-in and audit log reports and usage data insights, without broader administrative read access across the whole tenant configuration
B) Full Conditional Access policy visibility
C) Billing and subscription cost data
D) Password reset audit trails with the ability to reset passwords

**Explanation:** `Reports Reader` is a narrowly scoped role granting access to usage reports, sign-in logs, and similar reporting data — a subset of what Global Reader can see, without the broader visibility into full tenant/administrative configuration that Global Reader provides.

---

**Q45.** You deploy an ARM template that includes a `Microsoft.Authorization/roleAssignments` resource to grant Contributor to a service principal at deployment time. The deployment is run by a user who only has Contributor (not Owner or User Access Administrator) on the target resource group. What happens?

**Answer: B) The deployment fails for that specific role assignment resource, because Contributor does NOT include `Microsoft.Authorization/roleAssignments/write` permission.**

A) The deployment succeeds because Contributor can deploy any ARM resource type
B) The deployment fails for that specific role assignment resource, because Contributor does NOT include `Microsoft.Authorization/roleAssignments/write` permission
C) The deployment succeeds, but the role assignment silently doesn't apply
D) The deployment prompts for elevated consent automatically

**Explanation:** Contributor explicitly excludes the ability to grant access to others (this is by design, per its role definition's `NotActions`, which excludes `Microsoft.Authorization/*/Write`). Attempting to deploy a role assignment resource without `Microsoft.Authorization/roleAssignments/write` permission (granted by Owner or User Access Administrator) will cause that portion of the deployment to fail with an authorization error.

---

**Q46.** Which scenario correctly describes when an Azure Policy "Audit" effect resource would show as "Non-compliant" in the compliance dashboard even though no Deny policy blocked its creation?

**Answer: C) The resource was created before the Audit policy was assigned, or was created without triggering evaluation, and a later compliance scan (which runs periodically, roughly every 24 hours, or can be triggered on-demand) evaluates it against the current policy and finds it doesn't meet the condition.**

A) Audit-effect resources are never marked non-compliant since they aren't blocked
B) Audit is only evaluated at policy assignment creation time, never again
C) The resource was created before the Audit policy was assigned, or was created without triggering evaluation, and a later compliance scan (which runs periodically, roughly every 24 hours, or can be triggered on-demand) evaluates it against the current policy and finds it doesn't meet the condition
D) Audit-effect resources require manual compliance marking by an administrator

**Explanation:** `Audit` doesn't block creation, but it still evaluates resource compliance — both at creation/update time AND during periodic compliance scans (approximately every 24 hours by default, or triggerable on-demand via `Start-AzPolicyComplianceScan` or the portal). This allows pre-existing non-compliant resources to be surfaced in reporting without ever being blocked.

---

**Q47.** You want a user to be able to create, read, update, and delete Key Vaults and manage their access policies, but NOT read the actual secrets/keys/certificates stored inside. Which combination correctly reflects Azure RBAC's separation of concerns for Key Vault (in RBAC-enabled Key Vaults, not legacy access policies)?

**Answer: A) Assign "Key Vault Contributor" (management plane — manages the vault resource itself) without assigning any Key Vault data-plane role like "Key Vault Secrets User," since data access is governed separately via DataActions.**

A) Assign "Key Vault Contributor" (management plane — manages the vault resource itself) without assigning any Key Vault data-plane role like "Key Vault Secrets User," since data access is governed separately via DataActions
B) Assign "Owner" since it's the only role with full Key Vault control
C) Assign "Key Vault Administrator" since it restricts secret access by default
D) This separation is not possible; management and data access are always bundled together

**Explanation:** Key Vault (with Azure RBAC permission model enabled) cleanly separates management-plane operations (create/delete the vault, configure network rules, etc. — via `Key Vault Contributor`) from data-plane operations (reading secrets/keys/certificates — via roles with `DataActions` like `Key Vault Secrets User`). This is a deliberately tested design showing that resource management access does not imply data access in Key Vault's RBAC model.

---

**Q48.** An initiative assignment has a policy with effect `Deny` and a defined "excludedScopes" parameter pointing to a specific resource group. Separately, a policy exemption exists for a different resource group under the same initiative. What's the functional difference between using `excludedScopes` on the assignment versus a Policy Exemption resource?

**Answer: B) `excludedScopes` is a static, permanent configuration set at assignment time (part of the assignment object) with no expiration or audit trail category; a Policy Exemption is a separate, trackable resource with expiration dates, categories (Waiver/Mitigated), and independent lifecycle management.**

A) There is no functional difference; they behave identically
B) `excludedScopes` is a static, permanent configuration set at assignment time (part of the assignment object) with no expiration or audit trail category; a Policy Exemption is a separate, trackable resource with expiration dates, categories (Waiver/Mitigated), and independent lifecycle management
C) excludedScopes only works for Audit effects, never Deny
D) Policy Exemptions require re-deploying the entire initiative

**Explanation:** Both mechanisms exclude a scope from policy enforcement, but they serve different governance purposes. `excludedScopes` is baked directly into the assignment and requires editing the assignment to change. Policy Exemptions are independent, purpose-built resources supporting expiration and categorized justification — making them more auditable and appropriate for temporary, tracked exceptions.

---

**Q49.** A subscription is configured with an Azure Policy that uses the `[[Deny]]` effect via a parameterized policy definition, where the effect parameter allows values `Audit`, `Deny`, or `Disabled`, and the assignment sets the parameter to `Disabled`. What is the practical outcome for resource compliance evaluation?

**Answer: A) The policy is assigned but performs no enforcement or evaluation — resources are not evaluated against its rule logic at all, and no compliance state is reported for it.**

A) The policy is assigned but performs no enforcement or evaluation — resources are not evaluated against its rule logic at all, and no compliance state is reported for it
B) Disabled behaves the same as Audit, just with a different UI label
C) Disabled temporarily pauses evaluation for 24 hours before reverting to the default effect
D) Disabled effect is invalid and causes the assignment to fail validation

**Explanation:** The `Disabled` effect (available on parameterized policy definitions that support it) fully turns off evaluation for that policy — it's commonly used to stage rollouts (assign the initiative broadly first with policies disabled, then selectively enable specific policies later) without needing to remove and re-add assignments.

---

**Q50.** You need to design an access strategy where: (1) standing/permanent access to production resources is minimized, (2) elevated access requires justification and time-bound activation, (3) guest collaborators are periodically re-certified, and (4) resource-level tagging for cost allocation is enforced automatically at creation. Which combination of features correctly addresses all four requirements?

**Answer: C) PIM eligible assignments (1 & 2) + Access Reviews on guest/group membership (3) + Azure Policy with Append/Modify effect for tags (4).**

A) Security Defaults + RBAC Owner assignments + manual guest audits + resource locks
B) Conditional Access alone for all four requirements
C) PIM eligible assignments (1 & 2) + Access Reviews on guest/group membership (3) + Azure Policy with Append/Modify effect for tags (4)
D) Azure Blueprints alone, since it covers RBAC, Policy, and guest management natively

**Explanation:** This capstone question requires synthesizing the whole domain: PIM directly addresses minimizing standing access and enforcing just-in-time, justified activation (requirements 1–2). Access Reviews are purpose-built for periodic guest/member recertification (requirement 3). Azure Policy's `Append` (add tag if missing, at creation) or `Modify` (add/update tag, including on existing resources) effects enforce automatic tagging (requirement 4). No single feature covers all four — correctly identifying the right tool per requirement is the core governance competency tested at this level.

---

## Study Notes: Recurring "Gotcha" Themes in This Set

1. **Deny assignments vs. NotActions vs. Deny policy effect** — three completely different mechanisms, easy to confuse.
2. **ReadOnly locks block far more than expected** (key rotation, VM restart, scaling) — CanNotDelete is narrower and often the better fit.
3. **RBAC/Policy inherit downward automatically and dynamically; Tags do NOT inherit** — must be explicitly propagated via Policy.
4. **Entra ID roles ≠ Azure RBAC roles** — directory objects vs. Azure resources, entirely separate permission systems.
5. **Global Administrator has zero Azure resource access by default** — requires explicit elevation.
6. **PIM activation nuances** — approval gating, MFA claim freshness, and automatic deactivation at max duration.
7. **Policy effect precedence and layering across management group hierarchy is cumulative, not overriding.**

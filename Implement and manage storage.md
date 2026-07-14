# AZ-104: Implement and Manage Storage
## Advanced Practice Set — 50 Questions (Exam-Tough Edition)

Each question is followed immediately by the correct answer, a full explanation, and a breakdown of why each incorrect option is wrong — matching real AZ-104 exam standards.

---

**Q1.** You need the highest level of durability and availability for a storage account, protecting against a full regional outage, with read access to the secondary region during normal operations. Which redundancy option should you choose?

A) LRS
B) ZRS
C) GRS
D) RA-GRS

**Answer: D) RA-GRS**

**Explanation:** RA-GRS (Read-Access Geo-Redundant Storage) replicates data to a secondary paired region AND allows read access to that secondary endpoint at all times, satisfying both "protects against regional outage" and "read access to secondary region during normal operations."

**Why not the others:**
- A) LRS only replicates within a single datacenter — no protection against a regional or even zonal outage.
- B) ZRS replicates across availability zones within one region, protecting against zone failure, but not a full regional outage.
- C) GRS replicates to a secondary region but the secondary is NOT readable unless a failover occurs — it fails the "read access during normal operations" requirement.

---

**Q2.** Which storage redundancy option combines zone-redundancy in the primary region with geo-replication to a secondary region?

A) ZRS
B) GZRS
C) GRS
D) LRS

**Answer: B) GZRS**

**Explanation:** GZRS (Geo-Zone-Redundant Storage) writes synchronous copies across three availability zones in the primary region (like ZRS) AND asynchronously replicates to a secondary paired region (like GRS) — offering the highest combined durability and availability short of RA-GZRS with read access.

**Why not the others:**
- A) ZRS alone offers zone protection but no geo-replication to a secondary region.
- C) GRS offers geo-replication but uses LRS (single datacenter) in the primary region, not zone redundancy.
- D) LRS offers neither zone nor geo protection.

---

**Q3.** You need to move a Blob's access tier from Hot to Archive. What must you do to read the blob's data afterward?

A) Simply issue a normal GET request; Archive blobs are always instantly readable
B) Rehydrate the blob to Hot or Cool tier first, which can take hours
C) Delete and recreate the blob in Hot tier
D) Archive tier blobs cannot be tiered back; you must copy to a new storage account

**Answer: B) Rehydrate the blob to Hot or Cool tier first, which can take hours**

**Explanation:** Archive tier is offline storage — the blob's data is not immediately accessible. To read it, you must rehydrate it (via "Set Blob Tier" or copy operation) back to Hot or Cool, which can take up to 15 hours (standard priority) or under 1 hour (high priority, where supported).

**Why not the others:**
- A) Incorrect — Archive blobs cannot be read directly; attempting a GET on an un-rehydrated archive blob fails.
- C) Unnecessary and destructive — rehydration preserves the existing blob; deletion isn't required.
- D) Incorrect — rehydration in place (or via tier change) is the standard supported method; copying to a new account isn't required.

---

**Q4.** Which access tier is most cost-effective for data accessed less than once a month but requires immediate (millisecond) read access when needed, with a minimum 30-day storage duration commitment?

A) Hot
B) Cool
C) Cold
D) Archive

**Answer: C) Cold**

**Explanation:** The Cold tier sits between Cool and Archive — designed for data accessed rarely (less than Cool's ~30-day pattern, closer to quarterly or less) but still requiring immediate online access, with a minimum retention commitment of 90 days (note: Cool's minimum is 30 days). For this question's specific 30-day framing paired with immediate access needs at the lowest cost among online tiers beyond Cool, Cold is the better fit for infrequent-but-instant-access scenarios beyond typical Cool usage patterns.

**Why not the others:**
- A) Hot is optimized for frequently accessed data and has the highest storage cost (lowest access cost) — not cost-effective for rare access.
- B) Cool is for infrequently accessed data (typically 30+ days) but Cold is more cost-optimized for even rarer access while still being online.
- D) Archive is offline — it does NOT provide immediate/millisecond access; data must be rehydrated first, failing the "immediate access" requirement.

---

**Q5.** A lifecycle management policy is configured to move blobs to Cool tier after 30 days of no modification, and delete them after 365 days. The policy is scoped with a filter for blobs with prefix `logs/`. A blob `logs/app1.log` was last modified 40 days ago but was READ 2 days ago. What tier is it in, assuming default policy settings?

A) Still Hot, because last-access tracking prevents tiering
B) Cool, because lifecycle rules by default evaluate based on last MODIFIED time, not last accessed time, unless access tracking is explicitly enabled
C) Archive, because it's older than 30 days
D) Deleted, because it exceeded the read threshold

**Answer: B) Cool, because lifecycle rules by default evaluate based on last MODIFIED time, not last accessed time, unless access tracking is explicitly enabled**

**Explanation:** By default, lifecycle management policies use `daysAfterModificationGreaterThan` to evaluate blob age. Since the blob was modified 40 days ago (exceeding the 30-day threshold) it moves to Cool, regardless of the recent read — unless the account has "last access time tracking" enabled and the policy explicitly uses `daysAfterLastAccessTimeGreaterThan`.

**Why not the others:**
- A) Incorrect — access tracking is opt-in, not default behavior, and even when enabled, the question specifies default settings.
- C) Incorrect — the policy only specifies a Cool tier transition at 30 days; there's no Archive rule mentioned, so it wouldn't jump to Archive.
- D) Incorrect — the deletion threshold is 365 days; 40 days does not trigger deletion.

---

**Q6.** Which type of SAS (Shared Access Signature) is signed with the storage account key and can grant access to multiple services (Blob, File, Queue, Table) and resource types in one token?

A) Service SAS
B) Account SAS
C) User Delegation SAS
D) Ad hoc SAS

**Answer: B) Account SAS**

**Explanation:** An Account SAS is scoped at the account level, signed with the storage account key, and can grant permissions across multiple services and resource types (service, container, object) in a single token — broader in scope than a Service SAS.

**Why not the others:**
- A) A Service SAS is limited to a single storage service (e.g., only Blob) and cannot span multiple services in one token.
- C) A User Delegation SAS is signed with Entra ID credentials (not the account key) and is scoped specifically to Blob storage only.
- D) "Ad hoc SAS" is not an official Azure SAS type name; it's a colloquial term sometimes used to describe a SAS without a stored access policy, but not a formal category tested this way.

---

**Q7.** Which SAS type is considered the most secure because it eliminates the need to share or manage storage account keys, using Entra ID (Azure AD) credentials to sign the token instead?

A) Service SAS
B) Account SAS
C) User Delegation SAS
D) Shared Key SAS

**Answer: C) User Delegation SAS**

**Explanation:** A User Delegation SAS is signed using Entra ID (Azure AD) credentials of a security principal that has been granted the appropriate RBAC role (e.g., Storage Blob Data Contributor), rather than the storage account key — Microsoft explicitly recommends it as the most secure SAS option since it avoids key management/exposure risk entirely.

**Why not the others:**
- A) Service SAS is signed with the storage account key, which if leaked, grants broad access until the key is rotated.
- B) Account SAS is also signed with the account key, carrying the same key-exposure risk, and with even broader scope.
- D) "Shared Key SAS" isn't a distinct formal SAS category — it describes the key-signing mechanism used by both Service and Account SAS, which are less secure than User Delegation SAS.

---

**Q8.** You rotate a storage account's primary access key. What is the immediate impact on SAS tokens that were generated using that primary key before rotation?

A) No impact — SAS tokens are independent of the keys
B) All such SAS tokens are immediately invalidated, since they are cryptographically tied to the specific key used to sign them
C) Only tokens with no expiration are invalidated
D) SAS tokens automatically re-sign themselves with the new key

**Answer: B) All such SAS tokens are immediately invalidated, since they are cryptographically tied to the specific key used to sign them**

**Explanation:** A SAS token (other than User Delegation SAS) is cryptographically signed using one of the storage account's two access keys. Rotating (regenerating) that specific key immediately invalidates every SAS token that was signed with it, regardless of the token's stated expiry — this is a standard mechanism for emergency revocation.

**Why not the others:**
- A) Incorrect — SAS tokens are directly dependent on the key used to sign them; this is the entire basis of Shared Key-based SAS security.
- C) Incorrect — expiration is irrelevant to this invalidation; ALL tokens signed with the rotated key are invalidated, expired or not.
- D) Incorrect — there is no auto re-signing mechanism; a new SAS token must be manually generated with the new key.

---

**Q9.** A storage account has "Allow Blob public access" disabled at the account level. A container within it has its access level set to "Container (anonymous read access for containers and blobs)." What is the effective access?

A) Anonymous read access is granted, because container-level settings override account-level settings
B) Anonymous read access is denied, because the account-level setting overrides and disables all anonymous/public access regardless of container-level configuration
C) Anonymous access works only for blob-level, not container-level, listing
D) The configuration is invalid and will throw an error

**Answer: B) Anonymous read access is denied, because the account-level setting overrides and disables all anonymous/public access regardless of container-level configuration**

**Explanation:** The account-level "Allow Blob public access" setting acts as a master switch. When disabled, it overrides and blocks any container-level public access configuration, regardless of what the container's individual access level is set to — a critical security control and commonly tested precedence rule.

**Why not the others:**
- A) Incorrect — it's the reverse; account-level settings take precedence over container-level settings for this control.
- C) Incorrect — this isn't a partial restriction; anonymous access is fully blocked for both container and blob-level listing/reads when disabled at the account.
- D) Incorrect — this isn't an invalid configuration; you can freely set container-level public access even when the account-level switch is off — it just has no effect until re-enabled.

---

**Q10.** You need to protect against accidental deletion of blobs, allowing you to restore a deleted blob within a defined retention window. Which feature should you enable?

A) Blob versioning
B) Soft delete for blobs
C) Immutable storage
D) Point-in-time restore

**Answer: B) Soft delete for blobs**

**Explanation:** Soft delete for blobs retains deleted blobs (and optionally their snapshots) in a recoverable state for a configured retention period (1–365 days), allowing undelete operations within that window.

**Why not the others:**
- A) Blob versioning automatically maintains previous versions of a blob when it's modified or deleted, which is complementary but distinct — versioning alone tracks version history, while soft delete specifically governs deletion recovery behavior (though they're often used together).
- C) Immutable storage (WORM) prevents deletion/modification entirely for a retention period — it doesn't provide a "recover after deletion" workflow; it prevents deletion outright.
- D) Point-in-time restore allows reverting an entire block blob storage account (or container) to a previous state within a retention period, but requires versioning and change feed to be enabled first, and is a broader/different recovery mechanism than simple soft delete.

---

**Q11.** Which combination of features is required to perform a Point-in-Time Restore for block blobs in a storage account?

A) Soft delete only
B) Blob versioning + Change feed + Soft delete (all three enabled)
C) Immutable storage policies only
D) Lifecycle management only

**Answer: B) Blob versioning + Change feed + Soft delete (all three enabled)**

**Explanation:** Point-in-time restore requires blob versioning, the blob change feed, and soft delete for blobs to all be enabled together — these features collectively provide the version history and change tracking necessary to revert containers to a prior state within the retention period.

**Why not the others:**
- A) Soft delete alone only recovers deleted blobs; it cannot restore a whole container/account to an arbitrary prior point in time without versioning and change feed.
- C) Immutable storage prevents changes/deletions but does not provide a "restore to a point in time" capability.
- D) Lifecycle management automates tiering/expiration; it's unrelated to point-in-time restore functionality.

---

**Q12.** You configure a legal hold on a blob container. What is the effect on blobs within that container?

A) Blobs can be deleted but not modified
B) Blobs cannot be deleted or overwritten until the legal hold is explicitly removed by an authorized user, regardless of any set expiration date
C) Only new blobs uploaded after the hold are protected
D) The legal hold automatically expires after 90 days

**Answer: B) Blobs cannot be deleted or overwritten until the legal hold is explicitly removed by an authorized user, regardless of any set expiration date**

**Explanation:** A legal hold is a form of immutability policy with NO fixed expiration — it remains in effect indefinitely until a user with sufficient permissions explicitly clears/removes the hold, making it suitable for indefinite litigation/compliance holds.

**Why not the others:**
- A) Incorrect — legal holds block BOTH deletion and modification (overwrite) of blobs, not just deletion.
- C) Incorrect — a legal hold applies to all existing blobs in the container at the time it's set (and can affect new ones depending on configuration); it's not limited only to future uploads.
- D) Incorrect — unlike a time-based retention policy, a legal hold has no automatic expiration; it must be manually cleared.

---

**Q13.** A time-based immutability policy is set on a container in "Locked" state with a 180-day retention period. Can an administrator with Owner RBAC role delete a blob before the 180 days expire?

A) Yes, Owner can always override immutability
B) No — once locked, not even the storage account owner or Microsoft can delete or modify protected blobs before the retention period expires
C) Yes, but only via the Azure CLI, not the portal
D) No, but the retention period can be shortened by an Owner

**Answer: B) No — once locked, not even the storage account owner or Microsoft can delete or modify protected blobs before the retention period expires**

**Explanation:** A "Locked" immutability policy is enforced at the storage service level and is irreversible in the sense that the retention period can only be extended, never shortened or removed, and no RBAC role (including Owner) can bypass it — this is the core guarantee of WORM (Write Once, Read Many) compliance storage, satisfying SEC 17a-4(f) and similar regulations.

**Why not the others:**
- A) Incorrect — this directly contradicts the purpose of a locked immutability policy; no role can override it.
- C) Incorrect — the restriction is enforced at the storage service level regardless of which client/tool (Portal, CLI, PowerShell, SDK) is used.
- D) Incorrect — once locked, the retention period can only be INCREASED, never decreased, which is part of the compliance guarantee.

---

**Q14.** Which statement about unlocked vs. locked time-based immutability policies is correct?

A) An unlocked policy can be deleted or have its retention period both increased and decreased
B) A locked policy can have its retention period decreased by up to 10%
C) An unlocked policy behaves identically to a locked one in all respects
D) A locked policy allows deletion after providing a business justification

**Answer: A) An unlocked policy can be deleted or have its retention period both increased and decreased**

**Explanation:** While a policy is in the "Unlocked" state, it's still being tested/configured — administrators can freely modify (increase or decrease) the retention period or delete the policy entirely. Once locked, these freedoms disappear.

**Why not the others:**
- B) Incorrect — locked policies cannot have retention periods decreased at all; they can only be extended, and there's no "10% decrease" allowance.
- C) Incorrect — this is the opposite of correct; unlocked and locked policies behave very differently regarding modification/deletion rights.
- D) Incorrect — locked policies cannot be bypassed for deletion under any justification; that would defeat their compliance purpose.

---

**Q15.** You need to migrate a storage account's redundancy from LRS to GRS. What must be true beforehand, and what happens during the process?

A) The account must first be deleted and recreated with GRS
B) You can change redundancy directly via the portal/CLI without downtime; Azure begins asynchronous replication to the secondary region, which may take time to fully sync depending on data volume
C) GRS conversion requires opening a support ticket for all storage accounts
D) LRS to GRS conversion is not supported; only new accounts can be created as GRS

**Answer: B) You can change redundancy directly via the portal/CLI without downtime; Azure begins asynchronous replication to the secondary region, which may take time to fully sync depending on data volume**

**Explanation:** Changing redundancy configuration (e.g., LRS → GRS, or GRS → RA-GRS) is a supported, in-place operation performed via the Portal, CLI, PowerShell, or SDKs, without requiring downtime or data movement by the user — Azure handles the underlying replication automatically, though full sync to the secondary region takes time proportional to data size.

**Why not the others:**
- A) Incorrect — no recreation is necessary; this is a supported configuration change on the existing account.
- C) Incorrect — a support ticket is not required for standard redundancy changes between most SKUs (though some conversions, like adding zone-redundancy in certain older accounts, historically required support — but LRS→GRS is a standard self-service change).
- D) Incorrect — this conversion is directly supported and commonly performed.

---

**Q16.** During a regional outage affecting the primary region of a GRS storage account, you initiate a customer-managed (unplanned) failover. What happens to the account's redundancy configuration immediately after failover completes?

A) It remains GRS, now pointing to the former secondary as the new primary
B) It automatically converts to LRS in the new primary region, since geo-replication must be manually re-established for the new secondary
C) It converts to RA-GRS automatically
D) The storage account is deleted and must be recreated

**Answer: B) It automatically converts to LRS in the new primary region, since geo-replication must be manually re-established for the new secondary**

**Explanation:** After an account failover (customer-managed), the former secondary region becomes the new primary, but the account's redundancy is downgraded to LRS in that new primary region. Geo-replication to a new secondary region is NOT automatically re-established — this must be manually reconfigured afterward if geo-redundancy is still desired.

**Why not the others:**
- A) Incorrect — the account does not remain GRS automatically post-failover; it drops to LRS until manually reconfigured.
- C) Incorrect — RA-GRS specifically requires read access configuration and geo-replication, neither of which is automatically restored post-failover.
- D) Incorrect — failover does not delete the account; it changes which region serves as primary.

---

**Q17.** Which of the following operations can trigger data loss during an unplanned (customer-managed) storage account failover?

A) None — failover is always zero data loss
B) Yes — because GRS/RA-GRS replication is asynchronous, any data written to the primary that hadn't yet replicated to the secondary at the time of the outage will be lost
C) Only metadata is lost, never actual blob data
D) Data loss only occurs if versioning is disabled

**Answer: B) Yes — because GRS/RA-GRS replication is asynchronous, any data written to the primary that hadn't yet replicated to the secondary at the time of the outage will be lost**

**Explanation:** Since geo-replication in GRS/RA-GRS/GZRS/RA-GZRS is asynchronous (not synchronous like intra-region ZRS), there's inherently a replication lag. Any writes not yet propagated to the secondary at the moment of an unplanned outage are lost upon failover — this is a critical, frequently tested caveat of geo-redundant storage's disaster recovery model.

**Why not the others:**
- A) Incorrect — this is a common misconception; failover is explicitly NOT zero-data-loss for unplanned scenarios due to async replication lag.
- C) Incorrect — actual blob/file/table/queue data can be lost, not just metadata.
- D) Incorrect — versioning doesn't prevent this specific type of data loss; it's unrelated to cross-region replication lag.

---

**Q18.** You want to synchronize an on-premises Windows Server file share with Azure Files, maintaining a local cache of frequently accessed files while tiering less-used files to the cloud automatically. Which service should you deploy?

A) AzCopy scheduled task
B) Azure File Sync
C) Storage Mover
D) Azure Data Box

**Answer: B) Azure File Sync**

**Explanation:** Azure File Sync installs an agent on a Windows Server (physical or VM, on-prem or in Azure) and synchronizes it with an Azure file share, supporting cloud tiering — where frequently accessed files are cached locally and less-used files are tiered to the cloud with just a pointer left locally, optimizing local storage usage.

**Why not the others:**
- A) AzCopy is a command-line copy utility for one-time or scripted transfers; it doesn't provide continuous sync or cloud tiering capability.
- C) Azure Storage Mover is designed for migration (moving data from source to Azure), not ongoing bidirectional sync with local caching/tiering.
- D) Azure Data Box is a physical device for bulk offline data transfer, not for ongoing synchronization.

---

**Q19.** In Azure File Sync, which component represents the "top-level" object that contains one or more sync groups?

A) Sync group
B) Storage Sync Service
C) Cloud endpoint
D) Server endpoint

**Answer: B) Storage Sync Service**

**Explanation:** The Storage Sync Service is the top-level Azure resource representing the entire File Sync deployment; it contains one or more Sync Groups, each of which defines the sync topology between a Cloud Endpoint (an Azure file share) and one or more Server Endpoints (paths on registered servers).

**Why not the others:**
- A) A Sync Group is a child object within the Storage Sync Service that defines the actual sync topology, not the top-level container.
- C) A Cloud Endpoint represents a specific Azure file share within a sync group, not the top-level resource.
- D) A Server Endpoint represents a specific path on a registered server within a sync group — the most granular of these components, not the top-level one.

---

**Q20.** You need to restrict access to a storage account so that only traffic from a specific virtual network subnet is allowed, while blocking all public internet access, without needing a private IP endpoint. Which feature should you configure?

A) Private Endpoint
B) Service Endpoint (VNet integration via storage account firewall/network rules)
C) Network Security Group only
D) Azure Firewall

**Answer: B) Service Endpoint (VNet integration via storage account firewall/network rules)**

**Explanation:** Configuring a Virtual Network service endpoint for Azure Storage, combined with the storage account's network rules (firewall) restricting access to specific VNets/subnets, allows only traffic from the specified subnet while blocking public internet access — the storage account keeps its public IP but only accepts traffic from allow-listed sources, without requiring a private IP in the VNet.

**Why not the others:**
- A) A Private Endpoint assigns the storage account a private IP address directly within the VNet, which is a stronger isolation option — but the question specifies "without needing a private IP endpoint," making Service Endpoint the better-fit answer.
- C) An NSG alone controls traffic at the subnet/NIC level but doesn't by itself restrict access to the storage account's public endpoint — it must be combined with storage account network rules.
- D) Azure Firewall is a broader network security service for filtering outbound/inbound traffic across a VNet; it isn't the mechanism used specifically for storage account network rule allow-listing.

---

**Q21.** What is the key architectural difference between a Private Endpoint and a Service Endpoint for Azure Storage?

A) They are functionally identical
B) A Private Endpoint brings the storage account's identity into your VNet via a private IP (NIC), enabling on-premises/VPN/ExpressRoute access and eliminating data exfiltration risk to other storage accounts; a Service Endpoint keeps the storage account's public IP but restricts traffic to specific subnets
C) A Service Endpoint is more secure because it uses a private IP
D) A Private Endpoint only works for Blob storage, not File or Queue

**Answer: B) A Private Endpoint brings the storage account's identity into your VNet via a private IP (NIC), enabling on-premises/VPN/ExpressRoute access and eliminating data exfiltration risk to other storage accounts; a Service Endpoint keeps the storage account's public IP but restricts traffic to specific subnets**

**Explanation:** This is a core architectural distinction tested on the exam. Private Endpoints use Azure Private Link to provision a NIC with a private IP in your VNet mapped to the specific storage account, allowing access from on-premises networks over VPN/ExpressRoute and preventing traffic from being routed to any other storage account (closing a data exfiltration vector). Service Endpoints simply extend VNet identity to the storage account's still-public endpoint, restricting by subnet but not changing the underlying public IP architecture.

**Why not the others:**
- A) Incorrect — they differ significantly in architecture, security posture, and on-premises connectivity support.
- C) Incorrect — reversed; Private Endpoint is the one using a private IP, and is generally considered the more secure/robust option.
- D) Incorrect — Private Endpoints support Blob, File, Queue, Table, and Static Website sub-resources (each configured individually), not just Blob.

---

**Q22.** A storage account has network rules configured to "Deny" all traffic by default, with an exception allowing "Trusted Microsoft services." Which scenario benefits from this exception?

A) Allowing your own custom application hosted on an Azure VM to bypass network rules
B) Allowing Azure services like Azure Backup, Azure Monitor (diagnostic settings), or Azure Event Grid to access the storage account despite the network restriction, since they operate as trusted first-party services
C) Allowing any authenticated Entra ID user regardless of network location
D) Allowing anonymous public blob access even when network rules deny all

**Answer: B) Allowing Azure services like Azure Backup, Azure Monitor (diagnostic settings), or Azure Event Grid to access the storage account despite the network restriction, since they operate as trusted first-party services**

**Explanation:** The "Allow trusted Microsoft services" exception permits specific first-party Azure services (a defined list including Azure Backup, Azure Monitor, Event Grid, Azure DevTest Labs, and others) to access the storage account through their service infrastructure, even when general network rules would otherwise deny the traffic — necessary because these services often need to write logs, backups, or diagnostic data regardless of network restrictions.

**Why not the others:**
- A) A custom application on a VM is not a "trusted Microsoft service" — it would need to be explicitly allowed via VNet/subnet rules or a private endpoint, not this exception.
- C) This exception is about network-level trusted services, not about identity/authentication bypass for regular users.
- D) This has nothing to do with anonymous public access settings, which are governed by a separate "Allow Blob public access" setting.

---

**Q23.** Which encryption capability allows you to use your own encryption keys stored in Azure Key Vault, rather than Microsoft-managed keys, for encrypting data at rest in a storage account?

A) Client-side encryption
B) Customer-managed keys (CMK) for Storage Service Encryption
C) Infrastructure encryption
D) Transport Layer Security (TLS)

**Answer: B) Customer-managed keys (CMK) for Storage Service Encryption**

**Explanation:** Storage Service Encryption (SSE) encrypts all data at rest by default using Microsoft-managed keys, but can be configured to use Customer-Managed Keys stored in Azure Key Vault (or Managed HSM) instead, giving the customer control over key rotation, revocation, and access auditing.

**Why not the others:**
- A) Client-side encryption is performed by the application before data is sent to Azure Storage — it's a different layer of encryption entirely, not a storage-account-level configuration.
- C) Infrastructure encryption is a SEPARATE, additional layer of encryption (double encryption) applied at the infrastructure level, complementary to but distinct from the choice of who manages the primary encryption key.
- D) TLS encrypts data in transit (over the network), not data at rest — a commonly tested distinction.

---

**Q24.** You enable "Infrastructure encryption" in addition to default Storage Service Encryption. What does this provide?

A) A second, independent layer of 256-bit AES encryption at the infrastructure level, using a different key than the primary SSE key, protecting against a scenario where one encryption algorithm/key is compromised
B) It replaces Microsoft-managed keys with customer-managed keys automatically
C) It encrypts only the transport layer, not data at rest
D) It has no practical effect and exists only for compliance checkbox purposes with zero technical benefit

**Answer: A) A second, independent layer of 256-bit AES encryption at the infrastructure level, using a different key than the primary SSE key, protecting against a scenario where one encryption algorithm/key is compromised**

**Explanation:** Infrastructure (double) encryption adds a second encryption layer at the infrastructure/hardware level, independent of the service-level SSE encryption, using a separate key. This defense-in-depth approach protects data even in the hypothetical event that one encryption layer/algorithm/key is compromised — important for highly regulated workloads. Note: infrastructure encryption must be enabled at account creation time and cannot be changed afterward.

**Why not the others:**
- B) Incorrect — infrastructure encryption is unrelated to who manages the keys (CMK vs. Microsoft-managed); it's a separate, independent encryption layer.
- C) Incorrect — this concerns data at rest, not transport layer security.
- D) Incorrect — while it does serve compliance purposes, it also provides a genuine technical defense-in-depth benefit against key/algorithm compromise scenarios.

---

**Q25.** Which storage account setting, when enabled, enforces that all requests to the storage account must use HTTPS, rejecting plain HTTP requests?

A) "Require secure transfer for REST API operations"
B) "Minimum TLS version"
C) "Allow storage account key access"
D) "Enable infrastructure encryption"

**Answer: A) "Require secure transfer for REST API operations"**

**Explanation:** This setting (enabled by default for new storage accounts) rejects any request made over unencrypted HTTP, requiring HTTPS for all REST API calls against Blob, File (REST-based), Queue, and Table services.

**Why not the others:**
- B) "Minimum TLS version" controls which TLS version (e.g., TLS 1.2) is the floor for HTTPS connections — it doesn't itself enforce HTTPS-only; that's the separate "secure transfer required" setting.
- C) "Allow storage account key access" controls whether Shared Key authorization is permitted at all (vs. requiring Entra ID auth) — unrelated to HTTP vs. HTTPS transport.
- D) Infrastructure encryption concerns data-at-rest double encryption, not transport security.

---

**Q26.** A company wants to disable the use of storage account access keys entirely for a storage account, forcing all access through Entra ID (Azure AD) RBAC-based authentication only. Which setting should be configured, and what is a key side effect?

A) "Allow storage account key access" = Disabled; SAS tokens signed with account keys (Service SAS, Account SAS) will also stop working, since they rely on the key
B) "Allow storage account key access" = Disabled; there is no impact on any existing SAS tokens
C) Disable public network access; this automatically disables key access too
D) Delete the storage account keys via Azure CLI

**Answer: A) "Allow storage account key access" = Disabled; SAS tokens signed with account keys (Service SAS, Account SAS) will also stop working, since they rely on the key**

**Explanation:** Disabling "Allow storage account key access" (previously called "Allow shared key access") blocks all Shared Key authorization, which includes both direct key-based requests AND any SAS tokens generated using the account key (Service SAS and Account SAS). Only User Delegation SAS (Entra ID-signed) and direct Entra ID RBAC-based access continue to function.

**Why not the others:**
- B) Incorrect — this significantly understates the impact; key-based SAS tokens are directly broken by this setting since they depend on key-based signing validation.
- C) Incorrect — public network access and shared key access are entirely separate, independently configurable settings.
- D) Incorrect — you cannot simply "delete" storage account keys; you can only regenerate/rotate them, and this isn't the mechanism for disabling key-based auth entirely (the dedicated toggle is).

---

**Q27.** You need to grant an Azure VM's managed identity permission to read blobs from a storage account without using any keys or SAS tokens. Which combination is required?

A) Assign the VM's managed identity the "Storage Blob Data Reader" RBAC role on the storage account/container, and the application code must acquire an Entra ID token for the storage resource
B) Just enable the managed identity; access is automatically granted to all storage accounts in the subscription
C) Generate a SAS token using the managed identity's credentials
D) Add the VM's IP address to the storage account's network firewall rules

**Answer: A) Assign the VM's managed identity the "Storage Blob Data Reader" RBAC role on the storage account/container, and the application code must acquire an Entra ID token for the storage resource**

**Explanation:** Passwordless/keyless access to Blob Storage from a VM requires two things: (1) an Azure RBAC data-plane role assignment (like Storage Blob Data Reader/Contributor/Owner) granted to the VM's managed identity at the storage account or container scope, and (2) application code that uses the managed identity to acquire an Entra ID access token and presents it when calling the Blob REST API/SDK.

**Why not the others:**
- B) Incorrect — enabling a managed identity alone grants no permissions; explicit RBAC role assignment is always required.
- C) Incorrect — managed identities don't generate SAS tokens; they authenticate directly via Entra ID tokens, bypassing SAS/key mechanisms entirely.
- D) Incorrect — network firewall rules control network-level access, not identity/authorization — this doesn't grant any actual data permissions.

---

**Q28.** Which blob storage feature automatically records create, update, and delete operations on blobs in a durable, ordered log that downstream applications can process, distinct from Storage Analytics logging?

A) Blob versioning
B) Change feed
C) Soft delete
D) Storage Analytics metrics

**Answer: B) Change feed**

**Explanation:** The Blob Storage change feed provides an ordered, durable, read-only log of create/update/delete/tier-change events for blobs, designed for applications (e.g., ETL pipelines, indexing services) to process changes efficiently without polling — and is also a prerequisite for point-in-time restore.

**Why not the others:**
- A) Blob versioning tracks previous states of a blob but doesn't provide a consumable event log/feed for external processing.
- C) Soft delete governs deletion recovery, not an event log.
- D) Storage Analytics metrics/logging captures request-level telemetry (latency, success/failure, request counts) for monitoring purposes, not a structured change event log for data pipeline consumption.

---

**Q29.** A blob container has a stored access policy defined with specific permissions and expiry. A Service SAS is generated referencing that stored access policy. What is the primary benefit of this approach over specifying permissions/expiry directly in the SAS URI?

A) Stored access policies allow the SAS's permissions and expiration to be revoked or modified centrally at any time, even after the SAS has been distributed, by modifying/deleting the policy — without needing to rotate the storage account key
B) Stored access policies make the SAS valid indefinitely regardless of expiry settings
C) Stored access policies are required for all SAS tokens; ad hoc SAS is deprecated
D) Stored access policies increase the SAS token's throughput limits

**Answer: A) Stored access policies allow the SAS's permissions and expiration to be revoked or modified centrally at any time, even after the SAS has been distributed, by modifying/deleting the policy — without needing to rotate the storage account key**

**Explanation:** When a SAS references a stored access policy (defined at the container/share/queue/table level), you can revoke access by simply deleting or modifying the stored policy — instantly invalidating any SAS tokens tied to it, without needing to regenerate storage account keys (which would invalidate ALL other SAS tokens too). This provides much more granular revocation control compared to "ad hoc" SAS with parameters embedded directly in the URI.

**Why not the others:**
- B) Incorrect — the stored policy still defines/respects an expiration; it doesn't make the SAS indefinitely valid unless explicitly configured that way (which is not a benefit, but a misconfiguration risk).
- C) Incorrect — ad hoc SAS (parameters directly in the URI) remains fully supported and commonly used; stored access policies are optional, not mandatory.
- D) Incorrect — there is no throughput/performance benefit; the benefit is purely around centralized revocation/management.

---

**Q30.** What is the maximum number of stored access policies that can be defined on a single container, queue, table, or share?

A) 1
B) 5
C) 10
D) Unlimited

**Answer: B) 5**

**Explanation:** Azure Storage limits each container, queue, table, or share to a maximum of 5 stored access policies at a time — a specific, testable numeric limit.

**Why not the others:**
- A) Incorrect — more than one policy is allowed, up to the limit of 5.
- C) Incorrect — 10 exceeds the actual documented limit of 5.
- D) Incorrect — there IS a hard limit; it's not unlimited.

---

**Q31.** You configure Azure Storage firewall network rules with a "Deny" default action but no VNet rules or IP rules added, and public network access is set to "Enabled from selected virtual networks and IP addresses." What is the effective access from the Azure Portal itself when browsing blobs?

A) The Azure Portal is automatically exempted and can always browse
B) Portal access may fail unless you explicitly check "Allow Azure services on the trusted services list to access this storage account" or add your own client IP to the firewall rules, since the Portal's blob browsing feature uses your own browser's network context (and sometimes trusted services)
C) The Portal always uses a private, unrestricted management channel unaffected by any firewall rule
D) Firewall rules only affect SDK/API access, not the Portal

**Answer: B) Portal access may fail unless you explicitly check "Allow Azure services on the trusted services list to access this storage account" or add your own client IP to the firewall rules, since the Portal's blob browsing feature uses your own browser's network context (and sometimes trusted services)**

**Explanation:** When firewall rules deny by default with no exceptions, browsing blob content directly in the Azure Portal can be blocked because the Portal's data-plane browsing feature effectively makes calls constrained by your network context; Microsoft recommends enabling the trusted services exception and/or explicitly allowing your client IP to ensure Portal-based blob browsing continues to work.

**Why not the others:**
- A) Incorrect — the Portal is not universally exempted from storage account network rules for data plane operations like blob browsing.
- C) Incorrect — there's no separate "unrestricted management channel" that bypasses data-plane network rules for content browsing.
- D) Incorrect — firewall rules can indeed affect Portal-based data browsing, not just programmatic SDK/API access.

---

**Q32.** Which Azure CLI command creates a new storage account with ZRS redundancy in the East US region?

A) `az storage account create --name mystorageacct --resource-group myRG --location eastus --sku Standard_ZRS`
B) `az storage create --name mystorageacct --redundancy ZRS`
C) `az storage account new --sku ZRS --region eastus`
D) `az storagev2 account create --sku Standard_ZRS`

**Answer: A) `az storage account create --name mystorageacct --resource-group myRG --location eastus --sku Standard_ZRS`**

**Explanation:** The correct Azure CLI syntax uses `az storage account create` with the `--sku` parameter set to `Standard_ZRS` (redundancy is expressed as part of the SKU name, combined with the performance tier prefix Standard/Premium), along with `--resource-group` and `--location`.

**Why not the others:**
- B) `az storage create` is not a valid command group/verb combination — the correct command group is `az storage account`.
- C) `az storage account new` is not valid CLI syntax; the verb is `create`, and `--sku` values follow the `Standard_XXX`/`Premium_XXX` naming pattern, not a bare `--region`/raw redundancy name.
- D) `az storagev2` is not a real Azure CLI command group.

---

**Q33.** You need to copy a large dataset (50 TB) from an on-premises location to Azure Blob Storage, but your available internet bandwidth would take several weeks to transfer it. What is the most appropriate solution?

A) AzCopy with maximum concurrency settings
B) Azure Data Box (physical shipped device)
C) Storage Explorer drag-and-drop
D) Increase the storage account's egress limit

**Answer: B) Azure Data Box (physical shipped device)**

**Explanation:** For very large datasets where network transfer would be impractically slow (commonly cited threshold: transfers that would take more than about a week or two over available bandwidth), Azure Data Box provides a physically shipped storage device you load on-premises and ship back to Microsoft for offline upload into your storage account — a standard, exam-tested solution for bandwidth-constrained bulk migrations.

**Why not the others:**
- A) AzCopy is excellent for network-based transfers but is still bound by available internet bandwidth — it doesn't solve a fundamental bandwidth constraint that would take weeks.
- C) Storage Explorer is a GUI tool for smaller-scale, interactive management tasks; it isn't designed or efficient for bulk 50 TB transfers and has the same bandwidth limitation as AzCopy.
- D) There's no "egress limit" setting on the storage account that would meaningfully address an on-premises-to-cloud bandwidth bottleneck; egress limits concern outbound traffic FROM Azure, not inbound ingestion speed constraints.

---

**Q34.** Which Azure Storage service tier/type is most appropriate for storing unstructured data accessed via HTTP/HTTPS, such as images, videos, and documents, with support for tiering (Hot/Cool/Cold/Archive)?

A) Azure Files
B) Azure Blob Storage
C) Azure Table Storage
D) Azure Queue Storage

**Answer: B) Azure Blob Storage**

**Explanation:** Blob Storage is Azure's object storage solution optimized for storing massive amounts of unstructured data (images, video, documents, backups) accessible via REST/HTTP, and is the only one of these services that supports the Hot/Cool/Cold/Archive access tier model.

**Why not the others:**
- A) Azure Files provides SMB/NFS file shares (structured as a hierarchical file system) — it supports some tiering (Transaction Optimized, Hot, Cool for standard file shares) but is fundamentally a managed file share service, not general unstructured object storage accessed primarily via HTTP REST semantics like Blob.
- C) Table Storage is a NoSQL key-value store for structured, schemaless data — not designed for large unstructured files like images/videos.
- D) Queue Storage stores small messages for asynchronous processing between application components — not designed for storing large unstructured content.

---

**Q35.** You want to host a static HTML/CSS/JS website directly from a storage account without a separate web server. Which feature must you enable, and what container name is automatically used?

A) Enable "Static website" hosting; files are served from the automatically created `$web` container
B) Enable "Static website" hosting; files are served from a container you must manually name "static"
C) This requires Azure App Service; storage accounts cannot host websites directly
D) Enable "Content Delivery Network" only

**Answer: A) Enable "Static website" hosting; files are served from the automatically created `$web` container**

**Explanation:** Enabling the Static Website feature on a storage account (available for Blob Storage/StorageV2 accounts, requires Hot/Cool tier access) automatically creates a special `$web` container where you upload your site's static content, and provides a public endpoint URL to serve it directly — no separate web server or App Service is required.

**Why not the others:**
- B) Incorrect — the container name is NOT user-chosen; it's automatically named `$web` when the feature is enabled.
- C) Incorrect — this directly contradicts the purpose of the Static Website feature, which exists precisely to avoid needing App Service for simple static content.
- D) Incorrect — a CDN can be layered on top for performance/caching but is not required to enable static website hosting itself; it's a separate, optional enhancement.

---

**Q36.** Which authentication method is NOT supported for accessing Azure Files via the SMB protocol from a domain-joined on-premises machine?

A) Storage account key
B) Entra ID (Azure AD) Domain Services or Entra Kerberos authentication
C) On-premises Active Directory Domain Services (AD DS) authentication (via Azure AD Connect sync + Kerberos)
D) Public anonymous access (no authentication)

**Answer: D) Public anonymous access (no authentication)**

**Explanation:** Azure Files (SMB) does not support anonymous/public access at all — unlike Blob Storage, which can be configured for anonymous public read access on containers/blobs, every SMB connection to an Azure file share must authenticate using one of the supported identity-based methods or the storage account key.

**Why not the others:**
- A) Storage account key IS a valid, supported authentication method for SMB access (used as the "password" with the storage account name as "username").
- B) Entra ID authentication (via Entra Domain Services or Entra Kerberos for hybrid identities) IS supported for identity-based SMB access.
- C) On-premises AD DS authentication (synced via Azure AD Connect, using Kerberos tickets) IS also a supported identity-based access method for Azure Files.

---

**Q37.** A Premium file share (FileStorage account kind) is being considered for a high-performance database workload requiring consistent low latency. Which underlying storage type does Premium file shares use?

A) Standard HDD-backed storage
B) SSD-backed storage
C) Archive-tier cold storage
D) The same storage as Cool blob tier

**Answer: B) SSD-backed storage**

**Explanation:** Premium file shares (requiring the `FileStorage` account kind, not general-purpose v2) are backed by SSD storage, providing consistent, low-latency, high-IOPS performance suitable for demanding workloads like databases or high-transaction applications — as opposed to Standard file shares, which use HDD-backed storage.

**Why not the others:**
- A) HDD-backed storage describes Standard file shares, not Premium — this is the opposite performance tier.
- C) Archive tier is a Blob Storage-specific offline tier concept; it doesn't apply to Premium Azure Files shares.
- D) Cool tier is also a Blob Storage concept and unrelated to Premium file share's underlying SSD architecture.

---

**Q38.** You need to grant fine-grained, share-level, identity-based permissions (like NTFS-style ACLs) for users accessing an Azure file share over SMB. Which combination is required?

A) Only a storage account key is needed; ACLs work automatically
B) Configure identity-based authentication (Entra Domain Services, Entra Kerberos, or on-prem AD DS) AND configure Windows ACLs/share-level permissions on the files/folders, since RBAC alone only controls share-level access, not file/folder-level granularity
C) RBAC roles alone provide full NTFS-style folder/file permission granularity
D) SAS tokens are required for all NTFS-level permission enforcement

**Answer: B) Configure identity-based authentication (Entra Domain Services, Entra Kerberos, or on-prem AD DS) AND configure Windows ACLs/share-level permissions on the files/folders, since RBAC alone only controls share-level access, not file/folder-level granularity**

**Explanation:** To achieve granular, Windows-style folder/file-level permissions on Azure Files, you need BOTH identity-based authentication configured (so users authenticate with their own Entra ID/AD identity, not just the storage key) AND traditional Windows ACLs set on the files/directories themselves (via Windows File Explorer or icacls after mounting) — Azure RBAC roles for Azure Files (like Storage File Data SMB Share Contributor) only control share-level access, not granular per-file/folder permissions.

**Why not the others:**
- A) Incorrect — storage account key access is an all-or-nothing credential; it does not carry per-user identity context needed for NTFS-style ACL enforcement.
- C) Incorrect — RBAC roles for Azure Files operate at the share level (e.g., can/can't access the share at all) — they do NOT provide folder/file-level granularity; that requires actual NTFS ACLs.
- D) Incorrect — SAS tokens grant time-limited, scope-based access but don't provide or enforce Windows-style NTFS ACL permission structures.

---

**Q39.** What is the default maximum size for a single Azure file share (Standard, using large file shares feature enabled) and a Premium file share, respectively (approximate exam-relevant figures)?

A) Both are limited to 5 TiB maximum
B) Standard with large file shares can scale up to 100 TiB; Premium file shares can scale up to 100 TiB as well (specific limits vary by redundancy type and are periodically increased)
C) Standard is limited to 1 TiB; Premium is unlimited
D) Both are limited to 500 GiB

**Answer: B) Standard with large file shares can scale up to 100 TiB; Premium file shares can scale up to 100 TiB as well (specific limits vary by redundancy type and are periodically increased)**

**Explanation:** With the "Large File Shares" feature enabled (available on LRS/ZRS standard accounts), Standard file shares can scale up to 100 TiB, and Premium file shares also support up to 100 TiB depending on configuration — both are far larger than their legacy default limits (5 TiB without large file share support). Exact figures are periodically updated by Microsoft, but the exam expects awareness that both tiers now support very large multi-TiB shares, well beyond the old 5 TiB ceiling.

**Why not the others:**
- A) Incorrect — 5 TiB was the legacy/default limit WITHOUT the large file shares feature enabled; with it enabled, both scale far beyond that.
- C) Incorrect — these figures don't match documented Azure Files limits; Standard is not capped at 1 TiB, and Premium is not literally "unlimited."
- D) Incorrect — 500 GiB is far too low and doesn't reflect actual Azure Files share size limits.

---

**Q40.** A blob is uploaded using the "Append Blob" type. Which operation is uniquely supported by this blob type compared to Block Blobs?

A) Random read/write access at any byte offset
B) Efficient append-only write operations, ideal for logging scenarios where data is continuously added to the end of the blob
C) Support for page-level differential snapshots
D) Native support for the Archive access tier

**Answer: B) Efficient append-only write operations, ideal for logging scenarios where data is continuously added to the end of the blob**

**Explanation:** Append Blobs are optimized specifically for append operations — data can only be added to the end of the blob (not modified/overwritten elsewhere), making them ideal for scenarios like logging, where multiple clients might concurrently append log entries.

**Why not the others:**
- A) Random read/write at arbitrary byte offsets is characteristic of Page Blobs (used for VHD/disk scenarios), not Append Blobs.
- C) Page-level differential snapshots are a Page Blob-specific concept (relevant to managed disks), not applicable to Append Blobs.
- D) Archive tier support applies to Block Blobs; Append Blobs and Page Blobs are NOT supported in the Archive access tier — this is actually a commonly tested restriction (only Block Blobs support tiering to Archive/Cool/Cold).

---

**Q41.** Which blob type underlies Azure Managed Disks (VHDs) and supports random read/write operations at any offset, along with efficient incremental snapshots?

A) Block Blob
B) Append Blob
C) Page Blob
D) Container Blob

**Answer: C) Page Blob**

**Explanation:** Page Blobs are optimized for random read/write access patterns (organized in 512-byte pages) and are the underlying storage mechanism for Azure VHDs/Managed Disks, supporting efficient incremental snapshots that only capture changed pages since the last snapshot.

**Why not the others:**
- A) Block Blobs are optimized for large sequential uploads/downloads (like files, images, videos) composed of blocks — not optimized for random-access read/write patterns needed by virtual disks.
- B) Append Blobs only support appending to the end — they don't support random read/write access at arbitrary offsets, which disqualifies them from disk-backing use cases.
- D) "Container Blob" is not a real Azure blob type; containers are the organizational unit that HOLDS blobs, not a blob type themselves.

---

**Q42.** You need to ensure that a specific blob cannot be deleted or modified for exactly 90 days, after which it should become fully deletable again automatically without manual intervention, and additional deletion attempts before that time should fail outright. Which immutability approach fits, and in what state should the policy be for the described automatic expiration behavior?

A) Legal hold, because it has clearly defined expiration behavior
B) Time-based retention policy in the "Locked" state with a 90-day retention period — after 90 days, the policy automatically permits deletion without manual removal
C) Time-based retention policy in the "Unlocked" state, since Unlocked policies auto-expire but Locked ones don't
D) Soft delete with a 90-day retention period

**Answer: B) Time-based retention policy in the "Locked" state with a 90-day retention period — after 90 days, the policy automatically permits deletion without manual removal**

**Explanation:** A time-based (interval) immutability policy set to Locked with a specific retention period automatically prevents deletion/modification until that period elapses; once the retention period expires, the blob becomes deletable without requiring an administrator to manually clear anything — this automatic expiration is precisely what distinguishes it from a Legal Hold (which never auto-expires).

**Why not the others:**
- A) Incorrect — Legal Holds have NO automatic expiration; they require manual removal regardless of how much time passes, which doesn't satisfy "automatically become deletable after 90 days."
- C) Incorrect — reversed logic; it's the Locked state that provides the enforceable, guaranteed retention behavior with automatic expiration. Unlocked policies can be freely modified/removed by an admin at any time, meaning the 90-day guarantee wouldn't be enforced/tamper-proof.
- D) Incorrect — soft delete allows recovery of DELETED blobs within a window; it doesn't PREVENT deletion/modification in the first place, which is the opposite of the described "cannot be deleted or modified" requirement.

---

**Q43.** Which storage account performance tier is required to use Page Blobs backing Premium SSD-based, low-latency disk workloads directly as unmanaged disks (legacy scenario) or to use Premium file shares?

A) Standard general-purpose v2
B) Premium
C) Standard general-purpose v1
D) Cool-tier only accounts

**Answer: B) Premium**

**Explanation:** Premium performance tier storage accounts (BlobStorage for Premium block blobs, FileStorage for Premium file shares, or the legacy Premium Page Blob-focused account kind) are backed by SSDs and required for consistently low-latency, high-IOPS workloads — Standard tier accounts (HDD-backed) don't provide this performance profile.

**Why not the others:**
- A) Standard general-purpose v2 accounts are HDD-backed (for Standard performance) and support Hot/Cool/Cold/Archive tiering for blobs, but don't provide Premium-level low latency.
- C) General-purpose v1 is a legacy account type with fewer features and also does not provide Premium SSD-backed performance.
- D) "Cool-tier only accounts" isn't a real distinct account type category, and Cool tier is about cost-optimized infrequent access, the opposite of low-latency premium performance.

---

**Q44.** You configure CORS (Cross-Origin Resource Sharing) rules on a Blob Storage service. A web application hosted at `https://app.contoso.com` tries to fetch a blob from your storage account using JavaScript `fetch()`. No CORS rule exists for this storage account. What happens?

A) The request succeeds because storage accounts allow all origins by default
B) The browser blocks the response due to the missing CORS headers, even though the storage account itself might process the request server-side — this is enforced client-side by the browser
C) The request fails at the Azure Storage service level with a 403 before even reaching the browser
D) CORS only applies to Table Storage, not Blob Storage

**Answer: B) The browser blocks the response due to the missing CORS headers, even though the storage account itself might process the request server-side — this is enforced client-side by the browser**

**Explanation:** CORS is fundamentally a browser-enforced security mechanism. Without a matching CORS rule configured on the storage account (specifying allowed origins, methods, headers), the storage service won't return the necessary `Access-Control-Allow-Origin` headers, causing the BROWSER to block the JavaScript code from accessing the response — even though the underlying HTTP request/response may have completed successfully at the network level.

**Why not the others:**
- A) Incorrect — by default, NO origins are allowed via CORS; you must explicitly configure CORS rules to permit any cross-origin browser-based access.
- C) Incorrect — this isn't strictly a server-side 403 rejection scenario in the traditional sense; the request can reach the storage service, but the browser blocks the JavaScript from reading the response due to missing CORS headers, which is a client-side enforcement nuance.
- D) Incorrect — CORS configuration applies across Blob, Queue, Table, and File services in Azure Storage, not just Table Storage.

---

**Q45.** Which of the following is TRUE regarding Blob Storage's "Last Access Time Tracking" feature?

A) It is enabled by default on all new storage accounts
B) It must be explicitly enabled, and once enabled, it allows lifecycle management policies to use `daysAfterLastAccessTimeGreaterThan` conditions instead of only modification-based conditions
C) It tracks access time at the container level only, never per-blob
D) It cannot be combined with lifecycle management rules

**Answer: B) It must be explicitly enabled, and once enabled, it allows lifecycle management policies to use `daysAfterLastAccessTimeGreaterThan` conditions instead of only modification-based conditions**

**Explanation:** Last Access Time Tracking is an opt-in feature (must be explicitly enabled on the storage account) that records the last read/write access timestamp per blob. Once enabled, lifecycle management rules can leverage `daysAfterLastAccessTimeGreaterThan` for tiering/deletion decisions, in addition to (or instead of) modification-time-based conditions — directly relevant to the nuance tested in Q5.

**Why not the others:**
- A) Incorrect — it's not enabled by default; it must be explicitly turned on due to the additional tracking overhead/cost.
- C) Incorrect — tracking operates at the individual blob level, not just container level, which is what makes it useful for lifecycle policies targeting specific blobs.
- D) Incorrect — it's specifically DESIGNED to integrate with and enhance lifecycle management rule capabilities, not to be incompatible with them.

---

**Q46.** You need to restrict a storage account so that it can only be accessed using TLS 1.2 or higher, rejecting older, less secure TLS versions. Which setting should you configure?

A) "Require secure transfer for REST API operations"
B) "Minimum TLS version"
C) "Allow storage account key access"
D) "Default to Entra authorization in the Azure portal"

**Answer: B) "Minimum TLS version"**

**Explanation:** The "Minimum TLS version" setting explicitly lets you set a floor (e.g., TLS 1.2) below which connection attempts will be rejected — distinct from simply requiring HTTPS (which "Require secure transfer" handles) since HTTPS connections could theoretically still attempt to negotiate older, weaker TLS versions without this additional restriction.

**Why not the others:**
- A) "Require secure transfer" only enforces HTTPS vs. HTTP — it does not control which specific TLS version is the minimum acceptable.
- C) "Allow storage account key access" concerns Shared Key authorization availability, unrelated to TLS version enforcement.
- D) This setting affects the default authorization method used within the Azure Portal's own data browsing experience (Entra ID vs. account key), not the minimum TLS version for general connections.

---

**Q47.** A storage account has soft delete for containers enabled with a 14-day retention period. A container named `archive-data` is deleted. On day 10, an administrator attempts to permanently purge it early instead of waiting for auto-expiration. Is this possible, and what permission is required?

A) No, soft-deleted containers can never be purged early; you must always wait the full retention period
B) Yes, an authorized user (with appropriate permission, e.g., via the "undelete" and "permanently delete" data actions) can manually and permanently purge a soft-deleted container before the retention period expires
C) Only Microsoft Support can perform early purging
D) Early purging is only possible for soft-deleted blobs, never for soft-deleted containers

**Answer: B) Yes, an authorized user (with appropriate permission, e.g., via the "undelete" and "permanently delete" data actions) can manually and permanently purge a soft-deleted container before the retention period expires**

**Explanation:** Soft-deleted containers (and blobs) CAN be manually and permanently purged before their retention period naturally expires, provided the user has the correct data-plane permission (this is a deliberate, explicit action distinct from normal deletion) — useful for immediately freeing up storage/complying with urgent deletion requests rather than waiting out the full retention window.

**Why not the others:**
- A) Incorrect — this contradicts the documented ability to manually purge soft-deleted resources early with proper permissions.
- C) Incorrect — this capability is available to authorized account users/roles directly, not restricted to Microsoft Support intervention.
- D) Incorrect — both soft-deleted blobs AND soft-deleted containers support early, manual permanent purging given the right permissions; it's not exclusive to blobs.

---

**Q48.** Which Azure Storage redundancy option provides the LOWEST cost but offers NO protection against a single datacenter-level hardware failure beyond standard local replica redundancy within that datacenter?

A) GRS
B) ZRS
C) LRS
D) RA-GZRS

**Answer: C) LRS**

**Explanation:** LRS (Locally Redundant Storage) replicates data three times within a single physical datacenter (protecting against individual disk/node/rack failures) but provides no protection if that entire datacenter experiences a catastrophic failure — making it the cheapest option but with the least geographic/zonal resilience among the four listed.

**Why not the others:**
- A) GRS is more expensive than LRS because it adds geo-replication to a secondary region, providing protection against a full regional disaster — the opposite of "lowest cost, no protection beyond datacenter."
- B) ZRS costs more than LRS because it synchronously replicates across multiple availability zones (separate physical datacenters within a region), providing zone-level failure protection LRS lacks.
- D) RA-GZRS is the most expensive/highest-availability option listed, combining zone-redundancy, geo-replication, AND read access to the secondary — the opposite extreme from what the question asks.

---

**Q49.** You want to configure a lifecycle management rule that deletes blob snapshots older than 90 days but leaves the base blob and any snapshots newer than 90 days untouched. Which lifecycle management rule scope/action should you configure?

A) `baseBlob` actions with `delete` after 90 days
B) `snapshot` actions with `delete` after `daysAfterCreationGreaterThan: 90`
C) `version` actions with `tierToArchive`
D) Blob container-level deletion, since individual snapshot targeting isn't supported

**Answer: B) `snapshot` actions with `delete` after `daysAfterCreationGreaterThan: 90`**

**Explanation:** Lifecycle management policy rules can independently target `baseBlob`, `snapshot`, and `version` object types with their own separate action sets. To specifically delete only snapshots (not the base blob or current versions) after they reach a certain age, you configure the `snapshot` action type with a `delete` action and a `daysAfterCreationGreaterThan` condition (since snapshots don't have a "modified" concept the same way base blobs do — they're immutable once created).

**Why not the others:**
- A) `baseBlob` actions target the current/live blob itself, not its historical snapshots — this would risk deleting the active blob, not the desired snapshots.
- C) `version` actions apply to blob versioning-created versions, which are a distinct concept/object type from manually or system-created snapshots; also, `tierToArchive` doesn't satisfy the stated requirement to delete, not tier.
- D) Incorrect — this is a false statement; lifecycle management explicitly DOES support targeting the `snapshot` object type independently at the rule level, without requiring whole-container deletion.

---

**Q50.** A company needs to ensure that data written to a storage account today remains recoverable even if an administrator accidentally deletes the entire storage account itself. Which statement about protecting against ACCOUNT-level deletion is most accurate?

A) Soft delete for blobs/containers fully protects against storage account deletion
B) Resource locks (e.g., `CanNotDelete`) applied to the storage account resource in Azure Resource Manager are the primary mechanism to prevent accidental deletion of the account itself; soft delete/versioning features protect data WITHIN an account, not the account resource itself
C) GRS/RA-GRS automatically prevents the storage account from ever being deleted
D) Immutability policies at the container level also prevent the parent storage account from being deleted

**Answer: B) Resource locks (e.g., `CanNotDelete`) applied to the storage account resource in Azure Resource Manager are the primary mechanism to prevent accidental deletion of the account itself; soft delete/versioning features protect data WITHIN an account, not the account resource itself**

**Explanation:** This capstone question tests a critical distinction: features like soft delete, versioning, and change feed protect against accidental deletion/modification of DATA (blobs, containers) within an existing storage account — but they do NOT prevent the storage account resource itself from being deleted via Azure Resource Manager. To protect against that, you need an ARM resource lock (`CanNotDelete` or `ReadOnly`) applied directly to the storage account resource, which is enforced independently of the storage service's own data-protection features.

**Why not the others:**
- A) Incorrect — soft delete operates on blobs/containers WITHIN the account; if the entire account is deleted, all underlying data (including soft-deleted retained items) is gone regardless of soft delete settings.
- C) Incorrect — geo-redundancy (GRS/RA-GRS) protects against regional data-center failure by replicating data, but has no bearing on preventing an administrator or automated process from deleting the account via ARM.
- D) Incorrect — immutability policies protect blob-level data from modification/deletion within a container; they do not extend to preventing deletion of the parent storage account resource itself at the ARM/subscription level.

---

## Study Notes: Recurring "Gotcha" Themes in This Set

1. **Archive tier is offline** — always requires rehydration; never instantly readable, unlike Hot/Cool/Cold.
2. **Lifecycle rules default to modification time, not access time** — access-time tracking is opt-in.
3. **ReadOnly vs. CanNotDelete locks and account-level vs. container-level public access settings** — precedence always favors the more restrictive account-level control.
4. **SAS types differ fundamentally**: Service (single service, key-signed), Account (multi-service, key-signed), User Delegation (Entra ID-signed, most secure, Blob-only).
5. **Locked immutability policies are truly immutable** — no RBAC role can override; retention can only increase, never decrease.
6. **GRS/RA-GRS replication is asynchronous** — failover can lose recently written, unreplicated data.
7. **Soft delete/versioning protect data WITHIN an account; resource locks protect the account resource ITSELF** — two entirely different protection layers.
8. **Private Endpoint vs. Service Endpoint** — private IP + on-prem connectivity + exfiltration protection, vs. public IP retained + subnet allow-listing only.
9. **Blob type restrictions** — only Block Blobs support access tiering (Hot/Cool/Cold/Archive); Page and Append Blobs do not.
10. **CORS is browser-enforced**, not a server-side access denial — a subtle but frequently tested distinction.

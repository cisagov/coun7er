# Implement Access Restrictions on Cloud Storage Objects 

* **ID:** CM0075
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Enable
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Cloud administration; object storage permissions and access controls

## Intended Outcome 

Implementing access restrictions on cloud storage objects restricts an adversary's ability to discover contents/information within cloud storage.

## Introduction 

Object storage services are prevalent in enterprise environments where the storage of data is dominated by scalable Cloud solutions. Examples of cloud storage services are Amazon S3, Google Cloud Storage, Microsoft Azure Blob, IBM Cloud Object Storage, and DigitalOcean Spaces.

Cloud storage auditing/configuration is a process that enables a user to remotely verify and help ensure the integrity of cloud storage services and the objects they contain. In some cases, outsourced data is present in cloud storage, which requires proper sanitization and authentication so as to eliminate the possibility of adversary access by embedded payloads or encoded commands.

The objective of this countermeasure is to eliminate an adversaries' ability to enumerate objects within cloud storage, and to enable prevention of object discovery in cloud storage by a threat actor. If an adversary is able to identify cloud storage services, they may conduct follow-on behaviors of exploitation/exfiltration of objects stored in cloud infrastructure.

Proper configuration or auditing of storage objects requires that permissions be designated to each user account or cloud service account under the Principle of Least Privilege.

Additionally, usage of APIs that list/enumerate objects in cloud storage should be restricted to accounts of higher privilege. 

## Preparation 

- Ensure that the cloud storage service(s) are updated and have logging and auditing features enabled. 
- Configure alerts and notifications constrained to anomalous data transfers, authentication events, and object access events.
- Take note of compromised/accessed storage objects, and be prepared to restore the affected storage objects to a previous backup.
- Compromised accounts or credentials must be evident before permissions and credentials are manipulated.

## Risks 

- This countermeasure can interfere with and/or break cloud storage objects and the contained data; poses the risk of misconfiguration which invites data breaches, unauthorized access, and loss of data.
- Rotating credentials for compromised cloud accounts tied to object storage services can disrupt business operations and induce downtime.
- Improper configuration of cloud storage services could lead to compatibility issues with objects connected to cloud storage.
- Adjusting API access creates administrator overhead and reduced functionality of the storage service.

## Guidance 

### Restrict API Access within Cloud Storage 

- Limit API access that permits listing of storage objects and/or buckets to privileged/administrator accounts. Examples of these sensitive APIs are ListObjectsV2 (AWS) and List Blobs (Azure).
- Implement IP whitelisting and blacklisting to constrain access of APIs that list objects or buckets to trusted IP addresses.
- Implement vendor-specific immutability features to objects that are essential to business operation and incident resolution.
- Keep API keys secret and regularly examine their usage to help ensure they are used by legitimate accounts. 

### Adjust Access Control Policies

- Setup Access Control Lists (ACLs) to specify who can access or perform actions on cloud storage.
- Restrict 'full control' permissions for cloud storage objects to the administrator account.
- Implement object-level and bucket-level policies that define access permissions at a granular level.

## Associated Techniques 

- [T1619](https://attack.mitre.org/techniques/T1619/) - Cloud Storage Object Discovery
- [T1580](https://attack.mitre.org/techniques/T1580/) - Cloud Infrastructure Discovery

## Related Countermeasures

- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0083 - Rotate Application Programming Interface (API) Keys
- CM0033 - Reset User Account Passwords
- CM0099 - Remove/Rotate Kubernetes Service Account Token
- CM0133 - Reset Access Control Lists to Default Values

## References 

- How do you audit and monitor cloud storage activities and events? | <https://www.linkedin.com/advice/0/how-do-you-audit-monitor-cloud-storage-activities>
- Enable Object Lock or a Legal Hold on an Existing Bucket | <https://www.backblaze.com/docs/cloud-storage-enable-object-lock-or-a-legal-hold-on-an-existing-bucket>
- Public cloud object storage auditing: Design, implementation, and analysis | <https://www.sciencedirect.com/science/article/pii/S0743731524000340>

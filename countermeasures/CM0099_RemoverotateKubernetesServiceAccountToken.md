# Remove/Rotate Kubernetes Service Account Token

* **ID:** CM0099
* **Version:** 1.0
* **Created:** 14 March 2025
* **Modified:** 14 March 2025
* **Type:** Disable, Refresh
* **Status:** Active
* **Nice Work Role:** Systems Administration (IO-WRL-005)
* **Skills & Knowledge:** Kubernetes administration; service accounts, tokens, and secrets

## Intended Outcome

Removing Kubernetes service account tokens, and/or rotating as needed, disrupts adversary credential access via Kubernetes service account token theft. 

## Introduction

Kubernetes service account tokens are credentials (effectively JSON Web Tokens) used by Kubernetes clusters to authenticate and authorize communication between various components within the cluster. They provide an identity for processes running within pods, allowing them to interact securely with the Kubernetes API server and other cluster resources. 

Kubernetes service account tokens include both *legacy tokens* and *bound tokens.* Legacy tokens are from Kubernetes version < 1.22 and are long-lived, and therefore stored indefinitely as secrets that are mounted into pods so that applications can authenticate API requests. Bound tokens are audience-bound, volume projected, and usually short-lived because they are time-bound. 

Service account tokens may be exposed to the adversary through any number of ways, including unencrypted transmission, public code repositories, misconfigured access rights, insider threats, and vulnerabilities in applications running in the cluster. 

Adversaries that steal Kubernetes service account tokens will be able to authenticate with the Kubernetes API, gaining access to the cluster and performing any number of actions including privilege escalation, data theft, persistent backdoor access to the cluster, or the deployment of malicious containers. 
  

## Preparation

- Identify Kubernetes service account token to be revoked.
- Install Kubernetes command line tool
- Configure local system to communicate with cluster by downloading the cluster's kubeconfig file and saving its file path to the `$KUBECONFIG` environment variable.

## Risks

- No Risks content identified.

## Guidance

- Secrets can be invalidated with the command `kubectl delete secret name-of-secret` if a Secret is known for a specific service account. 
- Tokens can be rotated by creating a new service account with a new token (`kubectl create serviceaccount`) and then update the pods or deployments to use the new service account. Old service accounts can be deleted with the command `kubectl delete serviceaccount <old-service-account-name>`
- After invalidating any service account tokens, secrets, or service accounts, restart all pods.
- Consider limiting the Service Account API credential auto-mounting with `automountServiceAccountToken: false`
- Where possible, use short-lived tokens over long-lived ones and use available commercial tools like Kyverno or Gatekeeper to enforce policies. 
- Explore using tools such as [External Secrets Operator](https://external-secrets.io/) to manage external secrets from third-party key vaults. 

## Associated Techniques

- [T1528](https://attack.mitre.org/techniques/T1528/) -  Steal Application Access Token 

## Related Countermeasures

- CM0028 - Reset Service Account Passwords
- CM0075 - Implement Access Restrictions on Cloud Storage Objects
- CM0076 - Remove Adversary Certificates and Rotate Secrets for Applications and Service Principals
- CM0077 - Revoke Microsoft 365 Refresh Tokens
- CM0080 - Block and Reissue Tokens
- CM0041 - Disable Service Account Interactive Login

## References

- Regenerate Kubernetes Service Account Tokens | <https://blog.gitguardian.com/understanding-the-risks-of-long-lived-kubernetes-service-account-tokens/>
- Managing Service Accounts | <https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/?ref=blog.gitguardian.com#bound-service-account-token-volume>
- Kubernetes Service Account Tokens -Security Improvements | <https://medium.com/@radharamadoss/kubernetes-service-account-tokens-security-improvements-a9734c186afb>
- Steal Pod Service Account Token | <https://stratus-red-team.cloud/attack-techniques/kubernetes/k8s.credential-access.steal-serviceaccount-token/>
- Guides - Rotate Kubernetes Secrets | <https://techdocs.akamai.com/cloud-computing/docs/rotate-kubernetes-secrets>
- External Secrets Operator | <https://external-secrets.io/>

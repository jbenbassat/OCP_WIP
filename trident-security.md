# NetApp Persistent Storage (Trident) - Security Recommendations

??? abstract "Document Control"

    !!! info "Page Owner: Jamal Boudi (Solutions Architect - NetApp)"

    !!! info "User Story"
        **As a**: Solutions Architect

        **I want to**: understand the security aspects associated with NetApp's persistent storage

        **So that**: I can properly incorporate NetApp's persistent storage and ONTAP technologies into OpenShift/Kubernetes based solutions with the proper security best practices

    !!! info "Status"
        - [ ] Structure
        - [X] Draft
        - [ ] Reviewed:
        - [ ] Ready
        - [ ] Published: _Add the public URL link here once published_

    !!! info "Classification"
        - [X] IBM Confidential
        - [ ] Public

The following Trident deployment considerations are as of v21.01.1 release.

## Run Trident in its own namespace:
It is important to prevent applications, application admins, users, and management applications from accessing Trident object definitions or the pods to ensure reliable storage and block potential malicious activity. To separate out the other applications and users from Trident, always install Trident in its own Kubernetes namespace. In our Installing Trident docs we call this namespace trident. Putting Trident in its own namespace assures that only the Kubernetes administrative personnel have access to the Trident pod and the artifacts (such as backend and CHAP secrets if applicable) stored in the namespaced CRD objects. Allow only administrators access to the Trident namespace and thus access to tridentctl application.


## CHAP authentication

Trident supports CHAP-based authentication for ONTAP backends and ONTAP SAN workloads (using the **ontap-san** and **ontap-san-economy** drivers). NetApp recommends using bidirectional CHAP with Trident for authentication between a host and the storage backend.


## CHAP with ONTAP SAN backends

For ONTAP backends that use the SAN storage drivers, Trident can set up bidirectional CHAP and manage CHAP usernames and secrets through **tridentctl**. Refer to Using [**CHAP with ONTAP SAN drivers**](https://netapp-trident.readthedocs.io/en/stable-v21.01/kubernetes/operations/tasks/backends/ontap/ontap-san/bidir-ontap-chap.html#using-chap-with-ontap-san-drivers) to understand how Trident configures CHAP on ONTAP backends.

**Note:** CHAP support for ONTAP backends is available with Trident 20.04 and above.


## Multi-tenancy

You may consider using multiple SVMs with Trident for many different reasons, including isolating applications and resource domains, strict control over resources, and to facilitate multi-tenancy. It’s also worth considering using at least two SVMs with any Kubernetes cluster to isolate persistent storage for cluster services from application storage. Using multiple SVMs with one dedicated to cluster services isolates and controls the workload. Since the persistent storage resources needed by the  Kubernetes cluster must exist prior to Trident deployment,  the Kubernetes cluster services SVM will not have dynamic provisioning in the same manner as the application SVM will have.


Link to [Trident Security Information](https://netapp-trident.readthedocs.io/en/stable-v21.01/dag/kubernetes/security_recommendations.html?highlight=security#)

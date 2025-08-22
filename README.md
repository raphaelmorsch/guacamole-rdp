# Guacamole on OpenShift

This repository provides the resources and configuration to deploy **Apache Guacamole** on **Red Hat OpenShift** as a containerized replacement for traditional jump hosts.  
The deployment is fully **rootless** (non-root containers) and adapted for environments where security and scalability are priorities.

---

## 🚀 Project Goal
Enable secure and scalable remote access to Windows or Linux environments through a browser, using **Guacamole** (clientless remote desktop gateway).  
This solution was designed to demonstrate the viability of Guacamole as an alternative to Citrix or traditional VMs in enterprise environments.

---

## 📂 Repository Structure
- **`Deployment/`** → Kubernetes/OpenShift YAML manifests for Guacamole and guacd.
- **`ConfigMap/`** → Example configuration for connections.
- **`Persistent Volumes`** → Example of `emptyDir` usage for FreeRDP certificate store.
- **`README.md`** → Project documentation (this file).

---

## 🛠️ Features
- Runs **without root privileges** (OpenShift SCC constraints respected).
- **Guacd + Guacamole webapp** deployment, using official containers as base.
- Support for **VNC, RDP (FreeRDP), and SSH** connections.
- Debugging instructions included for **protocol negotiation errors**.
- Workaround for FreeRDP certificate store:
  - Mounts an `emptyDir` volume to `/home/guacd/.config/freerdp`
  - Ensures writable, user-scoped configuration.

---

## ⚙️ How to Deploy

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-org>/guacamole-openshift.git
   cd guacamole-openshift
   ```

2. Apply manifests:
   ```bash
   oc apply -f manifests/
   ```

3. Expose the Guacamole route:
   ```bash
   oc get route guacamole -n guacamole
   ```

4. Access in the browser with the default credentials (or updated via ConfigMap).

---

## 🧪 Troubleshooting

- **FreeRDP certificate store errors**
  ```
  guacd: DEBUG: error creating directory '//.config/freerdp'
  guacd: DEBUG: certificate store initialization failed
  ```
  ✅ Fixed by creating a writable home directory for `guacd` and mounting `emptyDir` to `.config/freerdp`.

- **Protocol negotiation failure**
  Ensure that:
  - The target Windows/Linux VM is reachable from the OpenShift cluster.
  - Correct protocol and credentials are defined in the Guacamole configuration.

---

## 🔒 Security Considerations
- No privileged or root containers required.
- All storage is ephemeral by default (`emptyDir`), but can be adapted to persistent volumes.
- Network policies can be applied to restrict access between Guacamole and target systems.

---

## 📌 Next Steps
- Automate deployment with **Helm charts** or **Ansible Automation Platform**.
- Integrate with enterprise authentication (e.g., **Keycloak / RH-SSO**).
- Compare performance benchmarks between Guacamole, VNC, and Citrix.

---

## 👨‍💻 Authors
This repository was built and tested in collaboration with OpenShift solution architecture discussions, focusing on replacing Windows Jump Hosts with containerized access gateways. Lead by Raphael Morsch

---

## 📜 License
Apache 2.0

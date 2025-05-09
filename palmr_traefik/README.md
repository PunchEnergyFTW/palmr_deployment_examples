# Palmr TRAEFIK Deployment

This repository contains a minimal, production-ready example for deploying [Palmr](https://github.com/kyantech/Palmr) behind Traefik as a reverse proxy with HTTPS and automatic Let's Encrypt certificates.

---

## 🚦 **Important Requirements**

- **Both domain names used in this deployment must already exist and resolve to the correct public IP address of your server.**
    - SSL certificates and reverse proxying will not work unless DNS is properly configured.
    - You can check this with `dig your-domain.com` or online DNS tools.

---

## ⚙️ **Configuration**

- **You generally only need to modify the `.env` file** in this repository.
    - The `.env` file contains all the variables (domains, passwords, ports, etc.) required for your deployment.
    - The `docker-compose.yml` and other files should not need changes for most use cases.

---

## 📦 **Upload Size Limits**

- **Traefik is configured with a `client_max_body_size` of 1GB** by default.
    - If you need to change this limit (increase or decrease), edit the env variable:
      ```
        TRAEFIK_MAX_BODY_SIZE=size_in_bytes
      ```
    - After editing, restart the Traefik container for changes to take effect.

- **Note:**
    - If you increase the Palmr `MAX_FILESIZE` limit - **MAKE SURE** that you at least match the size in the variable `TRAEFIK_MAX_BODY_SIZE`

---

## 📌 **Version Pinning**

- In these Docker Compose files, *`palmr-app`* and *`palmr-api`* are pinned to specific image releases.
    - This ensures you are using the exact versions tested and verified with this deployment setup.
    - If you want to upgrade Palmr, update the image digests in `docker-compose.yml` and retest your deployment.

---

## 🚀 **Deployment Steps**

1. **Clone this repository and review the `.env` file.**
2. **Edit the `.env` file** to set your domain names, passwords, and other settings.
3. **Ensure your domains point to your server's IP.**
4. **Start the stack:**
---
## 🛠️ Troubleshooting: Missing SSL Certificates

Traefik can take some time on first start to generate certificates. Be patient and reload the website several times. It should come up normally when your *FQDNs (Domain Names)* are set up correctly.

If for some reason you need to reset all certificates, just remove the `letsencrypt` folder with
```
sudo rm -rf letsencrypt/
```
After that, restart the Traefik container and wait for it to regenerate the Certificates.

**Tip:**
Monitor the logs of the `traefik` container to ensure that certificates are issued successfully and that no errors occur. You may increase the logging verbosity [More Info](https://doc.traefik.io/traefik/observability/logs/#level for more information) for the container in the `docker-compose.yml` under:
```yml
services:
  traefik:
    image: "traefik"
    container_name: "traefik"
    command:
      - "--log.level=DEBUG"  -> Change this to DEBUG

```


---

## 📝 **Credits**

- [Palmr by Kyantech](https://github.com/kyantech/palmr)
- [Traefik Proxy](https://traefik.io/traefik/)
- Docker Compose example assembled and tested by [Charly ](https://github.com/PunchEnergyFTW)

---

## 🔗 **Further Reading**

- [Palmr Documentation](https://palmr.kyantech.com.br/)
- [Palmr Repository](https://github.com/kyantech/Palmr)
- [Traefik Proxy Documentation](https://doc.traefik.io/traefik/)

---

**If you encounter issues, please check your DNS, environment variables, and the Palmr project's documentation for updates.**

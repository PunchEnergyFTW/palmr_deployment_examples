# Palmr NGINX Deployment

This repository contains a minimal, production-ready example for deploying [Palmr](https://github.com/kyantech/Palmr) behind NGINX as a reverse proxy with HTTPS and automatic Let's Encrypt certificates.

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

- **NGINX is configured with a `client_max_body_size` of 1GB** by default.
    - If you need to change this limit (increase or decrease), edit the file:  
      ```
      nginx/conf.d/client_max_body_size.conf
      ```
    - After editing, restart the NGINX container for changes to take effect.

- **Note:**  
    - The current Palmr release has a **hard-coded 1GiB file upload limit**.  
    - Any file larger than 1GiB will fail to upload, regardless of NGINX settings.
    - **Watch for a future Palmr release** that introduces the `MAX_FILESIZE` environment variable for configurable upload limits.

---

## 📌 **Version Pinning**

- In these Docker Compose files, **`palmr-app` and `palmr-api` are pinned to specific image releases**.
    - This ensures you are using the exact versions tested and verified with this deployment setup.
    - If you want to upgrade Palmr, update the image digests in `docker-compose.yml` and retest your deployment.

---

## 🚀 **Deployment Steps**

1. **Clone this repository and review the `.env` file.**
2. **Edit the `.env` file** to set your domain names, passwords, and other settings.
3. **Ensure your domains point to your server's IP.**
4. **Start the stack:**
5. **(Optional) Adjust NGINX upload limit** in `nginx/conf.d/client_max_body_size.conf` if needed, then restart NGINX.
---
## 🛠️ Troubleshooting: Missing SSL Certificates

If your SSL certificates are missing or have not been generated-especially after several unsuccessful deployment attempts-it’s possible that Let’s Encrypt has scheduled the next renewal attempt far in the future. In this case, you can manually force a certificate renewal by running:
```
docker exec letsencrypt-companion /app/force_renew
```
This will immediately trigger the companion container to attempt to obtain or renew certificates for all configured domains, regardless of the current renewal schedule.

**Tip:**  
After running this command, monitor the logs of the `letsencrypt-companion` container to ensure that certificates are issued successfully and that no errors occur.


---

## 📝 **Credits**

- [Palmr by Kyantech](https://github.com/kyantech/palmr)
- [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)
- [Let's Encrypt Companion](https://github.com/nginx-proxy/acme-companion)
- Docker Compose example assembled and tested by [Charly ](https://github.com/PunchEnergyFTW)

---

## 🔗 **Further Reading**

- [Palmr Documentation](https://palmr.kyantech.com.br/)
- [Palmr Repository](https://github.com/kyantech/Palmr)
- [nginx-proxy Documentation](https://github.com/nginx-proxy/nginx-proxy)

---

**If you encounter issues, please check your DNS, environment variables, and the Palmr project's documentation for updates.**


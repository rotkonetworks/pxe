# PXE Boot Server

Minimal PXE boot service for Arch Linux using iPXE + Docker + Caddy, auto-deployed to `pxe.rotko.net`.

---

## 🤬 Motivation

> `This function requires SFT-DCMS-SINGLE license!`

Supermicro IPMI virtual media is locked behind a paid license. This repo is
great workaround for paying $180.

---

## 🔧 Usage (iPXE Shell)

```ipxe
dhcp
chain https://pxe.rotko.net/ipxe/archlinux.efi
```

Boots straight into Arch Linux Live over HTTPS.

---

## 🐳 Local Docker Setup

```bash
docker compose up -d --build
```

Serves content from `./ipxe` on `localhost:8089`.

---

## 🖥️ Remote Server Setup

```bash
sudo useradd -m pxe
sudo usermod -aG docker pxe
su pxe
ssh-keygen && cat ~/.ssh/.pub -> authorized_keys
```

Ensure Docker is installed and Caddy reverse-proxies `localhost:8089` on `pxe.rotko.net`.

---

## 🚀 GitHub Deployment

This repo auto-deploys to the `pxe` user on `pxe.rotko.net`.

### ✅ Setup:

1. Go to **GitHub → Settings → Secrets → Actions**
2. Add:

```
PXE_SSH_KEY = your private key (~/.ssh/id_ed25519)
```

Must match public key in `~pxe/.ssh/authorized_keys` on the server.

---

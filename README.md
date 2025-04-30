# PXE Boot Server

Serves Arch Linux netboot iPXE script over HTTPS via Docker + Caddy.


## gh setup
Go to Settings → Secrets → Actions

Add:

PXE_SSH_KEY: your private SSH key for pxe@pxe.rotko.net (use cat ~/.ssh/id_ed25519)

Must match the public key in ~pxe/.ssh/authorized_keys on the server


## srv setup
sudo usermod -aG docker pxe

&& setup ur webproxy

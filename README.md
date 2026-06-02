# pxe

PXE boot service for Arch Linux. iPXE binary served over HTTPS via nginx in
Docker, fronted by Caddy at `pxe.rotko.net`. Optional dnsmasq container provides
DHCP + TFTP on the LAN.

Avoids the SFT-DCMS-SINGLE license required for Supermicro IPMI virtual media.

## Boot a host

Force PXE on next boot and power-cycle via IPMI:

    ipmitool -I lanplus -H <bmc> -U <user> -P <pass> chassis bootdev pxe
    ipmitool -I lanplus -H <bmc> -U <user> -P <pass> power cycle

In the iPXE shell:

    dhcp
    chain https://pxe.rotko.net/ipxe/archlinux.efi

## Run

    docker compose up -d --build

`http`: nginx serving `./ipxe` on `:8089`.
`tftp-dhcp`: dnsmasq on host network for DHCP + TFTP. Edit `dnsmasq.conf` for
your LAN before starting.

## Server

    useradd -m pxe
    usermod -aG docker pxe

Append the deploy public key to `~pxe/.ssh/authorized_keys`. Caddy proxies
`localhost:8089` to `pxe.rotko.net`.

## Deploy

Push to master deploys to the `pxe` user over SSH. Set the `PXE_SSH_KEY` secret
(private key) under Settings → Secrets → Actions; its public half must be in
`~pxe/.ssh/authorized_keys`.

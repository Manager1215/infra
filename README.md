# infra

Ansible for my homelab and VPS. Also deploys my CV (nikitapavelko.dev) and the Kiln site (horizon-kiln.cc).

## Hosts

- homelab: old laptop with Debian. Grafana + Prometheus, Finanager, File Browser, all behind a Cloudflare Tunnel, no open ports. Offline right now, the laptop is in Warsaw and I'm in Dnipro.
- vps1: Ubuntu, 1 vCPU / 2 GB. nginx + Let's Encrypt, the CV, the Kiln site and its accounts service.

## Roles

base, hardening, docker, monitoring, finanager, files, cloudflared, static_sites, kiln_site

## Run

```sh
git submodule update --init
ansible-galaxy collection install -r requirements.yml

ansible-playbook site.yml --limit vps --check --diff
ansible-playbook site.yml --limit vps
ansible-playbook site.yml --limit homelab --ask-vault-pass
```

## Notes

- homelab secrets: Ansible Vault, `vault.yml` is not in git, keys are in `vault.yml.example`. Kiln accounts secrets only live on the server.
- new static site = one more item in `static_sites` (group_vars/vps). `nginx -t` before every reload.
- unknown hostnames: 444 on http, dropped handshake on https.
- Kiln CI deploys as `kiln-deploy`, its key is locked to rrsync into /var/www/kiln.
- homelab deploys are manual, GitHub runners are IPv4 only and the homelab is IPv6 only.

# Headscale Server

Headscale plus Headplane (UI) deployed via docker compose. 

```bash
cp .env_sample .env
vim .env
vim config/config.yaml
docker compose up
```

To work like a classic VPN, as in "clients connect to this server & they can reach Internet from it", also install tailscale client itself locally 

```bash
curl -fsSL https://tailscale.com/install.sh | sh

echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf

tailscale up --reset --advertise-exit-node=true --login-server=https://vpn.company.cloud
```

To allow reaching the local network, or even any private network this server can reach, advertise it

```bash
tailscale up --reset --advertise-routes=10.3.0.0/16 --login-server=https://vpn.company.cloud

docker compose exec headscale headscale routes list
docker compose exec headscale headscale routes enable -r 3
```
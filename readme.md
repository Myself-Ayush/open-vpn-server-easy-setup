# Easiest way to setup open vpn server - 

Prefer Debian based os

```bash
sudo snap install docker
```

```bash
docker pull openvpn/openvpn-as
```
## open port from vm and router 

- 9443 → Admin UI (container 943)
- 4443 → Client UI (container 443)
- 1195/udp → VPN data channel    - u can configure udp / tcp depend on use case

```bash
docker run -d \
  --name=openvpn-as-local \
  --device /dev/net/tun \
  --cap-add=MKNOD \
  --cap-add=NET_ADMIN \
  -p 9443:943 \
  -p 4443:443 \
  -p 1195:1195/udp \
  -v ${HOME}/openvpn-data:/openvpn \
  --restart=unless-stopped \
  openvpn/openvpn-as
```

## get password  - username - openvpn
```bash
docker logs openvpn-as-local | grep "Auto-generated pass"
```
### login - https://public-ip:9443/admin/
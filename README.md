# sing-box setup for tunneling traffic on specified domain through WireGuard on PC

## Setup

**sing-box version: 1.12.16**
**Tested on Arch Linux only**

1. Install sing-box  
   Follow the official installation steps for your system:  
   [https://sing-box.sagernet.org/installation/package-manager/](https://sing-box.sagernet.org/installation/package-manager/)

2. Download geosite rule set Database  
   Rule set (previously geosite) are pre-bundled domains provided by sing-box.  
   On Arch Linux, you can optionally install and use the `ruleset.srs` locally:

```bash
paru -S sing-geosite-rule-set
```

> On other system, you can find the list of rule sets on their GitHub, downloading is optional:  
> [https://github.com/SagerNet/sing-geosite/tree/rule-set](https://github.com/SagerNet/sing-geosite/tree/rule-set)

## Configuration

### Setup config file

1. Download or Copy the `config.json.example`

```bash
git clone https://github.com/dolkilu/sing-box-wireguard-example.git
```

2. Move the config into desired location

```bash
cd sing-box-wireguard-example
mkdir -p ~/.config/sing-box
mv config.json.example ~/.config/sing-box/config.json
touch ~/.config/sing-box/box.log
```

3. Complete the config with reference to inline comments and official documentation:
   [https://sing-box.sagernet.org/configuration/](https://sing-box.sagernet.org/configuration/)
   > While LLMs can be helpful, be aware they can often generated deprecated configurations.

### Gathering info for WireGuard

As sing-box has built-in WireGuard, an external WireGuard setup is not needed. You will need find:

- VPN address, port
- VPN DNS server
- WireGuard Private Key
- WireGuard Public Key

VPN providers like Mullvad and Proton provide WireGuard configuration downloads on their website.

For NordVPN, you can follow these urls:  
[Instructions to obtain WireGuard details of your NordVPN account.](https://gist.github.com/bluewalk/7b3db071c488c82c604baf76a42eaad3)  
[NordVPN WireGuard Config Generator](https://nord-configs.selfhoster.nl/)

> When generating the config, remember to use your private key to generate your own public key.

## Usage

- Verify config fields (Does not check for logic errors)

```bash
sing-box check
```

- Run with config

```bash
sing-box run -c ~/.config/sing-box/config.json
```

- Setting up systemd service

```bash
sudo systemctl enable sing-box.service
```

- systemd service uses `/etc/sing-box/config.json` by default, either edit the config or setup a systemd `override.conf`

```bash
sudo systemctl edit sing-box.service
```

- Add:

```service
[Service]
ExecStart=
ExecStart=/usr/bin/sing-box -D /var/lib/sing-box -c /home/username/.config/sing-box/config.json run
```


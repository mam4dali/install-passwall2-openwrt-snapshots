# How to Install PassWall2 on OpenWrt Snapshot Builds
[فارسی](https://github.com/mam4dali/install-passwall2-openwrt-snapshots/blob/main/README.fa.md)

⚠️ This configuration has been tested on a [Google WiFi Router](https://support.google.com/googlenest/answer/7168315?hl=en) running OpenWrt version OpenWrt 23.05.2 r23630-842932a63d stable. Ensure that your router has **at least 128MB of storage** before executing these commands. ⚠️

## 1. Initial Settings

### Configuring IP Address, WiFi, and Password

Read more about [OpenWrt initial settings](https://github.com/Ramtiiin/Install-PassWall-OpenWrt/blob/main/Openwrt-initial-setting.md).

## 2. Install Passwall

### SSH into the Router

1. Open your terminal (or SSH client) and connect to the router:
   ```sh
   ssh root@192.168.1.1

2. Update the package list:
   ```sh
   apk update

3. Remove the default dnsmasq and install dnsmasq-full:
   ```sh
   apk del dnsmasq
   apk add dnsmasq-full

4. Install required kernel modules:
   ```sh
   apk add kmod-nft-tproxy kmod-nft-socket
   
5. Download and add the Passwall public key:
   ```sh
   wget -O /etc/apk/keys/passwall.pub https://master.dl.sourceforge.net/project/openwrt-passwall-build/passwall.pub

6. Set up custom feeds for Passwall:
```sh
   read arch << EOF
$(. /etc/openwrt_release ; echo $DISTRIB_ARCH)
EOF

mkdir -p /etc/apk/repositories.d

{
  for feed in passwall_packages passwall2; do
    echo "https://master.dl.sourceforge.net/project/openwrt-passwall-build/snapshots/packages/$arch/$feed/packages.adb"
  done
} > /etc/apk/repositories.d/passwall.list
```

7. Update the package list again to include Passwall feeds:
   ```sh
   apk update --allow-untrusted

8. Install luci-app-passwall2 and v2ray-geosite-ir:
   ```sh
   apk add luci-app-passwall2 v2ray-geosite-ir --allow-untrusted
   

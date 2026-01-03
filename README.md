# openwrt-tailscale-https
 HTTPS Luci with Tailscale on OpenWRT 
 
## Rationale
Like in [truenas-tailscale-https](https://github.com/kc3wny/truenas-tailscale-https), I want to securely access my OpenWRT Luci interface over HTTPS with a valid certificate.

## Guide

```
wget -O /usr/bin/renew-ts-cert https://raw.githubusercontent.com/kc3wny/openwrt-tailscale-https/refs/heads/main/renew-ts-cert && chmod +x /usr/bin/renew-ts-cert
```

Edit this script to include your `machine-name.tailnet-name.ts.net` address so the correct certificate can be provisoned

Test the script using the command below before moving on:

```
/usr/bin/renew-ts-cert
```

Check the output, if you see:

```
Wrote public cert to /etc/uhttpd.crt
Wrote private key to /etc/uhttpd.key
```
then it has been sucessfully provisioned.


To automate this task, we will use a cron job.
```
crontab -e
```
I am using  `0 0 1 * * /usr/bin/renew-ts-cert` (note: this can be at any interval, but given Tailscale certs are only valid for 90 days, a monthly refresh is advisable)
Once added, restart cron:
```
service cron restart
```


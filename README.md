# TODO
Do as much green for the audit below at least

Support other linux distro
- Debian / Ubuntu 
- Rocky Linux
- Open Suse
- AlmaLinux

Integrate molecule on the CI?

# Audit
Also need to integrate this into the test or install automatically somehow, idk. Can add more if want
https://cisofy.com/lynis/controls/LYNIS/
```
sudo apt install lynis
sudo lynis audit system
```

https://vpsaudit.vernu.dev/
```
curl -O https://raw.githubusercontent.com/vernu/vps-audit/main/vps-audit.sh && chmod +x vps-audit.sh && sudo ./vps-audit.sh
```
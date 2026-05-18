
##  linux Configuration 
### So we need add ip addres in the same range at mikrotik ip ether 2 so we can connet and ping
```bash
sudo nano /etc/network/interface
```
<img width="416" height="53" alt="Screenshot 2026-05-14 234548 - Copy - Copy" src="https://github.com/user-attachments/assets/7a9224fa-d2f1-43d0-93c3-14b92952590e" />

```bash

#Internet (Adapter 1)

auto ethe
iface eth0 inet dhcp

#Adapter 2 MikroTik & Ubuntu
auto ethi
iface eth1 inet static
  address 10.216.27.10
  netmask 255.255.255.0
  gateway 10.216.27.1
```
<img width="1120" height="618" alt="Screenshot 2026-05-15 000416" src="https://github.com/user-attachments/assets/9e891040-50f8-4584-a3df-ed1c29e52a9d" />

And restart our network so the input we add can be each in linux
<img width="599" height="59" alt="Screenshot 2026-05-15 181153" src="https://github.com/user-attachments/assets/282495bf-221f-47e5-bfc6-57a6bc09ce4e" />

We can see our new ip linux with 
```bash
ip a
```

<img width="1267" height="383" alt="Screenshot 2026-05-14 234602" src="https://github.com/user-attachments/assets/282b9cee-e365-499b-b642-3cff74e3e448" />



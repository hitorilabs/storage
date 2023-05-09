# storage

I'm quite fond of my macbook air - I can't see myself
interfacing with anything other than a laptop.

For this reason, it's going to be a bit complicated to
think about how I'm going to deal with storage.

This is not a budget build guide, we haven't been grinding
normie work just to park the money in a bank.

# Things to Consider

It's a little embarrassing that I code for a living, but
I'm not familiar with how to actually evaluate my options.

- Portable External HDD
- Home NAS

I'm going to try both eventually, but first I'll be trying
out the Portable External HDD from Amazon.

# Portable External HDD

I bought a [WD My Passport Ultra](https://www.westerndigital.com/en-ca/products/portable-drives/wd-my-passport-ultra-usb-c-hdd#WDBC3C0010BSL-WESN).

This thing was cool at first, but it has a lot of flaws:
- passively draining all my battery away (don't know why it would even do this)
- it's supposed to have max bandwidth 5Gbps (625MB/s), but it was actually more like 2Gbps
- data can't be accessed without the physical drive being plugged in

Most importantly, I want to have some fun building a NAS.
# Network Attached Storage (NAS)

- 36TB of HDD storage
- 1TB NVMe cache 
- configured with `raidz1` 
- Running TrueNAS Scale

## Specifications
Type | Name | Quantity | Price
-- | -- | -- | --
HDD | Seagate IronWolf 12TB NAS HDD (ST12000VN0008) | 3 | 259.99
CPU | Intel Core i5-11600K 6 Cores up to 4.9 GHz | 1 | 252.15
RAM | Corsair Vengeance LPX 16 GB (2 x 8 GB) DDR4-3200 | 1 | 59.99
PSU | Corsair RM750x 750W PSU | 1 | 149.99
MOBO | MSI MPG Z590 GAMING PLUS | 1 | 204.99
SSD | Samsung 970 EVO Plus 250GB NVMe M.2 (MZ-V7S250/AM) | 2 | 59.97
CASE | Antec VSK4000E-U3_US | 1 | 94.55
COOL | Noctua NH-D15 | 1 | 139.94

**Total**: 1801.52 CAD

---

Most of the cost is coming from the HDDs. To optimize this build for cost:
1. downgrade `MOBO`, `COOL`, `PSU`
2. source most parts locally

Some high-level considerations that were made:
- 3x `HDD` are enough to run RAIDz1 (ideally, I should have at least 4 to run RAIDz2)
- `COOL` NH-D15 is a universally solid cooler, I move it into a more demanding build when appropriate
- `CASE` has space for 5 drives (terrible case and somehow still ~$100)
- `MOBO` supports 2.5GbE LAN (hard to get 10GbE in Canada, can add a network card to support this)
- `RAM` not a big deal at the moment

### Bandwidth over Wi-Fi
Even though I did consider getting high-bandwidth
ethernet, I didn't account for 802.11ax 1.2Gb/s (~150
MB/s) per stream. I thought 2.5 GbE (312.5 MB/s) would be
plenty, but over wifi I get a quarter of this bandwidth. 

I'm most likely going to park a DL workstation next to the
NAS connected via ethernet network switch.

## References for Setup + Inspiration
- Sentdex Home Lab - https://www.youtube.com/watch?v=CIQ20FWs478
- Setting up TrueNAS Core - https://www.youtube.com/watch?v=nVRWpV2xyds&t=0s

# Deep Learning Workstation

- RTX 3090
- 1x 1TB NVMe (980 Pro) + 2TB NVMe drive (970 Evo Plus)
- Intel i7-13700KF (16-core)
- arch btw

## Specifications
Type | Name | Quantity | Price
-- | -- | -- | --
CPU | Intel Core i7-13700KF | 1 | 519.98
COOL | Noctua NH-D15S | 1 | 99.95
MOBO | Asus ROG STRIX Z690-E | 1 | 448.98
RAM | Corsair Vengeance 64 GB (2 x 32 GB) DDR5-6000 | 1 | 319.99
SSD | Samsung 980 Pro 1 TB M.2-2280 NVMe | 1 | 129.97
SSD | Samsung 970 Evo Plus 2 TB M.2-2280 NVMe | 1 | 169.99
GPU | NVIDIA RTX 3090 FE | 1 | 900.00
CASE | Corsair 7000D AIRFLOW | 1 | 299.99
PSU | Corsair RM850x 850W PSU | 1 | 184.99

**Total**: 3073.84 CAD

---

This is clearly not a cheap build, some high-level considerations that were made:
- `CASE` + `MOBO` has enough space + ports for 6 drives and lots of space for fans.
- `MOBO` bundled with M.2 expansion card for 4 extra slots, 2.5GbE LAN, wi-fi
- `SSD` now that I know the `MOBO` came with an M.2 expansion card, I'm most likely going to invest in lots more of these NVMe drives (and give the OS it's own dedicated drives)
- `RAM` leaving room to expand to 128 GB

## Setup
Trying to save some cash and running _arch btw_ is not easy. 

1. **Software Compatibility**: The Asus ROG Strix Z690-E
motherboard was designed to be compatible with 12th Gen
Intel processors, the BIOS needed an update to support
13th Gen
2. **Hardware Compatibility**: Originally, I wanted to put
my NH-D15 in this, but it was actually incompatible due to
the heatsinks. To avoid headaches, the NH-D15S is the
variant that was designed to be more compatible with most
builds.
3. **Used Components**: Buying a used 3090 can be risky
business, but I ended up with a pretty solid deal
4. **Static IP**: I primarily wanted to SSH into the
machine, so I had to assign a static IP address so that I
don't have to check on the IP on every reboot.
5. **Arch Linux**: `archinstall` is great, but I have zero
concept of how to use the display managers or window
managers. Nothing like minimal install and `tmux` + `vim`

## Resources
### Setting up ssh ([wiki](https://wiki.archlinux.org/title/OpenSSH))
- check relevant service
  - `systemctl status sshd`
- check state of ip addresses
  - `ip address show`
- check restart service
  - `systemctl restart sshd`
### Configuring static IP using `systemd-networkd` ([wiki](https://wiki.archlinux.org/title/Systemd-networkd))
- check state of ip addresses
  - `ip address show`
  - `ip route show`
- check relevant services
  - `systemctl status sshd`
  - `systemctl status systemd-networkd`
- edit configuration (assuming wired ethernet)
  - `sudo vim /etc/systemd/network/20-ethernet.network`
- restart service
  - `systemctl restart systemd-networkd`
# TODO

- increase utilization on these machines
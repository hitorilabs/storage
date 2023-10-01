# navi at home

The name of the game is FLOPs, VRAM, mass storage, and
high-bandwidth networking.

_can we have navi?_

_Mom: no, we have navi at home_

![serialexperimentslain1](https://github.com/hitorilabs/navi/assets/131238467/fbb66c0e-b6b3-4eb8-b9c4-365544ef1b72)

# User Guide
Github actually has a decent reading experience with markdown, the "happy path" for reading this like a blog is to click the `README.md` file to enter "code preview" and then hit the side menu button to pop out the table of contents.

<img width="1444" alt="screenshot of markdown reader" src="https://github.com/hitorilabs/navi/assets/131238467/907c5cd6-ca61-40b7-ae5d-c346e6b4d943">

# Build Log

*These are not budget build guides, we haven't been grinding normie work just to park the money in a bank.*

#### 2023/08/30

recently secured a small pile of 3090s through the local social media marketplace and it's actually surprisingly hard to compose a system that's actually capable of running them all on a single machine.

<img width="1444" alt="rig" src="/images/rig.jpeg">

If only I had trusted the [lambdalabs article](https://lambdalabs.com/blog/deep-learning-hardware-deep-dive-rtx-30xx) to begin with, then I wouldn't have gone through countless hours of trying to get 4 3090s running on a single 1600W PSU. Basically, the sweet spot is 3 3090s running on a machine that's outfitted with server parts, since desktop parts will severely limit your total # of PCIE lanes (although you will rarely be memory constrained on multiple PCIE 4.0 x8 slots).

If you are only planning to run 2x GPU configurations as a workstation, you should consider settling for desktop parts and running them on PCIE 4.0 x8 (i.e. i9-13900K/i7-13700K both support 2x8 + 4 PCIE configurations, which leaves room for some weak networking or storage expansion). This way, you won't be dealing with used parts or old software.

Type | Name | Quantity | Unit Cost
-- | -- | -- | --
PSU | EVGA 1600W P+ | 1 | 551.42
GPU | ASUS ROG STRIX RTX 3090 | 3 | 850.00
MOBO | ROMED8-2T/BCM | 1 | 882.52
RAM | Micron 16GB DDR4-3200 RDIMM 1Rx4 (MTA18ASF2G72PZ-3G2R) | 4 | 60.915
STORAGE | WD_BLACK 1TB SN850X NVMe | 1 | 89.00
COOLER | Noctua NH-U9 TR4-SP3 | 1 | 101.64
CASE | Mining Rig Frame for 12GPU, Steel Open Air Miner | 1 | 45.19
RISERS | Thermaltake TT PCI-E 4.0 Riser Cable | 3 | 112.85
CPU | AMD EPYC 7302P 16 cores 3.0GHz 155W | 1 | 195.91

**Total**: 4,997.89 CAD

Review on pricing:
- RAM - $60 for a single 16GB stick is robbery, but I couldn't find any 1Rx4 memory on ebay
- RISERS - with one-day delivery on Amazon
- MOBO - got lots of recommendations for this and saw a lot of vast.ai builds that use it
  - bought from ebay at ~$300 off from the Amazon price
  - main advantage is that you get 7 slots with full PCIE 4.0 x16 bandwidth
- STORAGE - pretty smol, but won't be needing a ton on this machine yet because I'll be offloading to other machines
- GPU - a steal considering that the sellers were a very well-off family w/ kids who were probably not thrashing these cards
- CPU - a piece of crap, but I've never dealt with server parts before so I'm starting small. 
  - The ebay guys are surprisingly reliable, I wonder where they get these parts from 🤔
  - I also bought a torque driver that has the 14 lbs/in setting because I saw some comments about it on discord, but it might just be a psyop.

A neat article and chart on power limiting from a [Puget Systems article](https://www.pugetsystems.com/labs/hpc/NVIDIA-GPU-Power-Limit-vs-Performance-2296). This will help you control temperatures and stay comfortably under your power supply's maximum capacity.

<img width="1444" alt="power limiting chart" src="/images/power_limit.jpeg">

#### 2023/05/19

ubuntu-server pilled now. 

Accidentally wiped my root partition and realized what I need is stability. I want my server to be up all the time and feel roughly the same as a vm on `lambdalabs`. I was also trying to put tailscale on my arch linux install, but it was missing some mystery dependency that I'll never figure out.

ubuntu-server had a pretty straightforward install and sensible defaults (launch sshd without login, netplan is pretty straight forward, etc.)

- with tailscale + ubuntu, I can now connect to my DL workstation anywhere and safely reboot it without losing ssh access (unless something goes terribly wrong)
- UX workflow is now pretty much uniform with `lambdalabs` instances

#### 2023/05/11

- workstation w/ cuda
- network attached storage + rsync
- vs code w/ `copilot` + `remote - ssh plugin` + `jupyter notebooks` extension
- `htop` + `nvtop` monitoring on my TV screen
- arch btw

<img width="900" alt="screencap gif of sensor readings + nvtop + htop" src="https://github.com/hitorilabs/navi/assets/131238467/984d8b8c-5b45-4f77-a034-ee0075cfbdd4">

#### 2023/05/08

Built and setup the the workstation over the weekend. For the record, I've never owned a GPU before and my only interaction with linux distros is through docker images on the cloud.
  - can I still say arch btw if I used `archinstall`...
  - moved all the overkill components from NAS to DL workstation
  - never touched networking before, so this was an
  enlightening experience

#### 2023/05/01

I should just buy the machine already, I'm not completely broke. Looking through local marketplaces for honest people selling used 3090. Doing my research on all things hardware.

#### 2023/04/20

I have more than enough storage to dump everything I see in the foreseeable future, but no compute to make any of this productive. There are lots of gains to be made with solid networking and storage, but cloud computing platforms often don't offer much control there. I want to do a mix of inference and training, but none of the options make sense for me.
  - Lambda Labs never has any capacity available when I need it
    - For an A100, it costs `1.10 * 24 * 365 = 9636` to run an  inference server 24/7
    - For an A10 it still costs `0.6 * 24 * 365 = 5256`, but it's not even as good as an RTX 3090
  - Google colab is [doing some whack stuff](https://twitter.com/thechrisperry/status/1649189902079381505)
    - For a V100 expect `5.45 * 0.14 = 0.76` per hour, which is laughable compared to what you get with A10s.
    - For an A100 expect `15 * 0.14 = 2.1` per hour, which is double Lambda Labs pricing. 

#### 2023/04/14

Finished building and setting up TrueNAS Scale over the weekend. Honestly, not super impressed with it. 
  - RAID storage is a psyop
  - Could've just invested in a bunch of NVMe drives and deal with extra storage when the time actually comes.

#### 2023/04/10

Waiting 2 hours for a dataset to finish downloading over wifi, 5 mins for every model to download over the internet... this is the last time I will suffer. 
  - Ordered a bunch of parts and HDDs from Amazon to build a NAS (one-day shipping and scheduled pickup for returns feel illegal)

## Deep Learning Workstation

- RTX 3090
- 1x 1TB NVMe (980 Pro) + 2TB NVMe drive (970 Evo Plus)
- Intel i7-13700KF (16-core)
- arch btw

### Specifications
Type | Name | Quantity | Unit Cost
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

### Setup
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

### Configure bluetooth keyboard ([wiki](https://wiki.archlinux.org/title/bluetooth))

- Install `bluez` + `bluez-utils` packages
- check service
  - `systemctl status bluetoothctl`
  - `systemctl start bluetoothctl`
- scan for device
  - `bluetoothctl scan on` 
- connect device
  - `bluetoothctl connect <MAC_ADDRESS>`
  - `bluetoothctl trust <MAC_ADDRESS>` (so that the passcode will show up when pairing)
  - `bluetoothctl pair <MAC_ADDRESS>` (so that the passcode will show up when pairing)

pro tip: 
- connect via usb and turn scan on before entering pairing
- exit pairing and copy device `MAC_ADDRESS`
- trust device and start pairing on machine before
entering pairing mode on keyboard
- when rebooting the computer, you might get stuck on "no keyboard detected"
  - if your bluetooth keyboard has multiple pairing options (HHKB btw) you can cycle the pairing on/off to make it detect the keyboard.

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

```
[Network]
DHCP=no
Address=<ip_address_range>
Gateway=<your_gateway>
DNS=<some_dns_address>
IPv6PrivacyExtensions=yes
```
- restart service
  - `systemctl restart systemd-networkd`

### Mount Network Attached Storage (NFS)

- `/etc/fstab` (filesystem table)
```
# <file system> <dir> <type> <options> <dump> <pass>

<static_ip_address>:<path_to_dataset> <path_to_mount_location>  nfs  _netdev,noauto,x-systemd.automount,x-systemd.mount-timeout=10,timeo=14,x-systemd.idle-timeout=1min 0 0
```

# TODO

- increase utilization on these machines

## Suricata instructions

### Building Suricata

You need a running build environment for OpenWRT.

After running ```./scripts/feeds update``` and ```./scripts/feeds install -a``` copy the directory 'feeds' into the 'openwrt' directory.
Run ```./scripts/install feeds install libhtp``` and ```./scripts/install feeds install suricata```.

Run ```make menuconfig``` and select 'Network' > 'suricata'.

Run ```make package/suricata/compile``` and ```make```.

### Setup virtual machine with VMware

Create the virtual machine for OpenWRT image:

- create a new virtual machine
- remove all devices (hard disk, cd drive, sound, printer, camera)
- change primary network to host-only/Private (usually 'vmnet1')
- add secondary network as NAT/Shared (usually 'vmnet8')
- add 'openwrt.vmdk' as hard disk, select shared mode, not cloning
- power on the virtual machine

On the host, enable the default OpenWRT subnet for the host-only network:

```sudo ifconfig vmnet1 alias 192.168.1.2 255.255.255.0```

Test if the OpenWRT virtual machine is available:

```ping 192.168.1.1```

## Setup of Suricata

The package installs the configuration files in '/etc/suricata'.

It is necessary, to change the configuration in 'suricata.yaml' before running suricata.

First, the HOME_NET need to be changed in 'suricata.yaml':
The default for HOME_NET is "[192.168.0.0/16,10.0.0.0/8,172.16.0.0/12]", we need to set it to the masqueraded OpenWRT subnet, which is usually "[192.168.1.0/24]".

EXTERNAL_NET are all addresses that are not part of the HOME_NET.

'suricata.yaml' should look like:

```
vars:
  # more specific is better for alert accuracy and performance
  address-groups:
    HOME_NET: "[192.168.1.0/24]"
    ...

    EXTERNAL_NET: "!$HOME_NET"
    #EXTERNAL_NET: "any"
    ...
```

Suricata requires a log directory, usually the path is '/var/log/suricata':

```mkdir -p /var/log/suricata```

## Running suricata

Suricata can be launched using

```suricata -c /etc/suricata/suricata.yaml -i br-lan```

for listening one the masqueraded bridge interface.

For monitoring the WAN interface use

```suricata -c /etc/suricata/suricata.yaml -i eth1```

For monitoring both interfaces use

```suricata -c /etc/suricata/suricata.yaml -i br-lan -i eth1```

Statistical data will be logged in '/var/log/suricata/stats.log'.

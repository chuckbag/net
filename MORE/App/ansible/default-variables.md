# Default Variables

You can poll the local host and see what all the default ansible variables are.  

```bash
$ ansible -m setup localhost
[WARNING]: No inventory was parsed, only implicit localhost is available
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "198.18.2.138",
            "10.33.32.150"
        ],
        "ansible_all_ipv6_addresses": [
            "fe80::8cd:c062:e9b6:2f62%en0",
            "fe80::e849:ff:fe44:ddb1%awdl0",
            "fe80::e849:ff:fe44:ddb1%llw0",
            "fe80::93e4:d2d1:aa30:1099%utun0",
            "fe80::bba8:f525:33e7:7756%utun1",
            "fe80::aede:48ff:fe00:1122%en5"
        ],
        "ansible_apparmor": {
            "status": "disabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_awdl0": {
            "device": "awdl0",
            "flags": [
                "UP",
                "BROADCAST",
                "RUNNING",
                "PROMISC",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [
                {
                    "address": "fe80::e849:ff:fe44:ddb1%awdl0",
                    "prefix": "64",
                    "scope": "0xa"
                }
            ],
            "macaddress": "ea:49:00:44:dd:b1",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "1484",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "active",
            "type": "ether"
        },
        "ansible_bridge0": {
            "device": "bridge0",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "82:fa:07:24:dc:00",
            "media": "Unknown",
            "media_select": "Unknown",
            "media_type": "unknown type",
            "mtu": "1500",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_date_time": {
            "date": "2020-06-11",
            "day": "11",
            "epoch": "1591885815",
            "hour": "10",
            "iso8601": "2020-06-11T14:30:15Z",
            "iso8601_basic": "20200611T103015978266",
            "iso8601_basic_short": "20200611T103015",
            "iso8601_micro": "2020-06-11T14:30:15.978420Z",
            "minute": "30",
            "month": "06",
            "second": "15",
            "time": "10:30:15",
            "tz": "EDT",
            "tz_offset": "-0400",
            "weekday": "Thursday",
            "weekday_number": "4",
            "weeknumber": "23",
            "year": "2020"
        },
        "ansible_default_ipv4": {
            "address": "198.18.2.138",
            "broadcast": "198.18.2.255",
            "device": "en0",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "gateway": "198.18.2.1",
            "interface": "en0",
            "macaddress": "8c:85:90:58:03:0f",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "1500",
            "netmask": "255.255.255.0",
            "network": "198.18.2.0",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "active",
            "type": "ether"
        },
        "ansible_default_ipv6": {},
        "ansible_distribution": "MacOSX",
        "ansible_distribution_major_version": "10",
        "ansible_distribution_release": "19.5.0",
        "ansible_distribution_version": "10.15.5",
        "ansible_dns": {
            "nameservers": [
                "8.8.8.8"
            ],
            "search": [
                "cmed.com"
            ]
        },
        "ansible_domain": "0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa",
        "ansible_effective_group_id": 20,
        "ansible_effective_user_id": 501,
        "ansible_en0": {
            "device": "en0",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [
                {
                    "address": "198.18.2.138",
                    "broadcast": "198.18.2.255",
                    "netmask": "255.255.255.0",
                    "network": "198.18.2.0"
                }
            ],
            "ipv6": [
                {
                    "address": "fe80::8cd:c062:e9b6:2f62%en0",
                    "prefix": "64"
                }
            ],
            "macaddress": "8c:85:90:58:03:0f",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "1500",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "active",
            "type": "ether"
        },
        "ansible_en1": {
            "device": "en1",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "PROMISC",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "82:fa:07:24:dc:00",
            "media": "Unknown",
            "media_select": "autoselect",
            "media_type": "full-duplex",
            "mtu": "1500",
            "options": [
                "TSO4",
                "TSO6",
                "CHANNEL_IO"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_en10": {
            "device": "en10",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "a0:ce:c8:19:90:70",
            "media": "Unknown",
            "media_select": "autoselect",
            "media_type": "none",
            "mtu": "1500",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_en2": {
            "device": "en2",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "PROMISC",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "82:fa:07:24:dc:04",
            "media": "Unknown",
            "media_select": "autoselect",
            "media_type": "full-duplex",
            "mtu": "1500",
            "options": [
                "TSO4",
                "TSO6",
                "CHANNEL_IO"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_en3": {
            "device": "en3",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "PROMISC",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "82:fa:07:24:dc:01",
            "media": "Unknown",
            "media_select": "autoselect",
            "media_type": "full-duplex",
            "mtu": "1500",
            "options": [
                "TSO4",
                "TSO6",
                "CHANNEL_IO"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_en4": {
            "device": "en4",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "PROMISC",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "82:fa:07:24:dc:05",
            "media": "Unknown",
            "media_select": "autoselect",
            "media_type": "full-duplex",
            "mtu": "1500",
            "options": [
                "TSO4",
                "TSO6",
                "CHANNEL_IO"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_en5": {
            "device": "en5",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [
                {
                    "address": "fe80::aede:48ff:fe00:1122%en5",
                    "prefix": "64",
                    "scope": "0x4"
                }
            ],
            "macaddress": "ac:de:48:00:11:22",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "1500",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "active",
            "type": "ether"
        },
        "ansible_env": {
            "CLICOLOR": "1",
            "COLORFGBG": "7;0",
            "COLORTERM": "truecolor",
            "COMMAND_MODE": "unix2003",
            "DISPLAY": "/private/tmp/com.apple.launchd.jIV5YCFmPT/org.macosforge.xquartz:0",
            "EDITOR": "/usr/bin/vim",
            "HOME": "/Users/cm",
            "ITERM_PROFILE": "Default",
            "ITERM_SESSION_ID": "w0t0p1:77FCA598-5A8F-4C8E-8B5B-09B4215CEE19",
            "LANG": "en_US.UTF-8",
            "LC_TERMINAL": "iTerm2",
            "LC_TERMINAL_VERSION": "3.3.9",
            "LOGNAME": "cm",
            "LSCOLORS": "gxBxhxDxfxhxhxhxhxcxcx",
            "LS_COLORS": "di=1;93:fi=0:ln=31:pi=5:so=5:bd=5:cd=5:or=31:mi=0:ex=35:*.rpm=90",
            "LaunchInstanceID": "7516AA5F-7E42-471F-BA18-C9C4EA57C545",
            "PATH": "/Library/Frameworks/Python.framework/Versions/3.6/bin:/Library/Frameworks/Python.framework/Versions/3.6/bin:/opt/local/bin:/opt/local/sbin:/Users/cm/home/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/MacGPG2/bin:/opt/X11/bin:/Applications/Wireshark.app/Contents/MacOS:/Users/cm/bin",
            "PWD": "/Users/cm/techops/services/apps/ansible",
            "SECURITYSESSIONID": "186ab",
            "SHELL": "/bin/bash",
            "SHLVL": "3",
            "SSH_AUTH_SOCK": "/private/tmp/com.apple.launchd.eBVUgjwubA/Listeners",
            "TERM": "xterm-256color",
            "TERM_PROGRAM": "iTerm.app",
            "TERM_PROGRAM_VERSION": "3.3.9",
            "TERM_SESSION_ID": "w0t0p1:77FCA598-5A8F-4C8E-8B5B-09B4215CEE19",
            "TMPDIR": "/var/folders/jg/7hqm_8px1t96w7pwpkmmttx80000gn/T/",
            "USER": "cm",
            "XPC_FLAGS": "0x0",
            "XPC_SERVICE_NAME": "0",
            "_": "/usr/local/Cellar/ansible/2.9.7/libexec/bin/python3.8",
            "__CF_USER_TEXT_ENCODING": "0x1F5:0x0:0x0"
        },
        "ansible_fibre_channel_wwn": [],
        "ansible_fips": false,
        "ansible_fqdn": "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa",
        "ansible_gif0": {
            "device": "gif0",
            "flags": [
                "POINTOPOINT",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "unknown",
            "mtu": "1280",
            "type": "unknown"
        },
        "ansible_hostname": "Balsa",
        "ansible_hostnqn": "",
        "ansible_interfaces": [
            "awdl0",
            "bridge0",
            "en0",
            "en1",
            "en10",
            "en2",
            "en3",
            "en4",
            "en5",
            "gif0",
            "llw0",
            "lo0",
            "p2p0",
            "stf0",
            "utun0",
            "utun1",
            "utun2"
        ],
        "ansible_is_chroot": false,
        "ansible_iscsi_iqn": "",
        "ansible_kernel": "19.5.0",
        "ansible_kernel_version": "Darwin Kernel Version 19.5.0: Tue May 26 20:41:44 PDT 2020; root:xnu-6153.121.2~2/RELEASE_X86_64",
        "ansible_llw0": {
            "device": "llw0",
            "flags": [
                "UP",
                "BROADCAST",
                "SMART",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [
                {
                    "address": "fe80::e849:ff:fe44:ddb1%llw0",
                    "prefix": "64",
                    "scope": "0xc"
                }
            ],
            "macaddress": "ea:49:00:44:dd:b1",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "1500",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "status": "active",
            "type": "ether"
        },
        "ansible_lo0": {
            "device": "lo0",
            "flags": [
                "UP",
                "LOOPBACK",
                "RUNNING",
                "MULTICAST"
            ],
            "ipv4": [
                {
                    "address": "127.0.0.1",
                    "broadcast": "127.255.255.255",
                    "netmask": "255.0.0.0",
                    "network": "127.0.0.0"
                }
            ],
            "ipv6": [
                {
                    "address": "::1",
                    "prefix": "128"
                },
                {
                    "address": "fe80::1%lo0",
                    "prefix": "64",
                    "scope": "0x1"
                }
            ],
            "macaddress": "unknown",
            "mtu": "16384",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "type": "loopback"
        },
        "ansible_local": {},
        "ansible_lsb": {},
        "ansible_machine": "x86_64",
        "ansible_memfree_mb": 1376,
        "ansible_memtotal_mb": 16384,
        "ansible_model": "MacBookPro14,3",
        "ansible_nodename": "Balsa.local",
        "ansible_os_family": "Darwin",
        "ansible_osrevision": "199506",
        "ansible_osversion": "19F101",
        "ansible_p2p0": {
            "device": "p2p0",
            "flags": [
                "UP",
                "BROADCAST",
                "RUNNING",
                "SIMPLEX",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "0e:85:90:58:03:0f",
            "media": "Unknown",
            "media_select": "autoselect",
            "mtu": "2304",
            "options": [
                "CHANNEL_IO"
            ],
            "status": "inactive",
            "type": "ether"
        },
        "ansible_pkg_mgr": "homebrew",
        "ansible_processor": "Intel(R) Core(TM) i7-7700HQ CPU @ 2.80GHz",
        "ansible_processor_cores": "4",
        "ansible_processor_vcpus": "8",
        "ansible_product_name": "MacBookPro14,3",
        "ansible_python": {
            "executable": "/usr/local/Cellar/ansible/2.9.7/libexec/bin/python3.8",
            "has_sslcontext": true,
            "type": "cpython",
            "version": {
                "major": 3,
                "micro": 2,
                "minor": 8,
                "releaselevel": "final",
                "serial": 0
            },
            "version_info": [
                3,
                8,
                2,
                "final",
                0
            ]
        },
        "ansible_python_version": "3.8.2",
        "ansible_real_group_id": 20,
        "ansible_real_user_id": 501,
        "ansible_selinux": {
            "status": "Missing selinux Python library"
        },
        "ansible_selinux_python_present": false,
        "ansible_service_mgr": "launchd",
        "ansible_ssh_host_key_dsa_public": "AAAAB3NzaC1kc3MAAA3BAPN03ZQP43AW9XFYTFmPfv3AT248ZG8vmuIBH1Jpr2nAPEyqWsv11823odQZ1NDLPrQGg0H0RBoGyudNxMJckEDdk5RzYCojQJrpPyaHeAAMPN8Eg+sxfrQfValv/uiQVFoI1sKoWjOFLhVpptNiep15HUvT8Ha5XlVAHDUwwTTRAAAAFQCpGfy/FdlaxUdqA6Frw7sLW4h8awAAAIEAs0ONUWiuoOUt55nJd1WYBugvG2rXFjruJZUO4grCiyeWbjdvz/26n4AmPa+3i96JOoV+QBMqrktN/Ay46I9psdi4n4fUe9FntP3pRsuhinO8CQOEtaamMtJJ0d1L1wdgO/EJlD7vItAmxyID4qt70BrF1RB20wrnJe9iF8Y1H38AAACAc+KdW5qoJg8VVP9odA0lqzEwwtl6KKuE4SxIDwnwRg9otAw0t+BvOpAnD/Mi38Hj76++qN5BB8CVlyqOV3/PW4yvqMyqfj6JWqGa7ReAhza6o9Bouv3OTcbX8dfb/W/4EsuvuzXyjvIX/eeSgocsxL5QcQzSxJP+buidllJs2iE=",
        "ansible_ssh_host_key_ecdsa_public": "AAAAE2VjZHNhLXNo3TItbmlz3HAyNTYAAAAIbmlzdHAyNTYAAABBBAvoYk6a+pWKCVqDDrYC6ytySvNFRmNI1voWegERO/Yub4Q1fL/rzZmc1KUBUCWivUzCeJzHKte40oNIMYS4Zic=",
        "ansible_ssh_host_key_ed25519_public": "AAAAC3NzaC1lZDI3NTE5AAAAIHn24F4kGdrizQ74Lk+7lJqJZgcwQcLswvSkd4UQzPPi",
        "ansible_ssh_host_key_rsa_public": "AAAAB3NzaC1yc2EAAAADAQABAAABAQDCx3IBvNndg4HwrE/eUHSHPTn88iEJxJOTHbHV0zEC6FITHQ7mkCs+dRdBq+WILU1/TZcWv74aSePRk5hweFasy5FgvnymPl9i+8TfUcxVGFrfGMp6zfke41AP0MRL5XhfKvwuH/VpMXMha5c3WOpG4HqUFthqf/6P1N0dT0Q0UL3wj6KChlYgTNajzXgB92anZZ4jvewHz/9SjKl64nn0zmo5RRLByjyJcfvnRJNI6rPsfr+Mw1z6Ka/jzLiHYAtOufsG/qYCO+9aLAQ7WaHEfaIqGfYzZ7pQLJI1rlKi+01UA+o6okcsNt63D/+JD0TaPjbz1JsFPrHTuPKGgc+F",
        "ansible_stf0": {
            "device": "stf0",
            "flags": [],
            "ipv4": [],
            "ipv6": [],
            "macaddress": "unknown",
            "mtu": "1280",
            "type": "unknown"
        },
        "ansible_system": "Darwin",
        "ansible_user_dir": "/Users/cm",
        "ansible_user_gecos": "chuckie cheese",
        "ansible_user_gid": 20,
        "ansible_user_id": "cm",
        "ansible_user_shell": "/bin/bash",
        "ansible_user_uid": 501,
        "ansible_userspace_architecture": "x86_64",
        "ansible_userspace_bits": "64",
        "ansible_utun0": {
            "device": "utun0",
            "flags": [
                "UP",
                "POINTOPOINT",
                "RUNNING",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [
                {
                    "address": "fe80::93e4:d2d1:aa30:1099%utun0",
                    "prefix": "64",
                    "scope": "0xf"
                }
            ],
            "macaddress": "unknown",
            "mtu": "1380",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "type": "unknown"
        },
        "ansible_utun1": {
            "device": "utun1",
            "flags": [
                "UP",
                "POINTOPOINT",
                "RUNNING",
                "MULTICAST"
            ],
            "ipv4": [],
            "ipv6": [
                {
                    "address": "fe80::bba8:f525:33e7:7756%utun1",
                    "prefix": "64",
                    "scope": "0x10"
                }
            ],
            "macaddress": "unknown",
            "mtu": "2000",
            "options": [
                "PERFORMNUD",
                "DD"
            ],
            "type": "unknown"
        },
        "ansible_utun2": {
            "device": "utun2",
            "flags": [
                "UP",
                "POINTOPOINT",
                "RUNNING",
                "MULTICAST"
            ],
            "ipv4": [
                {
                    "address": "10.33.32.150",
                    "broadcast": "0xffffffff",
                    "netmask": "10.33.32.150",
                    "network": "10.33.32.150"
                }
            ],
            "ipv6": [],
            "macaddress": "unknown",
            "mtu": "1200",
            "options": [
                "RXCSUM",
                "TXCSUM",
                "CHANNEL_IO",
                "PARTIAL_CSUM",
                "ZEROINVERT_CSUM"
            ],
            "type": "unknown"
        },
        "ansible_virtualization_role": "",
        "ansible_virtualization_type": "",
        "gather_subset": [
            "all"
        ],
        "module_setup": true
    },
    "changed": false
}
$
```


## References: 
- [Where can I get a list of Ansible pre-defined variables?](https://stackoverflow.com/questions/18839509/where-can-i-get-a-list-of-ansible-pre-defined-variables): Stackoverflow, Aug 2015
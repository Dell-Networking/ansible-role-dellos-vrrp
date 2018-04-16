VRRP role
=========

This role facilitates configuring virtual router redundancy protocol (VRRP) attributes. It supports the creation of VRRP groups for interfaces and setting the VRRP group attributes. This role is abstracted for dellos9. 

The VRRP role requires an SSH connection for connectivity to a Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the *provider* dictionary.

Installation
------------

    ansible-galaxy install Dell-Networking.dellos-vrrp

Role variables
--------------

- Role is abstracted using the *ansible_net_os_name* variable that can take the dellos9 value
- If *dellos_cfg_generate* is set to true, the variable generates the role configuration commands in a file.
- Any role variable with a corresponding state variable set to absent negates the configuration of that variable
- Setting an empty value for any variable negates the corresponding configuration
- *dellos_vrrp* (dictionary) holds a dictionary with the interface name key
- Interface name can correspond to any of the valid dellos interface with a unique interface identifier name
- Physical interfaces names must be in `<interfacename> <tuple>` format (for example *fortyGigE 1/1*)
- Logical interface names must be in `<logical_interfacename> <id>` format (for example, *vlan 1* for dellos9)
- Variables and values are case-sensitive


| Key        | Type                      | Description                                             | Support               |
|------------|---------------------------|---------------------------------------------------------|-----------------------|
| ``vrrp``    | dictionary  | Configures VRRP commands (see ``vrrp.*``) | dellos6, dellos9, dellos10 |
| ``delay_min``      | integer      | Configures the minimum delay timer applied after interface up event (0 to 900 | dellos6, dellos9, dellos10 |
| ``delay_reload``     | integer      | Configures the minimum delay timer applied after boot (0 to 900) | dellos6, dellos9, dellos10 |
| ``vrrp_group``    | list  | Configures VRRP group commands (see ``vrrp_group.*``) | dellos6, dellos9, dellos10 |
| ``vrrp_group.type``  | string: ipv6,ipv4      | Specifies the type of the vrrp-group | dellos6, dellos9, dellos10 |
| ``vrrp_group.group_id``    | integer (required)  | Configures the ID for the vrrp-group (1 to 255) | dellos6, dellos9, dellos10 |
| ``vrrp_group.description``      | string          | Configures a single line description for the VRRP group | dellos6, dellos9, dellos10 |
| ``vrrp_group.virtual_address``  | string          | Configures virtual-address to the vrrp-group (A.B.C.D format) | dellos6, dellos9, dellos10 |
| ``vrrp_group.enable``      | boolean: true,false        | Enables/disables the vrrp-group at the interface  | dellos6, dellos9, dellos10 |
| ``vrrp_group.preempt``      | boolean: true\*,false          | Configures preempt mode on the VRRP group | dellos6, dellos9, dellos10 |
| ``vrrp_group.priority``      |integer          | Configures priority for the vrrp-group (1 to 255; default 100)  | dellos6, dellos9, dellos10 |
| ``vrrp_group.version``     | string: 2\*,3,both          | Configures the VRRP version of the vrrp-group; not supported when *vrrp_group.type* is "ipv6" | dellos6, dellos9, dellos10 |
| ``vrrp_group.hold_time_centisecs``    | integer          | Configures the hold-time for the vrrp-group in centiseconds (0 to 65525 and in multiple of 25; default 100); centisecs gets converted into seconds in version 2  | dellos6, dellos9, dellos10 |
| ``vrrp_group.adv_interval_centisecs``      | integer         | Configures the advertisement interval for the vrrp-group in centiseconds (25 to 4075; default 100) and in multiple of 25; centisecs gets converted into seconds in version 2 | dellos6, dellos9, dellos10 |
| ``vrrp_group.track_interface``      | list       | Configures the track interface of the vrrp-group (see ``track.*``) | dellos6, dellos9, dellos10 |
| ``track_interface.resource_id``      | integer       | Configures the object tracking resource ID of the vrrp-group; mutually exclusive with *track.interface* | dellos6, dellos9, dellos10 |
| ``track_interface.interface``      | string       | Configures the track interface of the vrrp-group (<interface name> <interface number> format) | dellos6, dellos9, dellos10 |
| ``track_interface.priority_cost``      | integer       | Configures the priority cost for track interface of the vrrp-group (1 to 254; default 10) | dellos6, dellos9, dellos10 |
| ``track_interface.state``       | string:present\*,absent          | Deletes the particular track interface from vrrp-group if set to absent | dellos6, dellos9, dellos10 |
| ``vrrp_group.track_interface_state``       | string:present*,absent          | Deletes all track interfaces from vrrp-group if set to absent | dellos6, dellos9, dellos10 |
| ``vrrp_group.authentication``      | dictionary       | Configures the authentication type of simple for the vrrp-group (see ``authentication.*``); not supported when ``vrrp_group.type`` is "ipv6" | dellos6, dellos9, dellos10 |
| ``authentication.key`` | string (required): 0,7,LINE           | Configures the authentication key for vrrp-group | dellos6, dellos9, dellos10 |
| ``authentication.key_string`` | string           | Configures the user key string; if key is 7, this variable takes the hidden user key string; if key is 0, takes the unencrypted user key (clear-text); supported only if the value of authentication.key is 7 or 0         | dellos6, dellos9, dellos10 |
| ``authentication.state``       | string:present\*,absent          | Deletes authentication from interface vrrp-group if set to absent | dellos6, dellos9, dellos10 |
| ``vrrp_group.state``       | string:present\*,absent          | Deletes vrrp-group from the interface if set to absent | dellos6, dellos9, dellos10 |
                                                                                                 
> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Connection variables
--------------------

Ansible Dell EMC Networking roles require connection information to establish communication with the nodes in your inventory. This information can exist in the Ansible *group_vars* or *host_vars* directories, or in the playbook itself.

| Key         | Required | Choices    | Description                                         |
|-------------|----------|------------|-----------------------------------------------------|
| ``host`` | yes      |            | Specifies the hostname or address for connecting to the remote device over the specified transport |
| ``port`` | no       |            | Specifies the port used to build the connection to the remote device; if value is unspecified, it defaults to 22 |
| ``username`` | no       |            | Specifies the username that authenticates the CLI login for connection to the remote device; if value is unspecified, the ANSIBLE_NET_USERNAME environment variable value is used  |
| ``password`` | no       |            | Specifies the password that authenticates the connection to the remote device; if value is unspecified, the ANSIBLE_NET_PASSWORD environment variable value is used |
| ``authorize`` | no       | yes, no\*   | Instructs the module to enter privileged mode on the remote device before sending any commands; if value is unspecified, the ANSIBLE_NET_AUTHORIZE environment variable value is used, and the device attempts to execute all commands in non-privileged mode. This key is supported only in dellos9 and dellos6. |
| ``auth_pass`` | no       |            | Specifies the password to use if required to enter privileged mode on the remote device; if *authorize* is set to no, this variable does not apply; if value is unspecified, the ANSIBLE_NET_AUTH_PASS environment variable value is used . This key is supported only in dellos9 and dellos6. |
| ``provider`` | no       |            | Passes all connection arguments as a dictonary object; all constraints (such as required or choices) must be met either by individual arguments or values in this dictionary |

> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Dependencies
------------

The *dellos-vrrp* role is built on modules included in the core Ansible code. These modules were added in Ansible version 2.2.0.

Example playbook
----------------

This example uses the *dellos-vrrp* role to configure VRRP commands at the interfaces. It creates a *hosts* file with the switch details and corresponding variables. The hosts file should define the *ansible_net_os_name* variable with corresponding Dell EMC networking OS name. When *dellos_cfg_generate* is set to true, the variable generates the configuration commands as a .part file in *build_dir* path. By default, the variable is set to false. It writes a simple playbook that only references the *dellos-vrrp* role.

**Sample hosts file**

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9)>

**Sample host_vars/leaf1**
     
    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
    build_dir: ../temp/dellos9
    dellos_vrrp:
        fortyGigE 1/5:
          vrrp:
            delay_min: 2
            delay_reload: 3
          vrrp_group:
            - group_id: 2
              type: ipv6
              description: "Interface-vrrp-ipv6"
              virtual_address: 2001:4898:5808:ffa3::9
              enable: true
              priority: 120
              preempt: false
              track_interface:
                - resource_id: 3
                  priority_cost: 25
                  state: present
                - interface: port-channel 120
                  priority_cost: 20
                - interface: fortyGigE 1/11
                      state: present
              track_interface_state: present
              adv_interval_centisecs: 200
              hold_time_centisecs: 20
            - group_id: 4
              state: present
              description: "Interface-vrrp4"
              virtual_address: 10.28.0.2
              enable: true
              priority: 120
              preempt: false
              version: both
              track_interface:
                - resource_id: 3
                  priority_cost: 25
                  state: present
                - interface: port-channel 120
                  priority_cost: 20
                - interface: fortGigE 1/10
                  state: present
              track_interface_state: present
              adv_interval_centisecs: 225
              hold_time_centisecs: 25
              authentication:
                key: 0
                key_string: vrrpkey
                state: present

**Simple playbook to setup system - leaf.yaml**

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-vrrp
                
**Run**

    ansible-playbook -i hosts leaf.yaml

(c) 2017 Dell EMC

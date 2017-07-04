VRRP Role for Dell EMC Networking OS
====================================
This role facilitates configuring Virtual Router Redundancy Protocol (VRRP) attributes. It supports the creation of vrrp groups for interfaces and setting the vrrp group attributes.
This role is abstracted for dellos9. 

Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-vrrp
```

Requirements
------------

This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Role Variables
--------------

``dellos_vrrp`` (dictionary) contains the objects to configure VRRP.
This role is abstracted using the variable ``ansible_net_os_name`` that can take the following values: dellos9.

Any role variable with a corresponding state variable set to absent negates the configuration of that variable. 
For variables with no state variable, setting an empty value for the variable negates the corresponding configuration.
The variables and its values are case-sensitive.

The dellos_vrrp (dictionary) holds a dictionary with the interface name key. The interface name can correspond to any of the valid dellos interfaces with the unique interface identifier name.

For physical interfaces, the interface name must be in the format `<interfacename> <tuple>`. For logical interfaces, the format is `<logical_interfacename> <id>`.
For example, the physical interface name can be in the format fortyGigE 1/1 and the logical interface names can be in the format vlan 1 for dellos9.

``interface name`` of type list holds the following key values:

|       Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| vrrp    | dictionary  | Configures vrrp commands. See the following vrrp.* for each item .|
| delay_min      | integer      | Configures minimum delay timer applied after interface up event. The value can be in range 0-900. |
| delay_reload     | integer      | Configures minimum delay timer applied after boot. The value can be in range 0-900. |
| vrrp_group    | list  | Configures vrrp group commands. See the following vrrp_group.* for each item .|
| vrrp_group.type  | string, choices: ipv6,ipv4      | Specifies the type of the VRRP group. |
| vrrp_group.group_id    | integer(required)  |Configures id for the vrrp-group. It can be in the range 1-255 .|
| vrrp_group.description      | string          |Configures one line description for the VRRP group. |
| vrrp_group.virtual_address  | string          |Configures virtual-address to the vrrp-group. Ip address is specified in the format A.B.C.D |
| vrrp_group.enable      | boolean: true, false        |Enables/Disables the VRRP group at the interface.  |
| vrrp_group.preempt      | boolean: true*, false          |Configures preempt mode on the VRRP group. |
| vrrp_group.priority      |integer, default=100          |Configures priority for the VRRP group. The value can be in the range 1-255.  |
| vrrp_group.version     | string, choices=2*,3,both          |Configures VRRP version of the VRRP group. This key is not supported when vrrp_group.type is "ipv6". |
| vrrp_group.hold_time_centisecs    | integer, default=100          |Configures hold time for the VRRP group in centi-seconds. The value can be in the range 0-65525 and in multiple of 25. For version 2, the centisecs gets converted into secs.  |
| vrrp_group.adv_interval_centisecs      | integer, default=100          |Configures advertisement interval for the VRRP group in centi-seconds. The value can be in the range 25-4075 and in multiple of 25. For version 2, the centisecs gets converted into secs. |
| vrrp_group.track_interface      | list       | Configures track interface of the VRRP group. See the following track.* for each item. |
| track_interface.resource_id      | integer       | Configures object tracking resource ID of the VRRP group. This key is mutually exclusive with track.interface. |
| track_interface.interface      | string       | Configures track interface of the VRRP group. The value can be in form &lt;interface name&gt; &ltinterface number&gt;. |
| track_interface.priority_cost      | integer, default=10       | Configures priority cost for track interface of the VRRP group. The value can be in range 1-254. |
| track_interface.state       | string, choices:present*,absent          |If set to absent, removes the particular track interface from vrrp-group. |
| vrrp_group.track_interface_state       | string, choices:present*,absent          |If set to absent, removes all track interfaces from vrrp-group. | 
| vrrp_group.authentication      | dictionary       | Configures authentication type of simple for the VRRP group. See the following authentication.* for each item.This key is not supported when vrrp_group.type is "ipv6". |
| authentication.key | string (required), choices: 0,7,LINE           | Configures the authentication key for VRRP group. |
| authentication.key_string | string           | Configures the user key string. If key is 7, this variable takes the hidden user key string. If key is 0, takes the unencrypted user key(clear text). This variable is supported only if the value of authentication.key is 7 or 0.                            |
| authentication.state       | string, choices:present*,absent          |If set to absent, removes authentication from interface vrrp-group. |
| vrrp_group.state       | string, choices:present*,absent          |If set to absent, removes vrrp-group from the interface. |
                                                                                                      

```
Note: Asterisk (*) denotes the default value if none is specified.
```

Connection Variables
--------------------

Ansible Dell EMC Networking roles require the following connection information to establish 
communication with the nodes in your inventory. This information can exist in
the Ansible group_vars or host_vars directories, or in the playbook itself.


|         Key | Required | Choices    | Description                              |
| ----------: | -------- | ---------- | ---------------------------------------- |
|        host | yes      |            | Hostname or address for connecting to the remote device over the specified ``transport``. The value of this key is the destination address for the transport. |
|        port | no       |            | Port used to build the connection to the remote device. If this key does not specify the value, the value defaults to 22. |
|    username | no       |            | Configures the username that authenticates the connection to the remote device. The value of this key authenticates the CLI login. If this key does not specify the value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
|    password | no       |            | Specifies the password that authenticates the connection to the remote device. If this key does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
|   authorize | no       | yes, no*   | Instructs the module to enter privileged mode on the remote device before sending any commands. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. If not specified, the device attempts to execute all commands in non-privileged mode.|
|   auth_pass | no       |            | Specifies the password to use if required to enter privileged mode on the remote device. If ``authorize`` is set to no, then this key is not applicable. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
|   transport | yes      | cli*       | Configures the transport connection to use when connecting to the remote device. This key supports connectivity to the device over CLI (SSH).  |
|    provider | no       |            | Convenient method that passes all of the above connection arguments as a dictonary object. All constraints (such as required or choices) must be met either by individual arguments or values in this dictonary. |


```
Note: Asterisk (*) denotes the default value if none is specified.
```

Dependencies
------------

The Dell-Networking.dellos-vrrp role is built on modules included in the core Ansible code.
These modules were added in Ansible version 2.2.0.

Example Playbook
----------------
The following example uses the dellos-vrrp role to configure VRRP commands at the interfaces. The example creates a ``hosts`` file with the switch details and corresponding variables. The hosts file should define the variable `` ansible_net_os_name `` with corresponding Dell EMC networking OS name.
It writes a simple playbook that only references the dellos-vrrp role.


Sample hosts file:

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9)>

Sample ``host_vars/leaf1``:
     
    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
	  auth_pass: xxxxx 
      transport: cli
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



Simple playbook to setup system, ``leaf.yaml``:

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-vrrp
                
Then run with:

    ansible-playbook -i hosts leaf.yaml

License
--------

Copyright (c) 2017, Dell Inc. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

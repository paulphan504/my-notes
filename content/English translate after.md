Module 1: Network Basic (Foundation)

- TCP/IP - OSI Model:
    + Isolate or evaluate incidents to provide accurate solutions.
    + A comprehensive overview of all IT areas.

OSI 7-Layer Model:

| OSI | TCP/IP |
|---|---|
| 7. Application | Application |
| 6. Presentation | |
| 5. Session | |
| 4. Transport | Transport |
| 3. Network | Internet |
| 2. Datalink | Network Access |
| 1. Physical | |

____________________________________
- Hub n port = 1 Broadcast Domain, 1 Collision Domain
- Switch n port = 1 Broadcast Domain, n Collision Domains
- Router n port = n Broadcast Domains, n Collision Domains

- ARP resolves IP to MAC: In the entire process, the IP remains unchanged, only the MAC changes in each segment.

Module 2: Switching

1. VLAN, Trunking:
    - a. VLAN
        - Switch only plugged in, no configuration, only 1 VLAN
        - Switch with configured VLAN: 4096 - 2 = 4094 VLANs (switchport access vlan vlan-id)
        ```bash
        Switch(config)#switchport mode access
        Switch(config)#switchport access vlan vlan-id
        ```

    - b. Trunking
        - To lead VLANs between 2 switches, access both ends of 1 VLAN (10 VLANs require 10 cables).
        - Trunking allows leading 4094 VLANs on 1 cable (only 1 cable for 4094 VLANs).
        - Trunking: ISL (Cisco), Dot1q (Standard)
        ```bash
        Switch(config)#switchport trunk encapsulation dot1q
        Switch(config)#switchport mode trunk
        ```

    - c. VTP:
        - Synchronize VLANs on Cisco switches
        ```bash
        Switch(config)#vtp domain abc.com
        Switch(config)#vtp mode server/client
        ```
        - VTP has 3 modes: Server (Create VLAN, Sync VLAN), Transparent (Create VLAN, Not Sync VLAN), Client (Not Create VLAN, Sync VLAN).

    - d. Sub-interface on Router (Router interacts with Switch port Trunk) - assign VLAN (layer 2) to Router's port (Layer 3)
        ```bash
        Router(config)#interface f0/0.10
        Router(config)#encapsulation dot1q 10
        Router(config)#ip address 172.16.10.1 255.255.255.0
        ```

2. CDP - LLDP: Used to view device information
    ```bash
        Router(config)#cdp run
        Router(config)#lldp run
        Router#show cdp neighbor
    ```
3. DHCP: Automatically assigns IP to a VLAN.
    - Requires Routing to be complete.
    - For DHCP Relay Agent, enter the correct Gateway (configured by DHCP Server) to point to DHCP Server (ip helpder-address "IP-DHCP-Server")

4. Interface VLAN: Often acts as Gateway for the entire network.
    - 2-layer model: Interface-vlan configured on Switch-Core
    - 3-layer model: Interface-vlan usually configured on Switch-Distribution (can be configured on Switch-Core)

5. STP: Prevents loops in Layer 2 environment.
    - When switches are interconnected in a loop, a loop occurs.
    - Temporarily block a random port to prevent the loop.

    Step 1:
    - Elect Root Switch
    - Priority: Lowest is best (difference of n +/- 4096, default priority Switch = 32768)
    - MAC: Lowest is best. --> All ports of Root Switch are DP (Designated Port)

    Step 2:
    - Elect Root Port
    - Calculate Cost: Cost calculated from Root Switch to other ports, lowest Cost is best (Root Port - RP).

    Step 3: Block port (worst port) - Block all VLANs on this segment.
    - Cost: Lowest is best
    - Priority of Sender-ID: Lowest is best
    - MAC: Lowest is best.

    Port status:
    1. From normal port to Forwarding: 30s (Listening ->(15s) Learning ->(15s) Forwarding)
    2. From Block port to Forwarding: 50s (Blocking ->(20s) Listening ->(15s) Learning ->(15s) Forwarding)

    - STP portfast: Used for faster convergence of Access port (only affects Access port, not trunk port)
        - Enter individual interface port access to configure portfast
        ```bash
        Switch(config-if)#spanning-tree portfast
        ```

        - Configure portfast on config mode (apply to all Access ports, except trunk port)
        ```bash
        Switch(config)#spanning-tree portfast default.
        ```
        - Per-Vlan-STP (Cisco only): PVSTP+ each VLAN will have a separate STP

6. Increasing Redundancy:
    - a. Cable: Etherchannel - port channel
        - Cisco: PAgP (desirable - auto)
        - Standard: LaCP (active - passive)
        - no protocol: on - off

    - b. Switch: stackwise
        - Expansion: Switches don't need to be identical in device, configuration, or number of ports
        - Backup: Switches must be identical in device, configuration, and number of ports.

    - c. Layer3 Redundancy (Switch Layer3 / Router) - FHRP:
        - Cisco: HSRP only supports 2 devices (1 Active - 1 Standby)
        - Standard: VRRP supports up to 16 devices (1 Active - others Passive)
        - --> Creates a virtual GW representing 2 Routers (only Active Router is active).

7. Security Layer2 (starts at the company)
    - a. MAC table attack: flooding the MAC table of the Switch (Switch becomes Hub when MAC table is flooded)
        Objectives:
        - Turn Switch into Hub to capture information.
        - Make Switch crash (malicious attack).
        - --> Limit port speed, use port Security to limit number of MACs on 1 port.

        Port Security:
        1. Learn MAC: default 1 MAC when port Security is enabled
        - Static: assign MAC address to port
        - Dynamic (default): learn MAC, relearn when reset -> lose MAC address when Switch resets or clears MAC table.
        - Sticky: learned MAC will be saved forever (unless Admin deletes MAC) combination of Dynamic and Static MAC learning.

        2. Actions: Protect, Restrict, Shutdown
        - Protect: no information sent to administrator, no impact on port.
        - Restrict: no impact on port, but sends warning to administrator.
        - Shutdown: impacts port (err-disable: to enable again, shutdown port first and no shutdown again), sends warning to administrator.

        - Default of port Security:
          ```bash
          Switch(config)#switchport port-security
          ```
        - Maximum number of MACs: 1
        - Learn MAC: Dynamic
        - Action: Shutdown

    - b. VLAN Hopping:
        - Transform into Trunk port and use VLAN Native to probe user/Server information in other VLANs. Trunk ------ Access = N/A --> configure all unused ports as Access ports and shutdown unused ports.

    - c. Modify STP:
        - Attach a SW with lowest MAC and Priority to network port (becomes Root Switch) making all traffic in network go through this Switch (to capture user information). --> Impacts network access and slows down network. --> Enable BPDU Guard on Access port (Do not receive BPDU from Access port)

    - d. DHCP snooping - ARP spoofing:
        - Spoof DHCP or a server to capture user information. --> enable dhcp snooping on Access port to prevent DHCP spoofing (prohibit setting static IP on ports configured with dhcp snooping - violation causes Err-disable). --> Trusted port trunk to receive DHCP from uplink, not downlink.

Module3: Routing

1. Static Route:
    - Routing is completely active according to the administrator's wishes.
    - --> Easy to configure for small and medium-sized models
    - --> Easy to troubleshoot (due to complete control by the administrator)

    - --> Difficult to configure for large network models.
    - --> Junk Static Route (some Routing no longer exists but not deleted) but not dare to delete because don't know if it affects other parts.
    - Solutions:
        - Always remark static route commands.

2. Dynamic Route: OSPF
    - a. Router-id:
        - IP representing Router when establishing

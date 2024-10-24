**<mark style="background: #BBFABBA6;">Module 1: Network Basic (Foundation)</mark>**
- Mô hình TCP/IP - OSI:
	+ Cô lập sự việc hay đánh giá sự việc để đưa ra phương án xử lý chính xác.
	+ Góc nhìn tổng thể của tất cả lĩnh vực của IT.

OSI mô hình 7 lớp:

| OSI	        <br>                                                                                                                     | TCP/IP                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------- |
| 7. Application  <br>6. Presentation  <br>5. Session	  <br>4. Transport     <br>3. Network       <br>2. Datalink      <br>1. Physical | <br>Application<br><br>Transport<br>Internet<br>Network Access |

____________________________________
- Hub n port    = 1 Broadcast Domain, 1 Collision Domain
- Switch n port = 1 Broadcast Domain, n Collision Domain
- Router n port = n Broadcast Domain, n Collision Domain

- ARP phân giải IP thành MAC: trong toàn bộ quá trình thì IP không đổi chỉ có MAC là thay đổi trong từng phân đoạn.

**<mark style="background: #BBFABBA6;">Module 2: Switching</mark>**
1. VLAN, Trunking:
	- a. VLAN	
		- Switch chỉ cắm vào xài, không cấu hình được là chỉ có 1 VLAN
		- Switch có cấu hình VLAN thì có 4096 - 2 = 4094 VLAN (switchport access vlan vlan-id)
	  ```bash
		Switch(config)#switchport mode access
		Switch(config)#switchport access vlan vlan-id
		```

	- b. Trunking
		- Muốn dẫn VLAN thì giữa 2 switch phải access 2 đầu 1 VLAN (10 VLAN thì tốn 10 dây cáp để dẫn 10 VLAN).
		- Trunking ra đời để dẫn 4094 VLAN trên 1 dây (chỉ tốn kém 1 dây cáp cho 4094 VLAN).
		- Trunking: ISL (Cisco), Dot1q (Standard)
		```bash
		Switch(config)#switchport trunk encapsulation dot1q
		Switch(config)#switchport mode trunk
		```
	
	- c. VTP: 
		- đồng bộ VLAN trên switch Cisco
	  ```bash
		Switch(config)#vtp domain abc.com
		Switch(config)#vtp mode server/client
		```
		- VTP có 3 mode: Server (Create VLAN, Sync VLAN), Transparent (Create VLAN, Not Sync VLAN), Client (Not Create VLAN, Sync VLAN).
	
	- d. Sub-interface trên Router (Router giao tiếp với Switch port Trunk) - gán vlan (layer 2) lên port của Router (Layer 3)
		```bash
		Router(config)#interface f0/0.10
		Router(config)#encapsulation dot1q 10
		Router(config)#ip address 172.16.10.1 255.255.255.0
		```

2. CDP - LLDP: dùng để xem thông tin của thiết bị
   ```bash
   		Router(config)#cdp run
		Router(config)#lldp run
		Router#show cdp neighbor
	```
3. DHCP: tính năng cấp IP 1 cách tự động cho 1 VLAN nào đó.
	- -> điều kiện đầu tiên để DHCP hoạt động là Routing đã hoàn tất.
	- -> Đối với DHCP Relay Agent thì vào đúng Gateway (mà DHCP Server cấu hình thông số Gateway) để trỏ về DHCP Server (ip helpder-address "IP-DHCP-Server")

4. Interface VLAN thường đóng vai trò làm Gateway cho toàn mạng.
    - Mô hình 2 lớp: Interface-vlan được cấu hình ở Switch-Core
	- Mô hình 3 lớp: Interface-vlan thường được cấu hình ở Switch-Distribution (có thể cấu hình trên Switch-Core)

5. STP: chống loop trong môi trường Layer 2.
	- Khi Switch được đấu nối thành vòng kín thì sẽ xảy ra hiện tượng loop.
	- -> Block tạm thời 1 port bất kỳ để kết nối không còn thành vòng kín.

	Bước 1: 
	-  bầu chọn Root Switch
	-  Priority: thấp nhất là tốt nhất (cách nhau n +/- 4096, mặc định priority Switch = 32768)
	-  MAC: thấp nhất là tốt nhất. --> Toàn bộ port của Root Switch là DP (Designated Port)
	
	Bước 2:
	-  bầu chọn Root Port
	- Tính theo Cost: Cost tính từ Root Switch đến các port còn lại, Cost nào thấp nhất là tốt nhất (Root Port - RP).
	
	Bước 3: Block port (port tệ nhất) - Block all VLAN trên phân đoạn này.
	- Cost: thấp nhất là tốt nhất
	- Priority của Sender-ID: thấp nhất là tốt nhất
	- MAC: thấp nhất là tốt nhất.

	Trạng thái của port:
	1. Từ port bình thường sang port Forwarding: 30s (Listening ->(15s) Learning ->(15s) Forwarding)
	2. Từ port Block sang port Forwarding: 50s (Blocking ->(20s) Listening ->(15s) Learning ->(15s) Forwarding)
	
	- STP portfast: dùng để port Access hội tụ nhanh hơn (chỉ có tác động trên port Access, k tác động trên port trunk)
		- Vào từng interface port access để cấu hình portfast
		```bash
		Switch(config-if)#spanning-tree portfast
		```
	
		- Cấu hình portfast trên mode config (apply trên toàn bộ port access, ngoại trừ port trunk)
		```bash
		Switch(config)#spanning-tree portfast default.
		```
		- Per-Vlan-STP (Cisco mới hỗ trợ): PVSTP+ mỗi VLAN sẽ có 1 STP riêng

6. Tăng tính dự phòng (High Redundancy):
	- a. Cable: Etherchannel - port channel
		- Cisco: PAgP (desirable - auto)
		- Standard: LaCP (active - passive)
		- no protocol: on - off
	
	- b. Switch: stackwise
		- Mở rộng: không nhất thiết các Switch phải giống nhau về thiết bị, cấu hình, số port
		- Backup: bắt buộc Switch phải giống nhau về thiết bị, cấu hình và số port.
	
	- c. Dự phòng Layer3 (Switch Layer3 / Router) - FHRP:
		- Cisco: HSRP chỉ hỗ trợ 2 thiết bị ( 1 Active - 1 Standby)
		- Standard: VRRP hỗ trợ tối đa khoảng 16 thiết bị ( 1 Active - còn lại là Passive)
		- --> Sinh ra 1 GW ảo đại diện cho 2 Router (trong đó chỉ có Router Active là hoạt động).

7. Security Layer2 (bắt nguồn tại ngay công ty)
	- a. Tấn công bảng MAC: tràn bảng MAC của Switch (Switch khi tràn MAC sẽ trở thành Hub)
		Mục tiêu:
		- Chuyển Switch thành Hub để capture thông tin.
		- Làm Switch bị Down (tấn công phá hoại).
		- --> Giới hạn lại tốc độ của port, và dùng port Security để giới hạn số lượng MAC trên 1 port.

		Port Security:
		1. Học MAC: default 1 MAC khi bật port Security
		- Static: gán thẳng địa chỉ MAC vào port
		- Dynamic (default): học MAC, khi reset lại thì học lại -> mất địa chỉ MAC khi Switch reset hoặc clear bảng MAC. 
		- Sticky: học MAC xong sẽ lưu mãi mãi (ngoài khi Admin delete MAC) kết hợp giữa học MAC theo Dynamic và lưu MAC theo Static.
		
		2. Tác động: Protect, Restrict, Shutdown
		- Protect: không gửi thông tin gì về cho người quản trị, không tác động lên port.
		- Restrict: không tác động lên port, nhưng có gửi cảnh báo về cho người quản trị.
		- Shutdown: tác động lên port (err-disable: muốn enable lại thì shutdown port trước và no shutdown lại), và gửi cảnh báo về cho người quản trị.
		
		- Default của port Security:
		  ```bash
		  Switch(config)#switchport port-security
		  ```
		- Số lượng MAC tối đa: 1
		- Học MAC: Dynamic
		- Action: Shutdown

	- b. VLAN Hopping:
	  -  biến đổi thành port Trunk và dùng VLAN Native để dò thông tin người dùng/Server trong các VLAN khác. Trunk ------ Access = N/A --> cấu hình tất cả các port không sử dụng là port Access và shutdown các port không sử dụng.

	- c. Thay đổi STP: 
	  - gắn 1 SW có MAC và Priority thấp nhất vào port mạng (sẽ trở thành Root Switch) làm tất cả traffic trong mạng đều đi qua Switch này (để capture thông tin người dùng). -> Gây ảnh hưởng đến truy cập trong mạng và làm chậm mạng. --> Bật BPDU Guard trên port Access (Không nhận BPDU từ port ACcess)
	
	- d. DHCP snooping - ARP spoofing: 
	  - giả mạo DHCP hoặc 1 server nào đó để capture thông tin người dùng. --> bật dhcp snooping trên port Access để chống giả mạo DHCP (cấm đặt IP tĩnh trên các port cấu hình dhcp snooping - khi vi phạm sẽ tác động Err-disable). --> Trusted port trunk để nhận DHCP từ uplink, không nhận DHCP từ downlink.

<mark style="background: #BBFABBA6;">Module3: Routing</mark>

1. Static Route: 
   - việc định tuyến hoàn toàn chủ động theo ý muốn của người quản trị.
	- -> dễ cấu hình cho các mô hạng nhỏ và vừa 
	- -> dễ xử lý sự cố (do hoàn toàn chủ động theo ý muốn của người quản trị)
	
	- --> khó khăn cấu hình cho các mô hình mạng lớn.
	- --> bị rác Static Route (có những Routing không còn nữa nhưng chưa xóa) nhưng chưa dám xóa vì không biết có ảnh hưởng đến các phần khác hay không.
	- Khắc phục:
		- Luôn marking lại câu lệnh static route.
	
2. Dynamic Route: OSPF
	- a. Router-id: 
	  - IP đại diện cho Router khi thiết lập neighbor giữa các Router tham gia OSPF (chọn ra IP lớn nhất trong các interface để làm router-id khi không cấu hình router-id).
	- b. DR-BDR: 
	  - trong phân đoạn Broadcast MultiAccess thì sẽ có bầu chọn DR và BDR
		- DR: đảm nhiệm nhận toàn bộ LSA từ các DR-OTHER (224.0.0.5) và trả lời lại thông tin định tuyến cho các DR-OTHER (224.0.0.6)
		- BDR: chỉ nhận LSA từ các DR-OTHER (224.0.0.5) và chỉ trả lời khi DR bị Down.
		
		- DR chỉ có giá trị trong 1 phân đoạn mạng, nên việc cấu hình DR và BDR là vào interface để chỉnh Priority.
		- Priority Default = 1
		- Priority = 0 (không được tham gia bình chọn DR)
		- Priority = 255 (giá trị lớn nhất)
		
		- Router(config-if)#ip ospf priority 255 (chắc chắn là DR)
		- Router(config-if)#ip ospf priority 0  (không được tham gia bầu chọn DR)
	
	- c. Point-to-Point: 
	  - môi trường ngang hàng thì không có bầu chọn DR/BDR trong môi trường này các Router gửi LSA cho nhau và tự tính toàn định tuyến rồi gửi cho nhau thông tin định tuyến. 
	    ```bash
	    Router(config-if)#ip ospf network point-to-point
	    ```
	
	- d. Cost của OSPF: 
		```bash
			reference BW (10^8)
		Cost = _____________________
			Interface BW
			```
		
		Có thể thay đổi Refernce BW trong mode Router
		```bash
		Router(config)#router ospf 1
		Router(config-router)#ospf auto-cost reference-bandwidth 10000 (10GB)
		```
		
		
		Việc thay đổi Cost để tối ưu định tuyến theo 1 đường naò đó thì vào interface để cấu hình.
		```bash
		Router(config-if)#ip ospf cost 10 
		```
	
3. Access Control List (ACL):
	Dùng để lọc traffic (Filter) hoặc phân loại traffic (Classification) trong mạng 
	theo mong muốn của người quản trị.
	a. Tác động của ACL: Cấm (Deny) và Cho phép (Permit)
	b. Dạng của ACL: 
	- Standard: chỉ có thể định nghĩa duy nhất source IP (destination IP mặc định là ANY)
	--> thường dùng để phân loại subnet, phân loại VLAN (Classification)
	- Extended: định nghĩa bao gồm: protocol (ip/icmp/tcp/udp), source IP, destination IP, port-ID/ping/time-exceeded/...
	--> thường dùng để lọc traffic (Filter)
	
	VD: viết ACL để thực hiện các nội dung sau:
	- VLAN10: được truy cập web và ping (các phần còn lại k được truy cập).
	- VLAN20: được truy cập tất cả dịch vụ.
	- VLAN30: cấm truy cập web và traceroute.
	172.16.VLAN-ID.0/24
	
	Router(config)#ip access-list extended Local_LAN
	Router(config-acl)#permit tcp   172.16.10.0 0.0.0.255 any eq www
	Router(config-acl)#permit icmp 172.16.10.0  0.0.0.255 any 
	Router(config-acl)#deny   ip   172.16.10.0  0.0.0.255 any
	
	Router(config-acl)#permit ip   172.16.20.0 0.0.0.255  any
	
	Router(config-acl)#deny   tcp  172.16.30.0 0.0.0.255  any eq www
	Router(config-acl)#deny   icmp 172.16.30.0 0.0.0.255  any traceroute
	Router(config-acl)#permit ip   172.16.30.0 0.0.0.255  any
	
	Router(config-acl)#deny ip any any (implicit deny - HIDDEN)
	
4. Network Address Translation (NAT): chuyển đổi IP/port-A sang IP/port-B
	Source NAT: nếu >1 thì định nghĩa = ACL
	Destination NAT: nếu >1 thì định nghĩa = POOL
	
	a. Static NAT: 1 -> 1
	- Source:       n IP-A = 1 -> không định nghĩa ACL
	- Destination:  n IP-B = 1 -> không định nghĩa Pool
	
	Router(config)#ip nat inside source static 172.16.1.1 100.0.0.1
	
	b. Dynamic NAT: n IP-A -> n IP-B
	- Source:      n IP-A > 1 -> định nghĩa ACL
	- Destination: n IP-B > 1 -> định nghĩa Pool
	172.16.1.1 - 172.16.1.12 sang IP 100.0.0.2 - 100.0.0.13
	Router(config)#ip access-list standard NAT-1.1-1.12
	Router(config-acl)#permit 172.16.1.0 0.0.0.15
	
	Router(config-acl)#ip nat pool MY_POOL 100.0.0.2 100.0.0.13 subnet-mask 255.255.255.240
	Router(config-acl)#ip nat pool MY_POOL 100.0.0.2 100.0.0.13 prefix-length 28
	
	Router(config)#ip nat inside source list NAT-1.1-1.12 MY_POOL 
	
	C. NAT overload: n IP-A -> 1 IP-B
	- Source:       n IP-A > 1 -> định nghĩa ACL
	- Destination:  n IP-B = 1 -> không định nghĩa Pool
	
	172.16.1.0/24 NAT ra ngoài với interface g0/0 kết nối với nhà mạng
	172.16.2.0/24 NAT ra ngoài với IP Public còn lại 100.0.0.14
	Router(config)#ip access-list standard Internet_1.0
	Router(config-acl)#permit 172.16.1.0 0.0.0.255
	
	Router(config)#ip nat inside source list Internet_1.0 interface g0/0 overload
	
	Router(config)#ip access-list standard Internet_2.0
	Router(config-acl)#permit 172.16.1.0 0.0.0.255
	
	Router(config)#ip nat inside source list Internet_2.0 100.0.0.14 
	
5. IPv6: 
	Trên interface sẽ có 2 IPv6
	- 1 IPv6 là địa chỉ Link-Local được tự động cấu hình bằng Link-Local EUI-64
	EUI-64 là tách đôi địa chỉ MAC, chèn giữa FF-FE và nghịch đảo bit số 7 của địa chỉ MAC
	--> Link-Local chỉ có tác động trên 1 Link thôi nên khi ping kiểm tra sẽ kèm theo Outbound-interface
	--> 1 Router có thể dùng 1 địa chỉ Link-Local cho toàn Interface.
	Mục tiêu của địa chỉ Link-Local là để nhận biết giữa các Router với nhau.
	
	- 1 IPv6 cấu hình lên Interface (IPv6 này dùng để định tuyến)
	--> mỗi Interface phải có địa chỉ IPv6 khác nhau.
	
	Bật IPv6:
	Router(config)#ipv6 unicast routing
	sau khi gõ câu lệnh này thì các interface của Router sẽ sinh ra địa chỉ Link-Local EUI-64
	
	Static Route:
	Router(config)#ipv6 route <destination IPv6> /subnet "ipv6-next-hop"
	
	Dynamic Route:
	R1(config)#ipv6 router ospf 1
	R1(config-router)#router-id 1.1.1.1
	
	R1(config)#int g0/0
	R1(config-if)#ipv6 ospf 1 area 0
	
	R2(config)#ipv6 router ospf 1
	R2(config-router)#router-id 2.2.2.2
	
	R2(config)#int g0/0
	R2(config-if)#ipv6 ospf 1 area 0
	
6. VPN (GRE Tunnel):
	- Kết nối môi trường Private giữa các site thông qua môi trường internet bằng cách tạo Tunnel
	1 inteface tunnel bao gồm:
	- Tunnel Source
	- Tunnel Destination
	- IP của Tunnel
	
	--> khác interface bình thường ở chỗ là có xác định tunnel-source và tunnel-destination.
	
	R1(config)#int tunnel 12
	R1(config-if)#tunnel source 100.0.0.1 (hoặc G0/1)
	R1(config-if)#tunnel destination 200.0.0.1
	R1(config-if)#tunnel mode gre ip (default) -> không gõ cấu hình cũng được
	R1(config-if)#ip address 192.168.12.1 255.255.255.0
	
	
	R2(config)#int tunnel 12
	R2(config-if)#tunnel source 200.0.0.1 (hoặc g0/1)
	R2(config-if)#tunnel destination 100.0.0.1
	R2(config-if)#tunnel mode gre ip (default) -> không gõ cấu hình cũng được
	R2(config-if)#ip address 192.168.12.2 255.255.255.0





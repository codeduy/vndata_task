# Cấu hình Network - Firewall cơ bản cho Proxmox - VM

## Cấu hình bridge Network cho VM ra Internet 
Ở phần này ta cần gán NIC **vmbr0** đã tạo ở ![Cấu hình Network](https://github.com/codeduy/vndata_task/blob/main/Proxmox/NodeSetup.md#c%E1%BA%A5u-h%C3%ACnh-network) vào VM theo các bước sau:
* Click chọn phần **Hardware** --> **Network Device (net0)** --> **Edit**
  ![](../Proxmox/images/Network_Basic/Edit_NetworkDevice.png)
* Chọn bridge **vmbr0** tại phần **Bridge** và chọn **OK**
  ![](../Proxmox/images/Network_Basic/Bridge_SelectStep.png)

## Cấu hình network private giữa các VM
### Tạo linux bridge vmbr1 gắn dải ip private - cấp phát cho các VM
* Truy cập vào phần **Network** của node **proxmox** để bắt đầu tạo linux bridge
  ![](../Proxmox/images/Network_Basic/Createvmbr1.png)
* Điền thông tin của **vmbr1** và bấm **Create** để tạo linux bridge **vmbr1**
  ![](../Proxmox/images/Network_Basic/Savevmbr1.png)
  ![](../Proxmox/images/Network_Basic/vmbr1_view.png)
* Tiếp theo, ta cần gắn NIC vmbr1 vào VM với các bước tương tự như ở phần ![Cấu hình bridge Network cho VM ra Internet](https://github.com/codeduy/vndata_task/blob/main/Proxmox/Basic_Network.md#c%E1%BA%A5u-h%C3%ACnh-bridge-network-cho-vm-ra-internet)
  ![](../Proxmox/images/Network_Basic/NetworkDevice_vmbr1.png)
  ![](../Proxmox/images/Network_Basic/vmbr1attach.png)
  ![](../Proxmox/images/Network_Basic/vmbr1_vm_view.png)
* Cấu hình thủ công file cấu hình mạng trong VM để có thể giao tiếp nội bộ bên trong NIC vmbr1
  ![](../Proxmox/images/Network_Basic/netplan_vmbr1.png)

### Tạo SDN để cấp phát IP Private cho các VM
* Đầu tiên, ta cần vào phần **SDN** trong mục **Datacenter** để thêm **"Simple" Zone**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Addzone.png)
* Sau khi click chọn **Simple** thì sẽ hiện một form dialog như hình để điền các thông tin cần thiết. Sau khi điền xong thì bấm **Add**
  ![](../Proxmox/images/Network_Basic/SDN/Addzone001.png)
  ![](../Proxmox/images/Network_Basic/SDN/zone001_view.png)
* Tiếp đến, ta sẽ tạo **Vnet001** và subnet tương ứng cho **Zone001**. Đầu tiên, click **VNets** và chọn **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Vnet_Create.png)
* Sau đó, sẽ hiện một form dialog để điền thông tin của Vnet. Ta sẽ điền tên Vnet vào trường **Name**, và chọn zone tương ứng trong **Zone** và bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Vnet001_Create.png)
  ![](../Proxmox/images/Network_Basic/SDN/Vnet001_View.png)
* Tiếp tục, ta sẽ tạo subnet cho **Vnet001**; click chọn **Vnet001** và bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Createsubnet_vnet.png)
* Và sẽ hiện dialog form để điền thông tin của subnet. Sau khi điền xong thì bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/inputsubnet_vnet001.png)
  ![](../Proxmox/images/Network_Basic/SDN/vnet_subnet_view.png)
* Click **SDN** và bấm **Apply** để áp dụng cấu hình của **Zone001**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Apply.png)
* Click **Yes**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Yes.png)
  ![](../Proxmox/images/Network_Basic/SDN/Zone001_Available.png)
* Để gán ip private của subnet mới tạo trong **SDN (Zone001 - Vnet001)** thì ta thực hiện tương tự trong phần hướng dẫn ![Cấu hình bridge Network cho VM ra Internet](https://github.com/codeduy/vndata_task/blob/main/Proxmox/Basic_Network.md#c%E1%BA%A5u-h%C3%ACnh-bridge-network-cho-vm-ra-internet)
  ![](../Proxmox/images/Network_Basic/SDN/SDN_VMAttach.png)
  ![](../Proxmox/images/Network_Basic/SDN/SDN_VM_Vnet001Apply.png)
* Cấu hình thủ công file cấu hình mạng trong VM để có thể giao tiếp nội bộ bên trong subnet tương ứng của NIC **Vnet001**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_VM_View.png)
> Các cấu hình của SDN được lưu ở phía node tại đường dẫn ```/etc/pve/sdn```

### Cấu hình NAT cho các NIC - Linux bridge
* Để NAT cho NIC **vmbr1** đã tạo ở phần hướng dẫn trước, ta cần truy cập vào **Shell** của **proxmox** host hoặc qua trình SSH để cấu hình NAT tại ```/etc/network/interfaces```
  ![](../Proxmox/images/Network_Basic/NAT/proxmoxShell_NAT.png)
  ![](../Proxmox/images/Network_Basic/NAT/termius_SSH.png)
* Thêm các dòng như ảnh để có thể NAT **vmbr0** quan **vmbr1**. Tham khảo thêm tại [Proxmox Docs](https://pve.proxmox.com/wiki/Network_Configuration#sysadmin_network_masquerading)
  ![](../Proxmox/images/Network_Basic/NAT/vmbr1_NAT.png)
  > * Các dòng ở (1) là cấu hình cho phép kernel forward gói tin từ NIC **vmbr1** và bật SNAT khi up; khi **vmbr1** down thì sẽ xóa rule SNAT này. 
  > * Các dòng ở (2) đảm bảo đặt conntrack của request vào đúng conntrack zone bởi trong một số trường hợp khi đặt firewall cho riêng VM thì firewall của VM đó có thể thay đổi conntrack zone của request gây ảnh hưởng đến NAT - mapping request đúng theo ip private <--> ip public tương ứng, và tự động thêm/xóa rule khi NIC up/down.
  > ![](../Proxmox/images/Network_Basic/NAT/NAT_VMFirewall.png)
  > * Câu hỏi: Các dạng/loại kết nối nào cần thiết phải thêm conntrack? -> Tùy các tác nhân gây ảnh hưởng lên request làm sai thông số conntrack zone mà ta cần định nghĩa lại rule PREROUTING ở bảng raw để đặt request lại cho đúng conntrack zone

### Cấu hình NAT cho các subnet trong SDN
* Đầu tiên, ta cần mở form dialog - Edit subnet theo các bước như hình
  ![](../Proxmox/images/Network_Basic/NAT/NAT_SDN_SubnetEdit.png)
* Tiếp theo, ta tick chọn **SNAT** và click **OK** để lưu cấu hình
  ![](../Proxmox/images/Network_Basic/NAT/NAT_SDN_SNATTick.png)
* Click **SDN** và chọn **Apply**
  ![](../Proxmox/images/Network_Basic/NAT/NAT_SDN_Apply.png)
* Chọn **Yes**
  ![](../Proxmox/images/Network_Basic/NAT/NAT_SDNApply.png)
* Gán ip của subnet trong SDN vào VM và kiểm tra
  ![](../Proxmox/images/Network_Basic/NAT/NAT_SDN_Success.png)
  > * Lưu ý: Khi bỏ tick **SNAT** của subnet trong SDN để tắt **SNAT** cho subnet đó thì proxmox sẽ không loại bỏ rule tương ứng ở iptables; mặc dù khi tick chọn để bật **SNAT** thì vẫn chèn rule bình thường. Vì thế dễ gây lặp lại rule như hình dưới.
  ![](../Proxmox/images/Network_Basic/NAT/NAT_DuplicateRule.png)
  > * Vì vậy, để tắt **SNAT** triệt để cho subnet - SDN thì trước tiên cần bỏ tick **SNAT** và thực thi lệnh ```iptables -t nat -F``` -        xóa hết rules của bảng nat và chạy lại lệnh ```ifreload -a``` - reload lại cấu hình mạng của proxmox host
  ![](../Proxmox/images/Network_Basic/NAT/NAT_ReloadNetworkConfig.png)

### Cấu hình pve, phpipam để cấp phát IP tự động cho VM
> Coming soon

## Cơ bản về Firewall trong Proxmox
### Sơ lược
* Luồng hoạt động - độ ưu tiên được thể hiện trong hình dưới đây:
  ![](../Proxmox/images/Network_Basic/Firewall/Firewall_Flowchart.png)
* Các firewall **Options** tại **Datacenter**, **proxmox** node, VM
  * Datacenter
    * **Firewall(Yes/No):** Bật/Tắt Firewall Layer 3/4 (iptables) - Kiểm soát traffic TCP/UDP/ICMP/... cho Datacenter, Node, và VM.
    * **ebtables(Yes/No):** Bật/Tắt Firewall Layer 2 của Proxmox (ebtables) - bao gồm MAC filter, DHCP filter, NDP filter,...
    * **Log rate limit:** Giới hạn tốc độ log firewall - Mặc định: 5 logs ngay lập tức + 1 log mỗi giây.
    <table>
      <tr>
          <td></td>
          <td>ACCEPT</td>
          <td>DROP</td>
          <td>REJECT</td>
      </tr>
      <tr>
          <td>Input Policy</td>
          <td>Mặc định <b>chấp nhận</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các DROP/REJECT rule </td>
          <td>Mặc định <b>từ chối</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các ACCEPT rule </td>
          <td>Mặc định <b>từ chối</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các ACCEPT rule, nhưng sẽ có <b>phản hồi</b> </td>
      </tr>
      <tr>
          <td>Output Policy</td>
          <td>Mặc định <b>chấp nhận</b> toàn bộ request gửi từ bên <b>trong host ra ngoài</b> trừ các DROP/REJECT rule </td>
          <td>Mặc định <b>từ chối</b> toàn bộ request từ bên <b>trong host ra ngoài</b> trừ các ACCEPT rule </td>
          <td>Mặc định <b>từ chối</b> toàn bộ request từ bên <b>trong host ra ngoài</b> trừ các ACCEPT rule, nhưng sẽ có <b>phản hồi</b>  </td>
      </tr>
      <tr>
          <td>Forward Policy</td>
          <td>Mặc định <b>chấp nhận</b> toàn bộ request <b>chuyển tiếp</b> trừ các DROP/REJECT rule </td>
          <td>Mặc định <b>từ chối</b> toàn bộ request <b>chuyển tiếp</b> qua host trừ các ACCEPT rule </td>
          <td></td>
      </tr>
    </table>  
    
    ![](../Proxmox/images/Network_Basic/Firewall/Firewall_Datacenter.png)
  * Node
    * **Firewall(Yes/No):** Bật tắt firewall L3/L4 của node (iptables)
    * **SMURFS filter(Yes/No):** Chặn tấn công Smurf attack (ICMP broadcast).
    * **TCP flags filter(Yes/No):** Chặn các packet TCP có flag bất thường (malformed TCP).
    * **NDP(Yes/No):** DHCP filter + ARP filter nhưng cho IPv6.
    * **nf_conntrack_max(Default/custom):** Số lượng entry connection tracking tối đa cho node (nếu node có nhiều VM thì cần tăng lên   để tránh quá tải).
    * **nf_conntrack_tcp_timeout_established (Default / custom):** Timeout cho TCP connections ở trạng thái ESTABLISHED.
    * **log_level_in, log_level_out, log_level_forward, tcp_flags_log_level, smurf_log_level:** cấp độ lưu log lần lượt cho traffic in/out/forward/gói TCP có flags bất thường/gói bị Smurf filter drop (tùy mỗi level mà thể hiện độ chi tiết của log).
    * **nftables(tech preview) - Yes/No:** Bật backend nftables thay cho iptables-legacy (iptables-legacy đóng vai trò là wrapper convert qua nftables vì thế nên vẫn thực thi các lệnh iptables bình thường - không cần thiết phải thực thi các lệnh nftables thuần túy).
    ![](../Proxmox/images/Network_Basic/Firewall/Firewall_Node.png)
  * VM
    * **Firewall:** Bật/tắt firewall cho VM.
    * **DHCP:** Cho phép VM gửi/nhận gói DHCP Discover/Offer/Request/Ack - nhận ip được router ảo cấp phát tự động.
    * **NDP:** DHCP filter + ARP filter nhưng cho IPv6.
    * **Router Advertisement:** Bật/tắt nhận/gửi IPv6 Router Advertisement.
    * **MAC Filter:** Bật tính năng này để buộc VM dùng MAC do proxmox cung cấp; nếu user tự đổi MAC thì sẽ drop traffic của NIC tương ứng trong VM đó.
    * **IP Filter:** Nếu bật tính năng này thì VM chỉ được dùng IP do proxmox cung cấp (qua DHCP,...); nếu tắt thì ngược lại - có thể tự chỉnh IP thủ công, miễn đúng route phù hợp mục đích của VM.
    * **log_level_in, log_level_out:** Điều khiển mức độ thông tin log khi có rule match.
      <table>
        <tr>
            <td></td>
            <td>ACCEPT</td>
            <td>DROP</td>
            <td>REJECT</td>
        </tr>
        <tr>
            <td>Input Policy</td>
            <td>Mặc định <b>chấp nhận</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các DROP/REJECT rule </td>
            <td>Mặc định <b>từ chối</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các ACCEPT rule </td>
            <td>Mặc định <b>từ chối</b> toàn bộ request <b>từ bên ngoài vào host</b> trừ các ACCEPT rule, nhưng sẽ có <b>phản hồi</b> </td>
        </tr>
        <tr>
            <td>Output Policy</td>
            <td>Mặc định <b>chấp nhận</b> toàn bộ request gửi từ bên <b>trong host ra ngoài</b> trừ các DROP/REJECT rule </td>
            <td>Mặc định <b>từ chối</b> toàn bộ request từ bên <b>trong host ra ngoài</b> trừ các ACCEPT rule </td>
            <td>Mặc định <b>từ chối</b> toàn bộ request từ bên <b>trong host ra ngoài</b> trừ các ACCEPT rule, nhưng sẽ có <b>phản hồi</b>  </td>
        </tr>
      </table>  

      ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM.png)    
* Các rule trên firewall khi được thực thi sẽ chèn rule vào bảng filter trong iptables với dạng **custom chain**, **custom chain jump**
  ![](../Proxmox/images/Network_Basic/Firewall/Firewall_chain.png)
  > Điều kiện tiên quyết để VM Firewall hoạt động thì cần phải bật Firewall ở Datacenter, Proxmox Node để chèn các custom        chain trên.
* Một ví dụ về custom chain jump
  ![](../Proxmox/images/Network_Basic/Firewall/Firewall_CustomchainJump_Input.png)
  ![](../Proxmox/images/Network_Basic/Firewall/Firewall_CustomChainJump_PVEFW-HOST-IN.png)
* Nếu có nhiều rule thì firewall sẽ thực thi các rule theo quy tắc ưu tiên thứ tự từ trên xuống dưới, và phải để **ACCEPT** rule trước; **DROP/REJECT** sau; vì nếu để rule **DROP/REJECT** ở thứ tự trước thì sẽ ghi đè rule **ACCEPT** sau nếu có xung đột phạm vi áp dụng rule.
  > Ví dụ: **Input/Output/Forward Policy: ACCEPT** ở **Datacenter**, VM; sắp xếp các rule **ACCEPT** ping icmp, **DROP** in/out
  > * Tại **Datacenter** Firewall
  >   ![](../Proxmox/images/Network_Basic/Firewall/Datacenter_FW_View.png)
  >   ![](../Proxmox/images/Network_Basic/Firewall/Datacenter_FWOptions_View.png)
  > * Tại **Node** Firewall
  >   ![](../Proxmox/images/Network_Basic/Firewall/Firewall_Node_View.png)
  >   ![](../Proxmox/images/Network_Basic/Firewall/Firewall_NodeOptions_View.png)
  > * Trường hợp 1: rule ACCEPT ở thứ tự trên/trước so với các rule DROP thỏa quy tắc trên  
  >   * Tại VM Firewall
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_Case1View.png)
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_Case1_2_OptionsView.png)
  >   * Tại VM Console: ở đây ta có thể thấy là rule ACCEPT vẫn có hiệu lực.
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_ConsoleCase1View.png)
  > * Trường hợp 2: rule ACCEPT ở thứ tự dưới/sau so với các rule DROP - sai quy tắc trên
  >   * Tại VM Firewall
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_Case2View.png)
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_Case1_2_OptionsView.png)
  >   * Tại VM Console: rule ACCEPT không còn hiệu lực nữa do bị ghi đè phạm vi rule bởi các rule DROP ở mức độ ưu tiên cao hơn
  >     ![](../Proxmox/images/Network_Basic/Firewall/Firewall_VM_ConsoleCase2_View.png)
  
  


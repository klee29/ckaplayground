--- 
### Networking Prerequisite Lecture 
- Switching: connect two system via physical or logical 
             ip link -> ip addr add ' ' ' ' 
- Router: connect two seperate network 
          route - > ip route add ' ' via ' '
EX) A -> B -> C, how about A to C? is it possible to reach? 
A a -> a B b -> a C, lower case is ethernet, Setting A with B b to C  + ip_forwading ' cat /proc/sys/net/ipv4/ip_forward 
- DNS: etc/hosts | we can set up multiple value such as google... 
    - If we have large number of entries, it is really hard to manage -> use DNS server (Name server) 'cat /etc/resolv.conf' 
    - 8.8.8.8: very common nameserver provides by Google 
    - resolve domain name? includes subdomain name? add search at '/etc/resolv.conf' 
    - Test DNS resolution? **nslookup**, **dig** includes more detail
- Network Namespace: Host(House), Room within that house, and each room doesn't know what they do inside their room. 
Each pod has its own virtual network interface.
  - Command: check network interface 'ip link' -> how about pods' internface? 'ip netns exec red lp link' = 'ip -n red link' 
    same as arp or route table 
  - Connect?: 
    1) create each pods' virtual ethernet interface with connection -> ip link add veth-red type veth peer name veth-blue 
    2) set veth up to each pods -> ip link set veth-red netns red
                                -> ip link set veth-blue netns blue
    3) assing ip -> ip -n red addr add 192.168.15.1 dev veth-red 
                 -> ip -n blue addr add 192.168.15.2 dev veth-blue 
    4) set up up to each veth -> ip -n red link set veth-red up 
                              -> ip -n blue link set veth-blue up 
    After this, host doesn't know what they created 
  - Linux Bridge(Virtual switch): ip link add **v-net-0** type bridge 
    - Connect bridge network: ip link add veth-red type veth peer name veth-red-br (blue also same sa red)
                              ip link set veth-red netns red 
                              ip link set veth-red-br master v-net-0 
                              (follow same procedure)
  - Communicate external network?: Now, we estabilished inside networks between them, but how they communicate with eth0 with sepcific pod's veth? ex red to eth0 or eth0 to red? USE **ROUTING TABLE**!
    - iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE

- Docker Networking?: 
  - NONE: without being part each other 
  - HOST: available on host 
  - BRIDGE: some device connects to this bridge network (docker network ls), in ip link (check docker0)
- Container Networking Interface(CNI): 
  - Network Namespace Challenger: use bridge program for whole those steps below  
    - Network namesapce
    - Bridge network/interface
    - VETH pairs (pipe, virtual cable)
    - attach veth to namespace
    - attach otehr veth to bridge 
    - assgin ip addresses
    - enable NAT -IP masquarade 
  - Standard? CNI: define set of responsibilities
    - Support Plug-in: Bridge, Vlan, Ipvlan, Macvlan, Windows, DHCP, host-local -> flannel, cilium, vmware nsx, weaveworks
    - Docker has its own CNI -> Container network model (CNM)
- Cluster Networking: 

- Pod Networking: connet node together at first, need to use network solution not included in kubernete (flannel, cilium, nsx ...)
  - set node network (gateway) -> create bridge network (v-net-0) -> pod has their own network

- CNI in Kubernetes: CNI congirue in kubelet service file 
  - ls /opt/cni/bin: check all cni plug in 
- WeaveWorks: weave cni plugin 
  - How it works? how we integrate our script with this plug in?
    - place their agent(plug in) to each site
### Network
---
---


#### Bridge show
---

Modern syntax to display bridges would be:
	
	ip link show type bridge

or looking for/by bridge ports:

	bridge link show
	bridge link show dev <interface>

To show only the ports of a given bridge requires again the ip link command:

	ip link show master <bridgename>


#### Create Two veth and attach them to the bridge
---

	sudo ip link add veth0 type veth peer name veth0p
	sudo ip link add veth1 type veth peer name veth1p
	sudo brctl addbr br0
	sudo brctl addif br0 veth0p
	sudo brctl addif br0 veth1p


#### Set links up
---

	sudo ip link set veth0 up
	sudo ip link set veth1 up
	sudo ip link set veth0p up
	sudo ip link set veth1p up
	sudo ip link set br0 up


#### Give each veth an IP address
---

	sudo ip addr add 10.0.0.1/24 dev veth0
	sudo ip addr add 10.0.0.2/24 dev veth1


#### Try to ping one from the other
---

	ping 10.0.0.1 -I veth1


#### Example
---

ip netns add system1
ip netns add system2

ip link set dev veth0 netns system1
ip link set dev veth1 netns system2

ip -n system1 link set lo up # this is optional for this problem
ip -n system2 link set lo up # this is optional for this problem
ip -n system1 address add 10.0.0.1/24 dev veth0
ip -n system2 address add 10.0.0.2/24 dev veth1
ip -n system1 link set dev veth0 up
ip -n system2 link set dev veth1 up

https://aly.arriqaaq.com/linux-networking-bridge-iptables-and-docker/


#### Scripted
---

	#!/usr/bin/env bash
	NS1="ns1"
	VETH1="veth1"
	VPEER1="vpeer1"
	
	NS2="ns2"
	VETH2="veth2"
	VPEER2="vpeer2"

##### create namespace
	ip netns add $NS1
	ip netns add $NS2

##### create veth link
	ip link add ${VETH1} type veth peer name ${VPEER1}
	ip link add ${VETH2} type veth peer name ${VPEER2}

##### setup veth link
	ip link set ${VETH1} up
	ip link set ${VETH2} up

##### add peers to ns
	ip link set ${VPEER1} netns ${NS1}
	ip link set ${VPEER2} netns ${NS2}

##### setup loopback interface
	ip netns exec ${NS1} ip link set lo up
	ip netns exec ${NS2} ip link set lo up

##### setup peer ns interface
	ip netns exec ${NS1} ip link set ${VPEER1} up
	ip netns exec ${NS2} ip link set ${VPEER2} up

##### assign ip address to ns interfaces
	VPEER_ADDR1="10.10.0.10"
	VPEER_ADDR2="10.10.0.20"
	
	ip netns exec ${NS1} ip addr add ${VPEER_ADDR1}/16 dev ${VPEER1}
	ip netns exec ${NS2} ip addr add ${VPEER_ADDR2}/16 dev ${VPEER2}


##### Build Bridges, Not Walls
	BR_ADDR="10.10.0.1"
	BR_DEV="br0"

##### setup bridge
	ip link add ${BR_DEV} type bridge
	ip link set ${BR_DEV} up

##### assign veth pairs to bridge
	ip link set ${VETH1} master ${BR_DEV}
	ip link set ${VETH2} master ${BR_DEV}

##### setup bridge ip
	ip addr add ${BR_ADDR}/16 dev ${BR_DEV}

##### add default routes for ns
	ip netns exec ${NS1} ip route add default via ${BR_ADDR}
	ip netns exec ${NS2} ip route add default via ${BR_ADDR}

##### Ping
	ip netns exec ${NS1} ping ${VPEER_ADDR2}
	PING 10.10.0.20 (10.10.0.20) 56(84) bytes of data.
	64 bytes from 10.10.0.20: icmp_seq=1 ttl=64 time=0.045 ms
	64 bytes from 10.10.0.20: icmp_seq=2 ttl=64 time=0.039 ms

	ip netns exec ${NS2} ping ${VPEER_ADDR1}
	PING 10.10.0.10 (10.10.0.10) 56(84) bytes of data.
	64 bytes from 10.10.0.10: icmp_seq=1 ttl=64 time=0.045 ms
	64 bytes from 10.10.0.10: icmp_seq=2 ttl=64 time=0.039 ms



#### Masquerade
---

##### enable ip forwarding
	bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
	iptables -t nat -A POSTROUTING -s ${BR_ADDR}/16 ! -o ${BR_DEV} -j MASQUERADE

MASQUERADE modifies the source address of the packet, replacing it with the address of a specified network interface. This is similar to SNAT, except that it does not require the machine's IP address to be known in advance.

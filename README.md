# proxmox-network-interfaces-for-public-network
Proxmox interfaces open to external internet that can be used for Opnsense
## File location: /etc/network 
## File name: interfaces

	# network interface settings; autogenerated
	# Please do NOT modify this file directly, unless you know what
	# you're doing.
	#
	# If you want to manage parts of the network configuration manually,
	# please utilize the 'source' or 'source-directory' directives to do
	# so.
	# PVE will preserve these directives, but will NOT read its network
	# configuration from sourced files, so do not attempt to move any of
	# the PVE managed interfaces into external files!

	auto lo
	iface lo inet loopback

	auto *NETWORK DEVICE NAME*
	iface ens18 inet static
	address *YOUR EXT IP WTIH CIDR*
	gateway *YOUR EXT IP GATEWAY*
	up route add -net *YOUR EXT IP GATEWAT -1* netmask *YOUR EXT IP NETMASK* gw "YOUR EXT IP GATEWAY* dev ens18
	# route add *YOUR EXT IP GATEWAY WITH CIDR* via *YOUR EXT IP*

	auto vmbr99
	iface vmbr99 inet static
	address 10.10.10.1/24
	bridge-ports none
	bridge-stp off
	bridge-fd 0


		post-up   echo 1 > /proc/sys/net/ipv4/ip_forward
		post-up   iptables -t nat -A POSTROUTING -s '10.10.10.0/24' -o *NETWAORK DEVICE NAME* -j MASQUERADE
		post-down iptables -t nat -D POSTROUTING -s '10.10.10.0/24' -o *NETWAORK DEVICE NAME* -j MASQUERADE 
		post-up   iptables -t raw -I PREROUTING -i fwbr+ -j CT --zone 1  
		post-down iptables -t raw -D PREROUTING -i fwbr+ -j CT --zone 1 
		
		
## User Manual

***NETWORK DEVICE NAME** example **ens18** --- delete all ( * )

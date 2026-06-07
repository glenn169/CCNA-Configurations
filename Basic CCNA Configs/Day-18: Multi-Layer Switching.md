<img width="733" height="419" alt="Screenshot 2026-06-06 123715" src="https://github.com/user-attachments/assets/41a0ef38-a139-455f-9b25-a5c75bbf0f55" />


All devices are preconfigured from the end of Day 17's lab (SW2 has been replaced with a multilayer switch):
Hosts are in the correct VLANs.
SW1 and SW2 are connected via a trunk.
R1 and SW2 are connected via ROAS.

1. Replace the ROAS configuration on R1 and SW2 with a point-to-point Layer 3 connection.
Use the IP addresses shown in the network diagram.
Configure a default route on SW2, with R1's G0/0 interface as the next hop.
2. Configure SVIs on SW2, one for each VLAN.
Assign the last usable IP address in each subnet to the appropriate SVI.
3. Test inter-VLAN connectivity by pinging between VLANs.
4. Test connectivity to the Internet by pinging 1.1.1.1.
(Routes have already been configured on R1 and the Internet router.)
- [ ]  configure the network as the Day-17 n/w lab
- [ ]  start from R1

```jsx
// remove all the subinterfaces if they are enabled
R1 (config)# no interface g0/0.10
R1 (config)# no interface g0/0.20
R1 (config)# no interface g0/0.30

// check config using 
R1 (config)# do show running-config 

//check the status of the interface
R1 (config)# do show ip interface brief
```

- [ ]  configure the interface G0/0 in R1

```jsx
R1 (config)# interface G0/0 
R1 (config-if)# ip address 10.0.0.194 255.255.255.252
```

- [ ]  configure SW2 (multi-layer switch)

```jsx
// make the default config on the switch interface
SW2 (config)# default interface g1/0/2

//reconfigure it 
SW2 (config)# interface g1/0/2
SW2 (config-if)# ip ?  //if you cannot see the 'address' option here then put the switch in layer 3 switch mode
SW2 (config-if)# no swithcport // puts the L2 switch into L3 switch

//now try to configure the switch address again.
SW2 (config-if)# ip address 10.0.0.193 255.255.255.252
SW2 (config)# do show ip routing // if you cannot see the results then enable the ip routing using the below command 
SW2 (config)# ip routing //enables the ip routing
SW2 (config)# do show ip routing //now it will display the routing table and it will only show the connected route
SW2 (config)# ip route 0.0.0.0 0.0.0.0 10.0.0.194 //setting up the default route: to able to connect to the internet 

// first configure the VLANs and then| 
                                     v
// configure Switch Virtual Interface(SVI) for vlan10
SW2 (config)# interface vlan 10  //this will create the SVI
SW2 (config-if)# ip address 10.0.0.62 255.255.255.192 //last usable ip of the vlan 10

//configure SVI for vlan 20
SW2 (config)# interface vlan 20
Sw2 (config-if)# ip address 10.0.0.126 255.255.255.192 //last usable ip of vlan 20

//configure SVI for vlan 30 
SW (config)# interface vlan 30
SW2 (config-if)# ip address 10.0.0.190 255.255.255.192 //last usable ip of vlan 30 

//check the interface 
SW2 (config)# show ip interface brief 
	" IT SHOULD SHOW THE VLAN 10,20 AND 30 AS UP/UP" /*if not up/up then use */ "NO shutdown"

```

- [ ]  this config should make the network work perfectly
- [ ]  check the connectivity by pinging 10.0.0.129 from PC7
- [ ]  Here the traffic should be routed by SWITCH 2 and not R1

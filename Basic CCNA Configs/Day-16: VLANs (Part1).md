<img width="765" height="449" alt="Screenshot 2026-06-06 010356" src="https://github.com/user-attachments/assets/0c2b61bd-a705-48ea-844d-ad6af6f5c44c" />



- [ ]  configure each endpoints (PCs) gateway with the last 2nd usable ip address in the VLAN network. Also, configure the IP address and the subnet on each endpoints (PCs)
- [ ]  Configure the separate interfaces in the router as much as the number of VLANs (since there is VLAN 10, VLAN 20 and VLAN 30) we need to connect the switch to routers g0/0 ,g0/1 and g0/2 and configure each interface with the ip address of gateways assigned in each VLAN

```jsx
// ROUTER
// interface G0/0 (10.0.0.62 is the gateway configured for VLAN 10)
R1 (config)# int g0/0
R1 (config-if)# ip address 10.0.0.62 255.255.255.192
R1 (config-if)# no shutdown

// interface G0/1 (10.0.0.126 is the gateway configured for VLAN 20)
R1 (config)# int g0/1
R1 (config-if)# ip address 10.0.0.126 255.255.255.192
R1 (config-if)# no shutdown

// interface G0/2 (10.0.0.190 is the gateway configured for VLAN 30)
R1 (config)# int g0/2
R1 (config-if)# ip address 10.0.0.190 255.255.255.192
R1 (config-if)# no shutdown
```

- [ ]  Configure the switch to access mode (so it can use VLAN)

```jsx
// enable 3 interface for vlan 10 at once
SW1 (config)# interface range g0/1,f3/1,f4/1
SW1 (config-if-range)# switchport mode access
SW1 (config-if-range)# switchport access vlan 10
SW1 (config-if-range)# vlan 10
SW1 (config-vlan)# ENGINEERING

// enable 3 interface for vlan 20 at once
SW1 (config)# interface range g1/1,f5/1,f6/1
SW1 (config-if-range)# switchport mode access
SW1 (config-if-range)# switchport access vlan 20
SW1 (config-if-range)# vlan 20
SW1 (config-vlan)# name HR

// enable 3 interface for vlan 30 at once
SW1 (config)# interface range g2/1,f8/1,f7/1
SW1 (config-if-range)# switchport mode access
SW1 (config-if-range)# switchport access vlan 30
SW1 (config-if-range)# vlan 30 
SW1 (config-vlan)# name SALES 
```

- [ ]  check for the correct assigned VLAN using `show vlan brief`
- [ ]  verify the config by pinging devices on different VLAN.

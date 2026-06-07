<img width="791" height="433" alt="Screenshot 2026-06-06 014614" src="https://github.com/user-attachments/assets/466fd34b-f638-479b-9482-5f3281ad82dc" />

- [ ]  Configure VLANs in SW1

```jsx
// create vlan10 in SW1 
SW1 (config)# interface range f0/1-2
SW1 (config-if-range)# switchport mode access
SW1 (config-if-range)# switchport access valn 10     //creates a vlan10

// create vlan30 in SW1
SW1 (config)# interface range f0/3-4
SW1 (config-if-range)# switchport mode access
SW1 (config-if-range)# switchport access vlan 30
```

- [ ]  configure VLANs in SW2

```jsx
// create vlan20 in SW2
SW2 (config)# interface f0/1
SW2 (config-if)# switchport mode access
SW2 (config-if)# switchport access vlan 20

// create vlan 10 in SW2
SW2 (config)# interface f0/2-3
SW2 (config-if)# switchport mode access
SW2 (config-if)# switchport access vlan 10
```

- [ ]  configure interface as a TRUNK in SW2 and SW1

```jsx
// configuring TRUNK mode on SW1

SW1 (config)# int g0/1
SW1 (config-if)# switchport mode trunk
SW1 (config-if)# switchport trunk allowed vlan 10,30
SW1 (config-if)# switchport trunk native vlan 1001    // always use unused native vlan number
  
// configuring TRUNK mode on SW2  
SW2 (config)# int g0/1
SW2 (config-if)# switchport mode trunk
SW2 (config-if)# switchport trunk allowed vlan 10,30
SW2 (config-if)# switchport trunk native vlan 1001    // always use unused native vlan number
    
```

- [ ]  when you use `do show interface trunk` and if you dont see the configured vlan number under the “ VLANS allowed and active in management domain” then create a vlan you want to add that vlan in the interface using `vlan <vlan_id>`
- [ ]  configure SW2 for communicating with R1 using router on a switch (ROAS)

```jsx
//configure the interface connected to the router
SW2 (config)# int g0/2
SW2 (config-if)# switchport mode trunk
SW2 (config-if)# switchport trunk allowed vlan 10,20,30
SW2 (config-if)# switchport trunk native vlan 1001    // always use unused native vlan number
  
```

- [ ]  configure R1 now: (sub-interface config for trunk)

```jsx
// configuring router and sub-interface g0/0.10 for vlan10

R1 (config)# interface g0/0
R1 (config-if)# no shutdown 
R1 (config-if)# interface g0/0.10
R1 (config-subif)# encapsulation dot1q 10 //assigning the vlan for the subinterface 
R1 (config-subif)# ip address 10.0.0.62 255.255.255.192 // 10.0.0.62 is the last usable ip address of the vlan10

// configuring sub-interface g0/0.20 for vlan20

R1 (config-if)# interface g0/0.20
R1 (config-subif)# encapsulation dot1q 20 //assigning the vlan for the subinterface 
R1 (config-subif)# ip address 10.0.0.126 255.255.255.192 // 10.0.0.126 is the last usable ip address of the vlan10

// configuring sub-interface g0/0.30 for vlan30

R1 (config-if)# interface g0/0.30
R1 (config-subif)# encapsulation dot1q 30 //assigning the vlan for the subinterface 
R1 (config-subif)# ip address 10.0.0.190 255.255.255.192 // 10.0.0.190 is the last usable ip address of the vlan10

```

- [ ]  to check the configuration, ping from one endpoint to the other, in different VLANs

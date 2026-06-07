<img width="789" height="330" alt="Screenshot 2026-06-05 215644" src="https://github.com/user-attachments/assets/a4f3344c-d29a-4c74-865e-905ae5186a5e" />


- [ ]  Configure the pc1 and pc2 with the mentioned ip address in the network diagram, use the gateway as the last working address or the router address connected to the pc1 and pc2 (configuring the switches is not required)
- [ ]  Now configure the static routes on each routers by calculating the static routing table

|                      Router | Destination | NEXT-HOP |
| --- | --- | --- |
|                              
                         R1 | 192.168.1.0/24 | connected |
|  | 192.168.3.0/24 | 192.168.12.2 |
| 
                         R2 | 192.168.1.0/24 | 192.168.12.1 |
|  | 192.168.3.0/24 | 192.168.13.3 |
| 
                          R3 | 192.168.1.0/24 | 192.168.13.2 |
|  | 192.168.3.0/24 | connected |
- [ ]  use the command `ip route <ip_address> <netmask> <Next_Hop>` , Now configure the interfaces G0/0 and G0/1  in R1
    
    ```jsx
    /* # interface G0/1 */
    R1 (config)# interface G0/1
    R1 (config-if)# ip address 192.168.1.254 255.255.255.0
    R1 (config-if)# no shutdown
    
    /* # interface G0/0 */
    R1 (config)# interface G0/0
    R1 (config-if)# ip address 192.168.12.1 255.255.255.0
    R1 (config-if)# no shutdown
    
    // # go to global config mode (config)# 
    R1 (config)# ip route 192.168.3.0 255.255.255.0 192.168.12.2
    
    /* check ip-routing table using */
    R1 (config)# do show ip route
    ```
    
- [ ]  configure the interface G0/0 and G0/1 of R2
    
    ```jsx
    // # interface G0/1
    R2 (config)# interface G0/1
    R2 (config-if)# ip address 192.168.13.2 255.255.255.0
    R2 (config-if)# no shutdown
    
    //# interface G0/0 
    R2 (config)# interface G0/0
    R2 (config-if)# ip address 192.168.12.2 255.255.255.0
    R2 (config-if)# no shutdown
    
    // # go to global config mode (config)# 
    R2 (config)# ip route 192.168.1.0 255.255.255.0 192.168.12.1
    R2 (config)# ip route 192.168.3.0 255.255.255.0 192.168.13.3
    
    /* check ip-routing table using */
    R2 (config)# do show ip route
    ```
    
- [ ]  configure the interface G0/0 and G0/1 of R3

```jsx
// # interface G0/1
R3 (config)# interface G0/1
R3 (config-if)# ip address 192.168.3.254 255.255.255.0
R3 (config-if)# no shutdown

//# interface G0/0 
R3 (config)# interface G0/0
R3 (config-if)# ip address 192.168.13.3 255.255.255.0
R3 (config-if)# no shutdown

// # go to global config mode (config)# 
R3 (config)# ip route 192.168.1.0 255.255.255.0 192.168.13.2

/* check ip-routing table using */
R3 (config)# do show ip route
```

- [ ]  check interface using `show ip interface brief`
- [ ]  ping from one end device to other to verify the configuration.

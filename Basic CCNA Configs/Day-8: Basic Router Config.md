<img width="1413" height="565" alt="Screenshot 2026-06-05 213401" src="https://github.com/user-attachments/assets/cb99c2a9-a144-4d1c-9afd-af2a915e65b7" />


- [ ]  configure the router 1  name as R1: `Router(config)# hostname R1`
- [ ]  check for ip interfaces:  `show ip interfaces brief`
- [ ]  configure R1 on interface G0/0, G0/1 and G0/2:

```jsx
# interface G0/0
R1(config)# interface g0/0 
R1(config-if)# ip address 15.255.255.254 255.0.0.0
R1(config-if)# no shutdown

# interface G0/1
R1(config)# interface g0/1 
R1(config-if)# ip address 182.98.255.254 255.255.0.0
R1(config-if)# no shutdown

R1(config)# interface g0/2 
R1(config-if)# ip address 201.191.20.254 255.255.255.0
R1(config-if)# no shutdown
```

  

- [ ]  check if the interfaces has been configured properly: `show ip interface brief`
- [ ]  check for connection status by pinging devices on the network.

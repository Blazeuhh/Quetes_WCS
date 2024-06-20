![Capture d'écran 2024-06-20 125812](https://github.com/Blazeuhh/Quetes_WCS/assets/156552845/64524eff-86ed-4ab7-b24a-4477e5c9388e)

## Configuration Routeur 1 :
```
Router> enable
Router# configure terminal
Router(config)# interface g0/0/0
Router(config-if)# ip address 192.168.0.251 255.255.255.0
Router(config-if)# ipv6 address fdabcd:1234:5678:192::251/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# interface g0/0/1
Router(config-if)# ip address 10.0.1.1 255.255.255.0
Router(config-if)# ipv6 address xxxxxx:xxxx:xxxx:x::x/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# ipv6 unicast-routing
Router(config)# exit
Router# end
```
## Configuration Routeur 2 :
```
Router> enable
Router# configure terminal
Router(config)# interface g0/0/0
Router(config-if)# ip address 192.168.0.252 255.255.255.0
Router(config-if)# ipv6 address fdabcd:1234:5678:192::252/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# interface g0/0/1
Router(config-if)# ip address 10.0.2.1 255.255.255.0
Router(config-if)# ipv6 address xxxxxx:xxxx:xxxx:x::x/64
Router(config-if)# ipv6 enable
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# ipv6 unicast-routing
Router(config)# exit
Router# end
```

# Networking Final Project

<p align="center">
Network Topology: <br/><br/>
<img src="https://i.imgur.com/cLWqNWh.png" height="80%" width="80%" alt="Landing page"/>
<br />

<p align="center">Subnet Design:</p>
<p align="center">40 Hosts -> 64 -> 2<sup>6</sup> -> 32-6 -> /26</p>
<p align="center"> 20 Hosts -> 32 -> 2<sup>5</sup> -> 32-5 -> /27</p>
<p align="center">20 Hosts -> 32 -> 2<sup>5</sup> -> 32-5 -> /27</p>
<p align="center">2 Hosts -> 4 -> 2<sup>2</sup> -> 32-2 -> /30</p>
<p align="center">2 Hosts -> 4 -> 2<sup>2</sup> -> 32-2 -> /30</p>
<p align="center">
<img src="https://i.imgur.com/cIgtuYO.png" height="80%" width="80%" alt="Landing page" />
<br /></p>
<p align="center">
<img src="https://i.imgur.com/JjJS5of.png" height="80%" width="80%" alt="Landing page" />
<br /></p>

<p align="center">Configurations:</p>
<p><b>Headquarters Router</b></p>
<p>Headquarters(config)#int s0/0/0<br/>
Headquarters(config-if)#ip address 172.16.1.129 255.255.255.252<br/>
Headquarters(config-if)#no shutdown<br/>
Headquarters(config-if)#clockrate 128000<br/>

Headquarters(config)#int s0/0/1<br/>
Headquarters(config-if)#ip address 172.16.1.133 255.255.255.252<br/>
Headquarters(config-if)#no shutdown<br/>
Headquarters(config-if)#clockrate 128000<br/>

Headquarters(config)#int g0/0<br/>
Headquarters(config-if)#ip address 172.16.1.1 255.255.255.192<br/>
Headquarters(config-if)#no shutdown<br/>

Headquarters(config)#int lo1<br/>
Headquarters(config-if)#ip address 150.0.222.225 255.255.255.252<br/>

Headquarters(config)#ip route 0.0.0.0 0.0.0.0 lo1<br/>

Headquarters(config)#router eigrp 1<br/>
Headquarters(config-router)#eigrp router-id 1.1.1.1<br/>
Headquarters(config-router)#network 172.16.1.129 0.0.0.3<br/>
Headquarters(config-router)#network 172.16.1.133 0.0.0.3<br/>
Headquarters(config-router)#network 172.16.1.1 0.0.0.63<br/>
Headquarters(config-router)#passive-interface g0/0<br/>
Headquarters(config-router)#exit<br/>

Headquarters(config)#int lo1<br/>
Headquarters(config-if)#ip summary-address eigrp 1 0.0.0.0 0.0.0.0<br/>
Headquarters(config-if)#exit<br/>
</p>
<p><b>Vancouver Router</b></p>
<p>Vancouver(config)#int s0/0/0<br/>
Vancouver(config-if)#ip address 172.16.1.130 255.255.255.252<br/>
Vancouver(config-if)#no shutdown<br/>

Vancouver(config)#int g0/0<br/>
Vancouver(config-if)#ip address 172.16.1.97 255.255.255.224<br/>
Vancouver(config-if)#no shutdown<br/>

Vancouver(config)#router eigrp 1<br/>
Vancouver(config-router)#eigrp router-id 2.2.2.2<br/>
Vancouver(config-router)#network 172.16.1.130 0.0.0.3<br/>
Vancouver(config-router)#network 172.16.1.97 0.0.0.31<br/>
Vancouver(config-router)#passive-interface g0/0<br/>

Vancouver(config)#ip access-list extended NO-REMOTE-HEADQUARTERS<br/>
Vancouver(config-ext-nacl)#deny tcp any 172.16.1.129 0.0.0.3 eq 22<br/>
Vancouver(config-ext-nacl)#deny tcp any 172.16.1.129 0.0.0.3 eq 23<br/>
Vancouver(config-ext-nacl)#permit ip any any <br/>

Vancouver(config)#int g0/0<br/>
Vancouver(config-if)#ip access-group NO-REMOTE-HEADQUARTERS in<br/>
</p>
<p><b>Montreal Router</b></p>
<p>Montreal(config)#int s0/0/0<br/>
Montreal(config-if)#ip address 172.16.1.134 255.255.255.252<br/>
Montreal(config-if)#no shutdown<br/>

Montreal(config)#int g0/0<br/>
Montreal(config-if)#ip address 172.16.1.65 255.255.255.224<br/>
Montreal(config-if)#no shutdown<br/>

Montreal(config)#router eigrp 1<br/>
Montreal(config-router)#eigrp router-id 3.3.3.3<br/>
Montreal(config-router)#network 172.16.1.134 0.0.0.3<br/>
Montreal(config-router)#network 172.16.1.65 0.0.0.31<br/>
Montreal(config-router)#passive-interface g0/0<br/>

Montreal(config)#ip access-list extended NO-VANCOUVER<br/>
Montreal(config-ext-nacl)#deny ip any 172.16.1.96 0.0.0.31<br/>
Montreal(config-ext-nacl)#permit ip any any<br/>

Montreal(config)#int g0/0<br/>
Montreal(config-if)#ip access-group NO-VANCOUVER in<br/>
Montreal(config-if)#exit<br/>
</p>
<p><b>SwitchA</b></p>
<p>SwitchA(config)#vlan 1<br/>
SwitchA(config-vlan)#exit<br/>
SwitchA(config)#int vlan1<br/>
SwitchA(config-if)#ip address 172.16.1.2 255.255.255.192<br/>
SwitchA(config-if)#ip default-gateway 172.16.1.1<br/>
SwitchA(config-if)#no shutdown<br/>

</p>







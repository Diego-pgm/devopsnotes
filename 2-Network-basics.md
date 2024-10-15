# Networking Basics

### Switches

- If we have two devices and we want to connect them, we connect them   to a switch, and the switch creates a network containing the two systems.

- To connect to a switch we need an interface on each host, physical or virtual, depending on the host.

- Imagine we have two devices `A` and `B` connected to a switch with the network `192.168.1.0`.

**List interfaces**:
```
ip link
```

`output`
```
eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
```

**Add IP addresses to a network**:
`A`
```
ip addr add 192.168.1.10/24 dev eth0
```

`B`
```
ip addr add 192.168.1.11/24 dev eth0
```

- Now the computers are able to comunicate through the switch.

- The switch can only enable communication within a network, which means it can receive packets from a host on the network and deliver it to other systems on the same network.

- If we have two more devices `C`: `192.168.2.10` and `D`: `192.168.2.11` connected to a switch with the network `192.168.2.0`, how do we connect this two networks?

### Router

- A `router` helps connect two networks together.

- Think of it as antoher **server with many network ports**.

- Since the router its connected to both networks it will get an `ip` from both of this networks.
  - `192.168.1.1` for network with devices `A` and `B`.
  - `192.168.2.1` for networks with devices `C` and `D`.

### Gateway

- If the network was a room, the gateway is a door to the outside world, to the other networks or to the Internet.

- The systems `need to know` where that door is, to go through it.


**Print Existing routing tables**
```
route
```

- To configure a `gateway` on system `B` to reach the systems on network 2, run the `ip route` command and specify that you can `reach` the `192.168.2.0` network through the gateway `192.168.1.1`:

- This should be done on all the network systems that we want to connect.

```
ip route add 192.168.2.0/24 via 192.168.1.1
```

```
ip route add 192.168.1.0/24 via 192.168.2.1
```

- Running the route command again should show the routing tables updated.


### Connect the router to the internet

- Instead of adding a routing table entry for every IP on the networks we can simply say for any network that you don't know a route to, use this router as the default gateway.

- This way any request to any network outside of the existing network goes to this particular router.


**Connect both networks to the internet**

- Run the command for every device you want to connect.

```
ip route add default via 192.168.2.1
```

- When we set a linux device as a router, by default, it drops any packet that flows through the system, so we need to enable the system to let packets flow, this can be done by running:
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

- This wont persist through system reboots so to make it persistant we need to change the `net.ip4.ip_forward` value in the `/etc/sysctl.conf` file.

### DNS

- It stands for `Domain Name System`, and it is a way of assigning names to hosts and the match them with their ips, this is known as `Name Resolution`.

- We can add a `DNS record` in the `/etc/hosts` file to assign the name.

```
cat >> /etc/hosts
192.168.1.11    db
```

- If we have a large set of hosts in a network and the IP of one of them changes, instead of changing every `hosts` file in every host, we can setup a `DNS Server` and point every host to read the `hosts` file from here.

- We can specify the DNS server (supposing it has ip `192.168.1.100`) adding an entry to the `/etc/resolv.conf` file like this:
```
nameserver  192.168.1.100
```

- By default to resolve the name-ip the system first looks into the local `/etc/hosts` file and then in the `DNS Server`.

- We can modify this behaviour in the `/etc/nsswitch.conf` file:
```
....
hosts:  files dns
....
```

- Change the order to `dns` `files` and it will look first in the DNS Server.

- If there is not a server named on your DNS Server (for example facebook) we can set another DNS server entry in the `/etc/resolv.conf` file, like `google's` public server with ip `8.8.8.8`.

### Domain Names

- The www.page.com format is called `domain name` and it is how IP is translate to names that we can remember on the public internet.

- The `last` portion of the domain name, .com, .net, .org, .edu, etc. are the top level domains, and represent the intent of the website .com for `commertial` or .edu for `education`.

- To resolve names without the `mycompany.com` part we need to add a new entry to the `/etc/resolv.conf` file like this:
```
search  mycompany.com prod.mycompany.com
```

- We can find information about servers with the commands `nslookup` and `dig`.
# ✅ **IP Slicing**

**IP Slicing = Dividing an IP network into smaller “slices” based on ranges or subnets**, usually for:

* Load balancing
* Traffic isolation
* Security
* Multi-tenant systems
* Routing efficiency

Think of it as **cutting a big network block into many small logical networks**.

### **Example**

- 2 1 2
- 2 2 4
- 2 3 8
- 2 4 16
- 2 5 32
- 2 6 64
- 2 7 128
- 2 8 256
- 2 9 512
- 2 10 1024
- 2 11 2048
- 2 12 4096
- 2 13 8192
- 2 14 16384
- 2 15 32768
- 2 16 65536

You have: `10.0.0.0/16` (65,536 IPs)

You slice it into:

* Slice 1 → `10.0.0.0/20` (4,096 IPs) → For production
* Slice 2 → `10.0.16.0/20` → For staging
* Slice 3 → `10.0.32.0/21` → For dev
* Slice 4 → `10.0.40.0/22` → For analytics

Each slice has different routing rules, firewall policies, or owners.

---

# ✅ **IP Multiplexing**

**IP Multiplexing = Sending multiple independent data streams over one IP connection or one network interface.**

Think of it as **combining multiple flows** over one IP path.

### **Two common forms**:

### **1. Port-based multiplexing (most common)**

One IP address, many ports:

```
192.168.1.10:80  → HTTP
192.168.1.10:443 → HTTPS
192.168.1.10:1883 → MQTT
192.168.1.10:22 → SSH
```

This is what lets a single server handle thousands of clients.

**WebSocket example:**

Even if WebSockets run on port 443,
**each client is uniquely identified by its (clientIP:clientPort → serverIP:443)** pair.

### **2. Protocol multiplexing**

Multiple application protocols over one connection.

Examples:

* HTTP/2 and HTTP/3 multiplex multiple streams on one TCP/QUIC connection
* MPTCP (Multipath TCP) multiplexes many sub-flows
* VPN tunnels multiplex many IP packets inside encrypted tunnel

### **3. IP-in-IP multiplexing (tunneling)**

GRE, IPsec, VXLAN, WireGuard → all carry multiple inner packets over one outer IP.

---

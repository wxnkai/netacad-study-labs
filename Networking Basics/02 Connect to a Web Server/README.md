# Connect to a Web Server

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how IP packets are routed across a network to reach a web server. This exercise covers basic connectivity verification and HTTP communication using a simulated client-server environment.

## 🎯 Objective

- Observe how packets travel across a network using IP addressing by connecting a client PC to a remote web server.

## 📋 Lab Steps

### Part 1 – Verify Network Connectivity

1. Open **PC0** in the Packet Tracer workspace.
2. Navigate to **Desktop** → **Command Prompt**.
3. Ping the web server to confirm reachability:

   ```
   ping 172.33.100.50
   ```

4. Confirm successful replies — this verifies layer 3 (IP) connectivity between the client and server.

   > **Note:** The first ping may time out while ARP resolves the MAC address. Subsequent replies should succeed.

5. Close the Command Prompt window, keeping the PC0 configuration window open.

---

### Part 2 – Access the Web Server via Browser

1. From the **Desktop** tab on PC0, open **Web Browser**.
2. Enter the server's IP address in the URL bar:

   ```
   172.33.100.50
   ```

3. Click **Go** — the browser connects to the web server over HTTP and loads the hosted web page.
   **Expected result:** A welcome page loads, confirming end-to-end connectivity and that both a web client and web server are functioning correctly.

## 🔑 Key Learnings

### IP Addresses

IP addresses are the foundation of network communication. Every device on a network needs a **unique IP address** to send and receive data. In this lab, reaching the web server was only possible because the client already knew the server's IP (172.33.100.50). Without that address, there is no way to direct traffic to the correct destination, which is exactly why DNS exists in real-world networks to translate human-readable domain names into IPs automatically.

### ICMP Ping

Ping is a first-responder diagnostic tool. Before attempting any application-level connection, using ping to test ICMP reachability is best practice. A successful ping tells you that the path between two hosts is intact at Layer 3 (the Network layer), meaning routers are forwarding packets correctly. A failed ping, on the other hand, immediately narrows your troubleshooting scope to the network or transport layer rather than the application itself.

### Address Resolution Protocol

ARP bridges the gap between Layer 2 and Layer 3. Even though IP operates at Layer 3, data still physically travels over Ethernet frames at Layer 2, which require MAC addresses, not IP addresses. ARP is what resolves an IP address to its corresponding MAC address on a local network. This is why the very first ping may time out or show packet loss: the device is pausing to broadcast an ARP request and wait for a reply before it can encapsulate the packet in an Ethernet frame and send it. Subsequent pings succeed instantly because the MAC address is now cached in the ARP table.

### HTTP

HTTP is what turns a raw IP connection into a web experience. Once Layer 3 connectivity was confirmed via ping, opening a web browser and navigating to the same IP address demonstrated the application layer at work. The browser sent an HTTP GET request to port 80 on the server, which responded with a web page.

## 📌 Points to Remember

- Always verify Layer 3 connectivity with ping before troubleshooting the application layer. Network layers depend on each other, and working from the bottom up saves time.
- First-ping packet loss is expected behavior. ARP needs time to resolve the destination MAC address before the first packet can be sent. This is normal. All four packets failing is the sign of a real problem.
- A successful ping does not guarantee a working application. The server must also have a service actively listening on the correct port (port 80 for HTTP) for a browser connection to succeed.
- Using a raw IP address in this lab reveals what DNS handles invisibly in production. Understanding the IP layer makes it easier to understand what DNS, firewalls, and HTTPS are each solving at their respective layers.

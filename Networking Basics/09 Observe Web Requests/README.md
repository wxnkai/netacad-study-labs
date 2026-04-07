# Observe Web Requests

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how a client communicates with a web server when requesting web services. The lab covers connectivity verification, DNS resolution, HTML inspection, and packet-level observation of HTTP traffic using Simulation mode.

## 🎯 Objective

- View the client/server traffic sent from a PC to a web server when requesting web services.

## 📋 Lab Steps

### Part 1 – Verify Connectivity to the Web Server

1. Click **External Client** and open the **Desktop** tab.
2. Click **Command Prompt** and run the following ping:

   ```
   ping ciscolearn.web.com
   ```

3. Note the IP address shown in the ping output. This address is returned by the DNS server and corresponds to the domain name `ciscolearn.web.com`. All traffic routed across a network relies on source and destination IP addresses, not domain names.
4. Close the Command Prompt window but leave the External Client desktop window open.

---

### Part 2 – Connect to the Web Server

1. From the **Desktop** tab on **External Client**, open **Web Browser**.
2. Type the following URL into the address bar and press **Go**:

   ```
   ciscolearn.web.com
   ```

3. Read the web page that is displayed. Leave the page open and minimize the External Client window without closing it.

---

### Part 3 – View the HTML Code

1. From the logical topology, click the **ciscolearn.web.com** server.
2. Open the **Services** tab, then click the **HTTP** tab.
3. Next to the **index.html** file, click **(edit)**.
4. Compare the raw HTML markup code on the server side to the rendered web page displayed in the External Client browser. Re-maximize the External Client window if needed to view both side by side.
5. Close both the External Client and the web server windows when finished.

---

### Part 4 – Observe Traffic Between the Client and the Web Server

#### Step 1: Enter Simulation Mode

1. Click the **Simulation** tab in the lower-right corner of Packet Tracer to switch to Simulation mode.
2. Double-click the **Simulation Panel** to detach it from the main window so the full network topology remains visible.

#### Step 2: Configure Event Filters

1. In the **Simulation Panel**, click **Edit Filters**.
2. Click the **Misc** tab and confirm that only **TCP** and **HTTP** are checked. Close the filter window.

#### Step 3: Create a Complex PDU

1. Click the **open envelope** icon above the Simulation mode icon to select the **Add Complex PDU** tool.
2. Click **External Client** in the topology to set it as the source device. The **Create Complex PDU** window will appear.

#### Step 4: Configure the Complex PDU

In the **Create Complex PDU** window, set the following:

| Setting | Value |
|---|---|
| Select Application | `HTTP` |
| Destination Device | Click `ciscolearn.web.com` in the topology |
| Starting Source Port | `1000` |
| Simulation Settings | Periodic Interval: `120` seconds |

Click **Create PDU** to confirm.

#### Step 5: Run and Observe the Simulation

1. Click **Play** in the Simulation Panel. Use the play speed slider to increase the animation speed.
2. If a **Buffer Full** message appears, click **View Previous Events** to close it and continue.
3. Scroll through the **Event List** when the animation completes.

   Observe the number of packets exchanged between the client and server. Because HTTP runs over TCP, the exchange includes connection establishment packets (the TCP three-way handshake) and acknowledgements for each data segment received, which adds significant overhead compared to the actual page content being transferred.

---

### Reflection – Key Observations from the Lab

- DNS resolution happens before any HTTP traffic is sent. The ping in Part 1 reveals the IP address that the DNS server maps to `ciscolearn.web.com`, which is the address all subsequent packets use at the network layer.
- The raw HTML on the server and the rendered page in the browser are two views of the same content. HTTP delivers the markup, and the browser interprets it into a visual display.
- TCP introduces considerable traffic overhead relative to the actual payload. Connection setup, acknowledgements, and teardown each generate their own packets, all of which appear in the Event List alongside the HTTP data itself.

## 🔑 Key Learnings

### DNS Resolves Names to IP Addresses Before Any Data Transfer

The ping command in Part 1 triggers a DNS lookup and returns the IP address of `ciscolearn.web.com`. This address is what the network actually uses to forward traffic. The domain name is a human-readable label; the IP address is the true network identifier that routers work with at every hop.

### HTTP Delivers Raw Markup That the Browser Then Renders

The index.html file on the server contains HTML tags and structure. What the browser displays is the browser's interpretation of that markup, not the file itself. Viewing the server-side source makes the distinction between content delivery and content rendering concrete.

### TCP Overhead Is Visible When Inspecting HTTP Traffic

Filtering the Event List to TCP and HTTP reveals that a single web page request generates far more packets than just the request and response. The TCP three-way handshake, per-segment acknowledgements, and connection teardown all contribute to the total packet count. This overhead is the cost of TCP's guaranteed, ordered delivery.

## 📌 Points to Remember

- `ping` resolves a hostname through DNS before sending ICMP packets, so the IP address shown in ping output is the DNS-resolved address for that domain.
- HTTP is a TCP-based protocol, so every HTTP session includes connection establishment and acknowledgement packets in addition to the actual request and response.
# The Client Interaction

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates how a PC communicates with a server to resolve a domain name and retrieve a web page. Using Simulation mode, the full DNS and HTTP exchange is captured and inspected step by step at the PDU level across the OSI model.

## 🎯 Objective

- Observe the client-server interaction that occurs when a web page is requested, including DNS resolution and HTTP delivery.

## 📋 Lab Steps

### Part 1 – Enter Simulation Mode

1. With the Packet Tracer topology open in **Realtime** mode, locate the **Simulation Mode** icon in the bottom-right corner of the logical workspace.
2. Click the icon to switch to **Simulation** mode.

---

### Part 2 – Set Event List Filters

By default, Simulation mode captures all event types. Narrow the capture to DNS and HTTP only.

1. In the **Event List Filters** section of the Simulation Panel, click **Show All/None** to clear all checked event types.
2. Click **Edit Filters**.
3. Under the **IPv4** tab, check **DNS**.
4. Under the **Misc** tab, check **HTTP**.
5. Close the filter window. The **Event List Filters** area should now show only DNS and HTTP as active.

---

### Part 3 – Request a Web Page from the PC

1. Click the **PC** in the topology and open the **Desktop** tab.
2. Click **Web Browser** to open the simulated browser.
3. Type the following URL into the address bar and click **Go**:

   ```
   www.example.com
   ```

4. Minimize the PC window.

---

### Part 4 – Run the Simulation

1. In the **Play Controls** section of the Simulation Panel, click **Play**.
2. Watch the animation as packets move between the PC and the server. Events are added to the **Event List** as they occur.

   The captured events represent the full request cycle:
   - The PC sends a DNS query to resolve `www.example.com` to an IP address.
   - The server responds with the resolved IP address.
   - The PC sends an HTTP request for the web page.
   - The server returns the web page in two segments.
   - The PC acknowledges receipt of the web page.

3. If a **Buffer Full** message appears, click **View Previous Events** to continue.

---

### Part 5 – Access a Specific PDU

1. Restore the simulated PC window and confirm the web page has loaded in the browser. Minimize the browser again.
2. In the **Simulation Panel Event List**, locate the colored box in the rightmost column of the first row.
3. Click that colored box to open the **PDU Information** window for the first event.

---

### Part 6 – Examine the PDU Information Window

1. The **PDU Information** window opens on the OSI model tab, showing the inbound and/or outbound PDU details layer by layer.
2. Click **Next Layer >>** repeatedly to step through each OSI layer. Read the description in the box below the layer diagram to understand what is happening at each stage of the exchange.
3. Close the window and repeat the process for each remaining event in the **Event List** to trace the complete DNS and HTTP exchange from start to finish.

---

### Reflection – Key Observations from the Lab

- A single web page request triggers two distinct protocols working in sequence: DNS resolves the name to an IP address first, and HTTP then uses that IP to fetch the page content.
- The server returns the web page across multiple segments, and the PC acknowledges each one. This reflects TCP's reliable delivery mechanism operating beneath HTTP.
- The PDU Information window shows how data is encapsulated and decapsulated at each OSI layer, making the separation between protocols and their responsibilities visible at each hop.

## 🔑 Key Learnings

### DNS Must Resolve Before HTTP Can Begin

When a user types a URL, the browser does not know the destination IP address. The PC first sends a DNS query to the configured DNS server, which responds with the matching IP. Only after this exchange completes can the PC send an HTTP request. This two-step process happens transparently on every web request.

### HTTP Operates Over Multiple TCP Segments

The server does not send the entire web page in a single packet. The content is broken into segments, each delivered and acknowledged individually. Simulation mode makes this visible by showing the HTTP response spread across separate events, each with its own PDU entry in the Event List.

### The OSI Model Maps Directly to Real Traffic

The PDU Information window ties abstract OSI layer concepts to actual packet data. Stepping through the layers for each event shows which protocol headers are added or removed at each stage, reinforcing how encapsulation and decapsulation work in practice.

### Filters Focus the Analysis

Capturing every event type in a busy network quickly produces too much data to analyze. Setting filters to show only DNS and HTTP keeps the Event List focused on the protocols relevant to the task, which is the same approach used when working with real packet capture tools like Wireshark.

## 📌 Points to Remember

- Switch to Simulation mode and set filters before triggering traffic, otherwise unrelated events fill the Event List and obscure the exchange being studied.
- DNS resolution always precedes HTTP in a URL-based web request. If DNS fails, the HTTP request never starts.
- The colored box in the rightmost column of each Event List row opens the PDU Information window for that specific packet.
- Use **Next Layer >>** in the PDU Information window to step through OSI layers and read the description for each one.
- A web page response arriving in multiple segments is normal TCP behavior, not an error.
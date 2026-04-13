# Use the ping Command

A hands-on networking lab completed in Cisco Packet Tracer that demonstrates a structured troubleshooting approach using `ping` and `ipconfig /all` to isolate and resolve a DNS misconfiguration on a statically addressed PC. The lab works through a layered diagnostic process: confirming application failure, testing name resolution, testing IP reachability, comparing DNS settings, and applying the fix.

## 🎯 Objective

- Use the `ping` command to identify and isolate an incorrect configuration on a PC that cannot connect to a web server.

## Background

A small office has multiple PCs configured with static IP addressing. Some users cannot reach a website. The symptom points to either a routing failure or a name resolution failure. The lab walks through each possibility in order, using `ping` to narrow the fault to its exact cause before making any changes.

## 📋 Lab Steps

### Part 1 – Verify Connectivity via Web Browser

1. On each PC, open the **Desktop** tab and click **Web Browser**.
2. In the URL bar, enter:

   ```
   www.cisco.pka
   ```

3. Allow up to one minute for all devices to complete their boot process before evaluating results.
4. Note which PCs successfully load the page and which display an error or blank response. The PCs that fail to connect are the ones to investigate in the following parts.

---

### Part 2 – Ping the Web Server by Name from the Affected PC

1. On the PC with connectivity issues, open the **Desktop** tab and click **Command Prompt**.
2. Ping the web server using its domain name:

   ```
   ping www.cisco.pka
   ```

3. Check whether replies are returned and note the IP address shown in the output. If replies are received and an IP address is shown (such as `192.15.2.10`), DNS is resolving the name correctly on this PC, and the fault lies elsewhere. If no reply is returned, continue to Part 4 to determine whether the problem is DNS or IP reachability.

---

### Part 3 – Ping the Web Server by Name from a Working PC

1. On a PC that successfully loaded the web page, open the **Desktop** tab and click **Command Prompt**.
2. Ping the same URL:

   ```
   ping www.cisco.pka
   ```

3. Record the IP address returned in the output. This IP address is the correct resolved address for the web server and will be used as a reference in Part 4.

---

### Part 4 – Ping the Web Server by IP Address from the Affected PC

1. On the PC with connectivity issues, open **Command Prompt**.
2. Ping the web server using its IP address directly (use the value recorded from Part 3):

   ```
   ping 192.15.2.10
   ```

3. Evaluate the result. If replies are received when pinging by IP but the browser fails when using the domain name, the PC can route to the server correctly but cannot resolve the hostname. This confirms the fault is DNS configuration, not routing or physical connectivity.

---

### Part 5 – Compare DNS Configuration Across PCs

1. On a working PC, open **Command Prompt** and run:

   ```
   ipconfig /all
   ```

   Record the DNS server address shown in the output.

2. On the affected PC, open **Command Prompt** and run the same command:

   ```
   ipconfig /all
   ```

3. Compare the DNS server address on both PCs. A missing, blank, or incorrect DNS server address on the affected PC confirms the root cause of the failure.

---

### Part 6 – Apply the Configuration Fix

1. On the affected PC, open the **Desktop** tab and click **IP Configuration**.
2. Update the DNS server field to match the correct value observed on the working PCs.
3. Close the IP Configuration window.
4. Open **Web Browser** from the **Desktop** tab and navigate to:

   ```
   www.cisco.pka
   ```

5. Confirm the web page loads successfully. A working browser response verifies that the DNS correction resolved the fault.

---

### Reflection – Key Observations from the Lab

- The structured sequence in this lab isolates the fault layer by layer. Browser failure alone could mean many things. A successful ping by IP but a failed ping by name narrows the problem to DNS specifically, without any guessing.
- Two PCs on the same network with identical IP addresses, masks, and gateways but different DNS server entries will have very different user experiences. One will browse normally while the other appears to have no internet at all, even though routing is fully functional on both.
- `ipconfig /all` is the right tool for comparing full host configurations because it shows the DNS server alongside the IP, mask, and gateway in a single output, making discrepancies immediately visible when read side by side.

## 🔑 Key Learnings

### ping Isolates Faults by Testing One Variable at a Time

Pinging by domain name tests both DNS resolution and IP reachability simultaneously. Pinging by IP address tests only IP reachability. Running both pings in sequence and comparing the results pinpoints whether the failure is at the name resolution layer or the network layer. This two-step ping technique is one of the most efficient first moves in any connectivity troubleshooting workflow.

### A Successful IP Ping with a Failed Name Ping Means DNS Is the Problem

If `ping 192.15.2.10` succeeds but `ping www.cisco.pka` fails or returns no address, the host is fully routed and reachable at the IP layer but cannot translate the domain name to an address. The fault is always in DNS configuration at that point: either the DNS server address is wrong, missing, or the configured server is unreachable.

### DNS Server Misconfiguration Looks Like Total Internet Failure to the User

From the user perspective, a missing or wrong DNS server address produces the same symptom as being completely disconnected from the network. Every website fails to load. Only the structured ping test reveals that the underlying IP connectivity is intact. Without this diagnostic step, it would be easy to misdiagnose a simple DNS configuration error as a hardware or routing problem.

### ipconfig /all Is the Authoritative Source of a Host's Full Network Identity

The `/all` flag surfaces fields that `ipconfig` alone omits: the DNS server address, DHCP status, MAC address, and DHCP lease information. When auditing a statically configured host, `ipconfig /all` provides everything needed to verify the configuration is complete and correct. Comparing its output between a working host and a failing host side by side is the fastest way to find the difference.

### Static Addressing Requires Every Field to Be Correct

DHCP provides the IP address, subnet mask, default gateway, and DNS server in a single transaction. Static addressing requires each of these to be entered manually and independently. Omitting or mistyping any one of them produces a specific and predictable failure. Understanding which field controls which behaviour makes it possible to match a symptom directly to its cause without trial and error.

## 📌 Points to Remember

- Start troubleshooting at the application layer by testing the browser, then work down through DNS and IP in sequence before concluding a hardware or routing fault.
- `ping <domain>` tests name resolution and routing together. `ping <ip>` tests routing only. Use both to separate DNS faults from network faults.
- A PC that can ping a server by IP but not by name has a DNS configuration error, not a connectivity problem.
- `ipconfig /all` shows the DNS server address. `ipconfig` alone does not. Always use `/all` when auditing a full host configuration.
- Compare `ipconfig /all` output between a working PC and a failing PC on the same network to locate the exact misconfigured field.
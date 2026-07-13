# CPENT Lab Notes -- Exercise 1: Scanning Against Defenses with Nmap

## Objective

Learn how firewall rules affect Nmap scans and understand the difference
between **DROP** and **REJECT**.

------------------------------------------------------------------------

## Lab Topology

-   **Attacker:** Parrot Security
-   **Target:** Ubuntu RPC Server
-   **Target IP:** `192.168.0.51`

------------------------------------------------------------------------

## Step 1 -- Verify Connectivity

Start Wireshark on Parrot:

``` bash
sudo wireshark
```

Capture on **eth0**.

Run:

``` bash
ping 192.168.0.51
```

### Observation

-   ICMP Echo Request
-   ICMP Echo Reply

The target is reachable.

------------------------------------------------------------------------

## Step 2 -- Initial Nmap Scan

Run:

``` bash
nmap -sC 192.168.0.51 -n
```

### Command Explanation

-   `nmap` -- Network scanner
-   `-sC` -- Run default NSE scripts
-   `-n` -- Disable DNS resolution

### Purpose

This is the **baseline scan** before enabling firewall filtering.

Expected: - Open ports detected - Services identified

------------------------------------------------------------------------

## Step 3 -- Enable Firewall (DROP)

On Ubuntu:

``` bash
sudo iptables -P INPUT DROP
```

Verify:

``` bash
sudo iptables -L
```

Expected:

    Chain INPUT (policy DROP)

### Meaning

-   `-P` = Set default policy
-   `INPUT` = Incoming traffic
-   `DROP` = Silently discard incoming packets

------------------------------------------------------------------------

## Step 4 -- Scan Again

Start Wireshark capture.

Run:

``` bash
nmap -sC 192.168.0.51 -n
```

### Observation

-   Nmap receives little or no response.
-   Ports may appear **filtered**.
-   Wireshark shows outgoing packets but no replies.

Reason: The firewall drops incoming packets before they reach the
services.

------------------------------------------------------------------------

## Step 5 -- Test with Ping

Run:

``` bash
ping 192.168.0.51
```

### Observation

-   Echo Requests leave Parrot.
-   No Echo Replies return.

Reason: Firewall silently drops ICMP packets.

------------------------------------------------------------------------

## Step 6 -- Remove Existing Rules

``` bash
sudo iptables -F
```

### Meaning

`-F` = Flush (remove all current firewall rules)

------------------------------------------------------------------------

## Step 7 -- Restore Default Policy

``` bash
sudo iptables -P INPUT ACCEPT
```

Meaning: Allow incoming traffic by default.

------------------------------------------------------------------------

## Step 8 -- Configure REJECT Rule

Run:

``` bash
sudo iptables -A INPUT -j REJECT --reject-with icmp-host-prohibited
```

### Command Breakdown

-   `-A` = Append rule
-   `INPUT` = Incoming traffic
-   `-j REJECT` = Reject the packet
-   `--reject-with icmp-host-prohibited` = Send ICMP "Host Prohibited"
    message

------------------------------------------------------------------------

## DROP vs REJECT

### DROP

-   Packet received by firewall
-   Packet discarded
-   No response sent
-   Appears as timeout

Flow:

    Parrot --> Firewall --> DROP
    (No reply)

### REJECT

-   Packet received by firewall
-   Packet blocked
-   Firewall sends ICMP Host Prohibited

Flow:

    Parrot --> Firewall --> REJECT
                        |
                        +--> ICMP Host Prohibited

------------------------------------------------------------------------

## Wireshark Observations

### Before Firewall

-   Echo Request
-   Echo Reply
-   TCP SYN
-   SYN-ACK

### DROP

-   Requests only
-   No replies

### REJECT

-   Requests
-   ICMP Host Prohibited response

------------------------------------------------------------------------

## Key Learning

1.  Initial scan establishes a baseline.
2.  `DROP` silently blocks incoming packets.
3.  `REJECT` blocks packets and informs the sender.
4.  Nmap results change depending on firewall behavior.
5.  Wireshark helps visualize requests and responses.

------------------------------------------------------------------------

## Commands Used

``` bash
sudo wireshark

ping 192.168.0.51

nmap -sC 192.168.0.51 -n

sudo iptables -P INPUT DROP

sudo iptables -L

sudo iptables -F

sudo iptables -P INPUT ACCEPT

sudo iptables -A INPUT -j REJECT --reject-with icmp-host-prohibited
```

------------------------------------------------------------------------

## Interview / Exam Points

-   Baseline scan = Scan before firewall changes.
-   DROP = Silent blocking, no response.
-   REJECT = Blocking with an ICMP error response.
-   Wireshark verifies whether responses are received.
-   Firewalls filter packets before they reach applications.

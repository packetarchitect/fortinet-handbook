# 📘 Chapter 5 — VDOM (Virtual Domains)

---

# 📌 Overview

VDOM (Virtual Domain) is a virtualization technology in FortiGate that allows a single physical firewall to operate as multiple independent virtual firewalls.

Think of it like:

```text
VMware ESXi
    ↓
Multiple Virtual Machines

FortiGate
    ↓
Multiple Virtual Firewalls (VDOMs)
```

Each VDOM has its own:

- Interfaces
- Routing Table
- Firewall Policies
- VPNs
- Administrators
- Security Profiles
- NAT Rules

---

# 🎯 Learning Objectives

By the end of this chapter, you should be able to:

- Understand what VDOMs are
- Understand why VDOMs are used
- Explain VDOM architecture
- Understand the Root VDOM
- Enable Multi-VDOM Mode
- Create VDOMs
- Assign interfaces to VDOMs
- Understand VDOM administration
- Understand NAT Mode and Transparent Mode VDOMs
- Understand VDOM Links
- Configure Inter-VDOM Routing
- Understand VDOM resource allocation
- Understand Management VDOMs
- Understand real-world VDOM deployments
- Troubleshoot VDOM connectivity
- Answer common VDOM interview questions

---

# 🧠 6.1 What is a VDOM?

**VDOM (Virtual Domain)** is a virtualization technology in FortiGate that allows a single physical firewall to operate as multiple independent virtual firewalls.

Think of it like:

```text
VMware ESXi
    ↓
Multiple Virtual Machines

FortiGate
    ↓
Multiple Virtual Firewalls (VDOMs)
```

Each VDOM has its own:

- Interfaces
- Routing Table
- Firewall Policies
- VPNs
- Administrators
- Security Profiles
- NAT Rules

Each VDOM operates independently from the others.

---

# 🏢 6.2 Why Use VDOMs?

Without VDOMs:

```text
One FortiGate
     │
     ├── One Routing Table
     ├── One Policy Set
     └── One Admin Domain
```

With VDOMs:

```text
FortiGate
   │
┌──┼─────────────┐
│  │             │
HR  Finance    Production
VDOM VDOM      VDOM
```

Each department becomes isolated.

---

# ⭐ 6.3 Benefits of VDOMs

## Multi-Tenancy

Used by:

- ISPs
- MSSPs
- Data Centers

One firewall serves multiple customers.

## Isolation

A problem in one VDOM does not affect another.

## Security

Separate:

- Policies
- Routing
- VPN

## Cost Saving

One appliance instead of many.

---

# 🏗️ 6.4 VDOM Architecture

```text
+--------------------------------+
|          FORTIGATE             |
+--------------------------------+
|            Root VDOM           |
+--------------------------------+
| HR | Finance | DMZ | Branch |
+--------------------------------+
```

Each VDOM behaves like an independent firewall.

---

# 🔵 6.5 Root VDOM

The **Root VDOM** is the default VDOM.

It is created automatically.

It contains:

- System Settings
- Global Configuration
- Default Interfaces

Example:

```text
root
```

---

# ⚙️ 6.6 Multi-VDOM Mode

Enable Multiple VDOMs.

### GUI

```text
System → Settings → Virtual Domains
```

### CLI

```text
config system global
set vdom-mode multi-vdom
end
```

Reboot required.

---

# ➕ 6.7 Creating a VDOM

Example: Create Finance VDOM.

```text
config vdom
edit Finance
next
end
```

Verify:

```text
show vdom
```

---

# 🔌 6.8 Assigning Interfaces to VDOM

Every interface belongs to only one VDOM.

Example:

```text
port1 → HR
port2 → Finance
port3 → Branch
```

### CLI

```text
config system interface
edit port2
set vdom Finance
next
end
```

---

# 👨‍💻 6.9 VDOM Administration

Each VDOM can have its own administrators.

Example:

```text
HR Admin
Finance Admin
Security Admin
```

### Benefits

Users see only their VDOM.

---

# 🌐 6.10 VDOM Types

## NAT Mode VDOM

Most common.

Features:

- Routing
- NAT
- Security Policies

Example:

```text
LAN
↓
VDOM
↓
Internet
```

## Transparent Mode VDOM

Acts as a Layer-2 firewall.

Features:

- No Routing
- No NAT
- Traffic Inspection

Example:

```text
Switch
↓
VDOM
↓
Router
```

---

# 🔗 6.11 VDOM Links

VDOMs are isolated.

To communicate between VDOMs:

**Use a VDOM Link.**

Example:

```text
HR VDOM
   │
VDOM Link
   │
Finance VDOM
```

A VDOM Link acts like a:

> **Virtual Cable**

between VDOMs.

---

# 🔌 6.12 Creating VDOM Link

```text
config system vdom-link
edit HR-FINANCE
next
end
```

Result:

```text
HR-FINANCE0
HR-FINANCE1
```

Two virtual interfaces are created.

---

# 🔄 6.13 Inter-VDOM Routing

Allows traffic between VDOMs.

Example:

```text
HR Network
192.168.10.0/24

Finance Network
192.168.20.0/24
```

Traffic Flow:

```text
HR
↓
VDOM Link
↓
Finance
```

Requires:

- Routes
- Policies
- VDOM Link

---

# 📊 6.14 VDOM Resource Allocation

Resources can be shared or reserved.

Examples:

- Sessions
- CPU
- Memory

Large enterprises may allocate resources.

```text
Production = 60%
HR         = 20%
Finance    = 20%
```

---

# 🛠️ 6.15 Management VDOM

One VDOM can manage:

- DNS
- NTP
- FortiGuard
- Logging

for the entire firewall.

Example:

```text
Root VDOM
```

The Root VDOM is often selected as the management VDOM.

---

# 🌍 6.16 Real World Deployments

## Enterprise Example

```text
FortiGate

├── Corporate VDOM
├── Guest VDOM
├── Server VDOM
└── DMZ VDOM
```

## ISP Example

```text
FortiGate

├── Customer-A
├── Customer-B
├── Customer-C
└── Customer-D
```

## MSSP Example

```text
FortiGate

├── Bank
├── Hospital
├── Retail
└── Education
```

One firewall serving multiple clients.

---

# 🔧 6.17 Troubleshooting VDOMs

## Show VDOMs

```text
get system status
```

## List VDOMs

```text
diagnose sys vd list
```

## Switch VDOM

```text
config vdom
edit Finance
```

## Check Routes

```text
get router info routing-table all
```

## Check Interfaces

```text
show system interface
```

---

# ⚠️ 6.18 Common VDOM Design Mistakes

- ❌ Forgetting Routes
- ❌ Missing Policies
- ❌ Wrong Interface Assignment
- ❌ No VDOM Link
- ❌ Wrong Admin Permissions
- ❌ Overloading One VDOM

---

# 🎤 Chapter 5 – Interview Questions & Answers

## Basic

### 1. What is a VDOM?

A Virtual Domain (VDOM) is a virtualization technology that allows a single FortiGate firewall to function as multiple independent virtual firewalls.

Each VDOM has its own:

- Interfaces
- Routing table
- Firewall policies
- NAT
- VPNs
- Administrators
- Security profiles
- Logging configuration

Each VDOM operates independently from the others.

---

### 2. Why are VDOMs used?

VDOMs are used to logically separate different networks or customers while sharing the same physical FortiGate hardware.

Common use cases include:

- Multi-tenancy
- MSSP deployments
- Departmental separation
- Development/Test environments
- Service provider environments
- Separate business units

---

### 3. What is the Root VDOM?

The Root VDOM is the default VDOM created when Multi-VDOM mode is enabled.

By default:

- It owns all interfaces.
- It contains the initial configuration.
- It acts as the default management VDOM unless another VDOM is assigned for management.

---

### 4. What is Multi-VDOM Mode?

Multi-VDOM Mode allows multiple independent VDOMs to exist on one FortiGate.

Example:

```text
FortiGate

├── Root VDOM
├── HR VDOM
├── Finance VDOM
├── DMZ VDOM
└── Guest VDOM
```

Each VDOM behaves like a separate firewall.

---

### 5. Can one interface belong to multiple VDOMs?

No.

A physical interface can belong to only one VDOM at a time.

However, VLAN subinterfaces on the same physical port can belong to different VDOMs.

Example:

```text
Port2
├── VLAN10 → HR VDOM
├── VLAN20 → Finance VDOM
├── VLAN30 → Guest VDOM
```

---

### 6. What is a VDOM Link?

A VDOM Link is a virtual interface used to connect two VDOMs together.

It acts like a virtual Ethernet cable.

Example:

```text
Root VDOM
     |
 VDOM Link
     |
 HR VDOM
```

Traffic between VDOMs passes through this link.

---

### 7. What is Inter-VDOM Routing?

Inter-VDOM Routing is the process of routing traffic between two different VDOMs using:

- VDOM Links
- Static Routes
- Dynamic Routing Protocols (OSPF/BGP)

Firewall policies are still required to permit traffic.

---

### 8. Difference between NAT and Transparent VDOM?

| NAT VDOM | Transparent VDOM |
| -------- | ---------------- |
| Layer 3 | Layer 2 |
| Performs routing | Bridges traffic |
| Supports NAT | Does not NAT bridged traffic |
| Has IP addresses | Uses management IP only for administration |
| Acts as gateway | Does not act as the default gateway for bridged traffic |

---

### 9. What is a Management VDOM?

A Management VDOM is the VDOM designated to manage system-wide administrative functions such as:

- GUI
- CLI
- DNS
- NTP
- FortiGuard
- FortiManager
- Licensing

Only one VDOM is normally assigned as the management VDOM.

---

### 10. What are the benefits of VDOMs?

Benefits include:

- Multi-tenancy
- Better security isolation
- Separate routing tables
- Independent firewall policies
- Reduced hardware costs
- Centralized management
- Resource sharing
- Administrative separation

---

# 🟡 Intermediate

### 11. Explain Multi-Tenancy using VDOMs.

Multi-tenancy allows multiple customers, departments, or business units to share the same physical FortiGate while remaining logically isolated.

Example:

```text
FortiGate

├── Customer A
├── Customer B
├── Customer C
```

Each customer receives:

- Separate interfaces
- Separate VPNs
- Separate routing
- Separate policies
- Separate administrators

No customer can access another customer's configuration unless explicitly permitted.

---

### 12. Explain Inter-VDOM Communication.

Communication between VDOMs requires:

#### Step 1 — Create a VDOM Link

```text
Root
  |
VDOM Link
  |
Finance
```

#### Step 2 — Configure IP Addresses on Both Ends

Example:

```text
Root
10.1.1.1/30

Finance
10.1.1.2/30
```

#### Step 3 — Configure Static or Dynamic Routes

#### Step 4 — Create Firewall Policies

Create policies allowing traffic.

Without routes and policies, communication will fail.

---

### 13. Explain VDOM Resource Allocation.

All VDOMs share the physical FortiGate hardware.

Resources include:

- CPU
- Memory
- Session Table
- ASICs (supported models)
- Interfaces

Administrators can configure limits such as:

- Maximum sessions
- Firewall policies
- Administrators
- CPU usage (platform dependent)

This prevents one VDOM from consuming excessive resources.

---

### 14. How would you deploy VDOMs in an MSSP environment?

Example:

```text
FortiGate

├── Customer A
├── Customer B
├── Customer C
├── Customer D
```

Each customer receives:

- Dedicated interfaces
- Dedicated VPNs
- Separate firewall policies
- Separate routing
- Individual administrators
- Independent logging

The MSSP centrally manages the device while maintaining isolation between customers.

---

### 15. Troubleshooting steps when two VDOMs cannot communicate.

#### Step 1 — Verify the VDOM Link

```text
show system vdom-link
```

#### Step 2 — Check Interface Status

```text
get system interface physical
```

#### Step 3 — Verify IP Addresses

```text
show system interface
```

#### Step 4 — Check Routing Tables in Both VDOMs

```text
get router info routing-table all
```

#### Step 5 — Verify Firewall Policies

```text
show firewall policy
```

#### Step 6 — Test Connectivity

```text
execute ping
```

Run the test from each VDOM.

#### Step 7 — Run a Flow Debug

```text
diagnose debug flow filter addr <IP>
diagnose debug flow trace start 10
diagnose debug enable
```

#### Step 8 — Verify Session Creation

```text
diagnose sys session list
```

---

# ⭐ Most Common Follow-up Interview Questions

## Q1. Can two VDOMs share the same routing table?

**Answer:**

No.

Each VDOM has its own independent routing table.

---

## Q2. Can two VDOMs have the same IP address?

**Answer:**

Yes.

Because VDOMs are isolated virtual firewalls, each VDOM can use overlapping IP address ranges without conflict.

Example:

```text
Root VDOM
192.168.1.1

HR VDOM
192.168.1.1

Finance VDOM
192.168.1.1
```

This is common in MSSP and multi-tenant environments.

---

## Q3. Can firewall policies in one VDOM affect another VDOM?

**Answer:**

No.

Firewall policies are local to each VDOM.

A policy configured in one VDOM does not apply to any other VDOM.

---

## Q4. Can OSPF or BGP run inside a VDOM?

**Answer:**

Yes.

Each VDOM can independently run:

- OSPF
- BGP
- RIP
- Static Routing

Each maintains its own routing processes and routing table.

---

## Q5. Can a VDOM have its own administrators?

**Answer:**

Yes.

You can assign administrators to a specific VDOM, restricting them so they can manage only that VDOM.

---

## Q6. Can one FortiGate have both NAT and Transparent VDOMs?

**Answer:**

Yes.

One VDOM can operate in NAT mode, while another operates in Transparent mode.

This allows flexible deployments where different network segments have different requirements.

---

## Q7. What is the biggest advantage of VDOMs?

**Answer:**

The biggest advantage is virtualization and isolation.

A single FortiGate can securely host multiple independent virtual firewalls, each with its own interfaces, routing, policies, VPNs, and administrators.

This reduces hardware costs while providing strong separation between networks or customers.

---

# 💼 Interview Tip

A strong summary statement is:

> **"A VDOM is a virtual firewall instance within a FortiGate. Each VDOM operates independently with its own interfaces, routing table, firewall policies, VPNs, and administrators. VDOMs are widely used for multi-tenancy, departmental isolation, and MSSP deployments, allowing a single FortiGate to securely support multiple independent environments."**

---

# 📌 Chapter Summary

VDOMs allow a single physical FortiGate to operate as multiple independent virtual firewalls.

The major concepts covered in this chapter are:

```text
VDOM
   ↓
Virtual Firewall
   ↓
Isolation
   ↓
Separate Routing
   ↓
Separate Policies
   ↓
Separate Administration
   ↓
VDOM Links
   ↓
Inter-VDOM Routing
```

VDOMs are particularly useful for:

- Multi-tenancy
- MSSP environments
- Enterprise segmentation
- Departmental isolation
- Service provider environments
- Shared firewall infrastructure

---

# 🧠 Key Takeaways

- VDOM stands for Virtual Domain.
- A VDOM allows one FortiGate to operate as multiple virtual firewalls.
- Each VDOM has independent interfaces, routing, policies, VPNs, and administration.
- VDOMs provide strong logical isolation.
- Multi-VDOM Mode enables multiple VDOMs.
- The Root VDOM is the default VDOM.
- A physical interface can belong to only one VDOM at a time.
- VLAN subinterfaces can be assigned to different VDOMs.
- NAT Mode VDOMs provide Layer 3 firewall functionality.
- Transparent Mode VDOMs operate as Layer 2 firewalls.
- VDOM Links provide connectivity between VDOMs.
- Inter-VDOM communication requires VDOM Links, routes, and firewall policies.
- VDOMs can support separate routing processes.
- Resources such as CPU, memory, and sessions are shared or allocated across VDOMs.
- A Management VDOM can handle system-wide management functions.
- VDOMs are widely used in multi-tenant and MSSP environments.
- Correct interface assignment, routing, policies, and administrative permissions are essential for proper VDOM operation.

---

# 📚 Quick Revision

```text
                         VDOM
                           |
              +------------+------------+
              |                         |
        Virtual Firewall           Isolation
              |                         |
      +-------+-------+                 |
      |       |       |                 |
   Routing Policies  VPNs               |
      |       |       |                 |
      +-------+-------+-----------------+
                      |
                 Administration


                    VDOM TYPES
                        |
              +---------+---------+
              |                   |
           NAT Mode         Transparent Mode
              |                   |
          Layer 3              Layer 2
          Routing              Bridging
            NAT              Inspection


                 INTER-VDOM
                     |
                VDOM Link
                     |
              +------+------+
              |             |
            VDOM A        VDOM B
              |             |
            Route         Route
              |             |
           Policy        Policy
              +------+------+
                     |
                Communication
```

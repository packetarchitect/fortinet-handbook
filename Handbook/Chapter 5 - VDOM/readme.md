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

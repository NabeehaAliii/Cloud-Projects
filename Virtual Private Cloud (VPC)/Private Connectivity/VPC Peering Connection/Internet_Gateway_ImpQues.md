# VPC Peering and Internet Access

## Overview

This document outlines how VPC peering works in relation to internet access, specifically when one VPC has an Internet Gateway (IGW) and another VPC does not. The purpose is to clarify the limitations of VPC peering with regard to sharing internet access.

### What is VPC Peering?

VPC peering allows two Virtual Private Clouds (VPCs) to communicate with each other through private IP addresses. It enables resources in one VPC to connect to resources in another VPC as if they are within the same network.

### Scenario

- **VPC1**: This VPC has an **Internet Gateway (IGW)**, which allows its instances to communicate with the internet.
- **VPC2**: This VPC is peered with VPC1 but does **not** have its own Internet Gateway.

### Question

**Can instances in both VPC1 and VPC2 access the internet after the VPCs are peered?**

### Answer

No, the instances in VPC2 **cannot automatically** access the internet through the IGW attached to VPC1. 

### Why?

1. **VPC Peering** enables private IP communication between VPCs but does **not support transitive routing**. This means that traffic between VPCs cannot be forwarded through the Internet Gateway of one VPC to reach the internet.
  
2. **Internet Gateways (IGW)** only allow instances **within the same VPC** to access the internet. Therefore, VPC2 cannot "borrow" the internet access of VPC1.

## Solutions

If instances in **VPC2** also need internet access, you have the following options:

### 1. Attach an Internet Gateway (IGW) to VPC2
   - You can add a separate **IGW** for VPC2, which will allow its instances to access the internet directly.

### 2. Set Up a NAT Gateway
   - If you do not want to expose VPC2 instances directly to the internet, you can set up a **NAT Gateway** in VPC1, which allows outbound traffic from VPC2 to access the internet but restricts inbound traffic.

### 3. Route Tables Configuration
   - After setting up a NAT Gateway in VPC1, configure the route tables in VPC2 to direct outbound internet traffic through the NAT Gateway in VPC1.

## Example Configuration

```bash
# Route table configuration for VPC2 to send traffic through NAT Gateway in VPC1
# Add the following route to VPC2’s route table:
Destination: 0.0.0.0/0
Target: nat-xxxxxxx (NAT Gateway in VPC1)

# For instances in VPC1, they already have access via IGW, so their route tables look like:
Destination: 0.0.0.0/0
Target: igw-xxxxxxx (Internet Gateway in VPC1)
```

### Key Concepts
- **VPC Peering**: Allows VPCs to communicate, but not to share internet access.
- **Internet Gateway (IGW)**: Provides direct internet access to instances in the VPC it is attached to.
- **NAT Gateway**: Enables instances in a private subnet to initiate outbound connections to the internet, without allowing inbound connections.

## Summary

In conclusion, peering VPCs does not allow VPCs to share internet access. VPC2 needs either its own Internet Gateway or a NAT Gateway configured in VPC1 to access the internet. Each VPC must be individually configured for internet access, even if they are peered.

# Site-to-Site VPN Connection between AWS and On-Premises

This repository provides a step-by-step guide to set up a Site-to-Site VPN connection between AWS and an on-premises network using AWS Transit Gateway (TGW) and Customer Gateway (CGW).

## Overview

The guide walks you through the process of creating a secure VPN tunnel between your AWS VPC and an on-premises network. It covers the following steps:

1. **Create a Transit Gateway (TGW)**
2. **Attach a VPC to the Transit Gateway**
3. **Create a Customer Gateway (CGW)**
4. **Create a Transit Gateway VPN Attachment**
5. **Download Configuration for Router**
6. **Configure IPSec VPN on the Router**
7. **Configure TGW Route Table**
8. **Configure VPC Route Table**
9. **Verify the Connection**
10. **Troubleshooting**

## Prerequisites

- An AWS account with necessary permissions to create and manage VPC, Transit Gateway, and VPN connections.
- An on-premises router with a public IP address.
- Basic knowledge of AWS VPC, Transit Gateway, and VPN configurations.

## Steps

### Step 1: Create a Transit Gateway (TGW)
1. Log in to the **AWS Management Console**.
2. Navigate to **VPC** → **Transit Gateway** → Click **Create Transit Gateway**.
3. Configure the TGW with a descriptive name, Amazon ASN, and enable default route table association and propagation.
4. Click **Create Transit Gateway**.

### Step 2: Attach a VPC to the Transit Gateway
1. Go to the **Transit Gateway Attachments** section in the TGW Console.
2. Click **Create Transit Gateway Attachment**.
3. Configure the attachment with the TGW ID, VPC ID, and subnet IDs.
4. Click **Create Attachment**.

### Step 3: Create a Customer Gateway (CGW)
1. In the **VPC Dashboard**, go to **Customer Gateways** → Click **Create Customer Gateway**.
2. Configure the CGW with a name, the public IP address of your on-premises router, and leave BGP ASN blank for static VPN.
3. Click **Create Customer Gateway**.

### Step 4: Create a Transit Gateway VPN Attachment
1. Go to the **Transit Gateway Attachments** section in the TGW Console.
2. Click **Create Transit Gateway Attachment**.
3. Configure the attachment with the TGW ID, VPN as the attachment type, and select the CGW created in Step 3.
4. Choose **Static** for routing options and enable acceleration.
5. Click **Create Attachment**.

### Step 5: Download Configuration for Router
1. In the **VPC** → **Virtual private network (VPN)** → Click on **Site-to-Site VPN connections**.
2. Verify the VPN created in the previous step and wait until it gets in the available state.
3. Click **Download Configuration**.
4. Select **Generic** as the vendor and **IKEv1** for compatibility with your router.
5. Save the configuration file.

### Step 6: Configure IPSec VPN on the Router
1. Using the downloaded configuration file, configure two IPSec VPN tunnels on your on-premises router.
2. Configure the tunnel interfaces, IKEv1 settings, and static routing as per the configuration file.

### Step 7: Configure TGW Route Table
1. Navigate to **Transit Gateway Route Tables** in the TGW Console.
2. Select the TGW route table associated with the VPN attachment.
3. Add a static route with the on-premises CIDR block as the destination and the VPN attachment as the target.
4. Save the route.

### Step 8: Configure VPC Route Table
1. In the **VPC Dashboard**, go to **Route Tables**.
2. Select the route table associated with your VPC subnets.
3. Add a static route with the on-premises CIDR block as the destination and the TGW attachment VPC as the target.
4. Save the route.

### Step 9: Verify the Connection
1. Check the VPN connection status in the AWS Console.
2. Ensure both tunnels or one of the tunnels are in the **UP** state.
3. Test the connection by pinging or telneting between an on-premises machine and an EC2 instance in the VPC.

### Step 10: Troubleshooting
1. If tunnels are down, check pre-shared keys and IPSec/IKE configurations.
2. If routing fails, verify TGW and VPC route tables, and check security group and NACL rules.

## References

- [AWS Transit Gateway Documentation](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html)
- [AWS Site-to-Site VPN Documentation](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

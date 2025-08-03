# VPC and EC2 Setup

## 1. VPC Creation
- Created a VPC with CIDR block: `10.0.0.0/16`

## 2. Subnet Creation
- **Private Subnet**: `10.0.2.0/24`
- **Public Subnet**: `10.0.0.0/24`

## 3. Internet Gateway
- Created an Internet Gateway.
- Attached it to the **VPC**.
- Associated it with the **Public Subnet**.

## 4. Route Table
- Created a Route Table.
- Added route with:
  - **Destination**: `0.0.0.0/0`
  - **Target**: Internet Gateway
- Associated this route table with the **Public Subnet**.

## 5. EC2 Instances
- Launched **3 EC2 instances**:
  1. One in the **Public Subnet**
  2. One in the **Private Subnet**
  3. One in an **External Subnet**

## 6. Connectivity
- **Direct connection** from the external instance to the private instance: ❌ Not possible
- **Via Public Subnet**:
  - Connect to the **public instance** first (bastion host)
  - Then SSH into the **private instance** from there ✅

> The instance in the public subnet acts as a **bastion host** to access resources in the private subnet.

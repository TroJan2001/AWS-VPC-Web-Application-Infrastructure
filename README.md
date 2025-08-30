# AWS VPC Web Application Project

## 📖 Introduction
This project demonstrates the deployment of a secure **Virtual Private Cloud (VPC)** environment on **AWS**, designed to host a simple web application with a backend database. The system architecture separates public and private resources, ensures controlled access through bastion hosts and security groups, and uses AWS services like **Route 53** for DNS and a **Load Balancer** for traffic management.

---

## 🏗️ Network Architecture
The VPC is divided into three subnets, each serving a distinct purpose:

1. **DMZ Public Subnet**
   - Contains:
     - **Bastion Host (EC2 instance)** for secure SSH access to private subnets.
     - **NAT Gateway** for outbound internet access from private subnets.
     - **Load Balancer** to distribute incoming HTTPS traffic to the web servers.

2. **Front-End Private Subnet**
   - Hosts two **web servers** configured with Apache/PHP.  
   - Web servers listen on **port 8080** and receive traffic from the load balancer.  
   - Instances can access the internet for updates via the NAT Gateway.  

3. **Back-End Private Subnet**
   - Contains the **MySQL Database Server**.  
   - Only accessible from the web servers on port **3306**.  
   - Outbound internet access is enabled through the NAT Gateway for updates.  

---

## 🔐 Security Groups
Security was enforced with dedicated **security groups**:
- **Bastion Host SG**: Allows SSH (22) from anywhere.  
- **Load Balancer SG**: Allows inbound HTTPS (443).  
- **Web Server SG**:  
  - SSH access only from Bastion Host SG.  
  - HTTP (8080) access from Load Balancer SG.  
- **Database SG**:  
  - MySQL (3306) access only from Web Server SG.  
  - SSH restricted to the Database Endpoint SG.  

This ensures controlled communication paths and isolation of resources.

---

## ⚙️ Components Setup

### VPC and Networking
- **VPC Name:** `MyVPC` (`10.0.0.0/16`)  
- **Subnets:**
  - Public Subnet → `10.0.0.0/24`
  - Front-End Subnet → `10.0.1.0/24`
  - Back-End Subnet → `10.0.2.0/24`  
- **Routing:**
  - Public Route Table → Internet Gateway  
  - Private Route Table → NAT Gateway  

### Bastion Host
- EC2 instance for secure SSH access.  
- Key pair: `SSH Bash.pem`.  

### Web Servers
- Two Ubuntu EC2 instances running Apache.  
- Apache configured to listen on **port 8080** instead of 80.  
- Custom index pages deployed for testing.  
- Key pairs: `web.pem` and `web2.pem`.  

### Database
- MySQL installed on an EC2 instance in the back-end subnet.  
- Database: `test_db` with table `users (id, username)`.  
- Created user `noor`, restricted to `10.0.2.0/24` for web server access.  

---

## 🌐 Extra Setup

### PHP and Environment Variables
- Installed **PHP** and required extensions on both web servers.  
- Used **Composer** to install [`vlucas/phpdotenv`](https://github.com/vlucas/phpdotenv).  
- Database credentials stored in a `.env` file with restricted permissions.  
- A **PHP script (`index.php`)** retrieves data from MySQL and outputs JSON.  

### Domain and DNS (Route 53)
- Purchased domain: **lutfe.site**.  
- Created a **Route 53 hosted zone** for DNS management.  
- Configured an **A record** alias pointing to the Load Balancer DNS.  
- The application is now accessible at [https://lutfe.site](https://lutfe.site).  

---

## 📄 Documentation
The full detailed documentation (with diagrams and screenshots) is available in the following PDF:

[AWS.pdf]

---

## ✅ Conclusion
This project demonstrates the successful creation of a secure and functional AWS VPC environment. By implementing public and private subnets, routing, security groups, a bastion host, a load balancer, and a database, the setup reflects **best practices in AWS networking and application hosting**. The use of Route 53 for DNS and environment variables for sensitive credentials further enhances both usability and security.

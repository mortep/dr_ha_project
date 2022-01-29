# Infrastructure

## AWS Zones
Identify your zones here

## Servers and Clusters

### Table 1.1 Summary
| Asset | Purpose | Size | Qty | DR |
|---|---|---|---|---|
| VPC | Virtual private cloud - Network where we put the subnets and assets | - | 1 | Replicated in the DR region |
| EC2 instances | Servers that run the web application | t3.micro | 3 | Replicated in the DR region |
| EKS cluster | Kubernetes cluster running the monitoring stack | t3.medium | 2 nodes + control plane | Replicated in the DR region |
| Application Load Balancer | Handle the HTTP requests and forward them to the EC2 instances running the web app | - | 1 | Replicated in the DR region |
| RDS Database - Primary | Database in the main region | t2.small | 2 | Replicated in the DR region as secondary database |
| RDS Database - Secondary | Database replica in the DR region | t2.small | 2 | Replica of the main region DB |

### Descriptions

*VPC*: Network where the EC2 instances and the databases are located. We create a VPC in `us-east-2` for the main assets and another VPC in `us-west-1` for 
the DR assets.

*EC2 instances*: These Ubuntu servers host the Flask application. Each region has 3 instances distributed among different availability zones within the region. For example, `us-east-2a` and `us-east-2b` for the main region and `us-west-1b` and `us-west-1c` for the DR region.

*EKS cluster*: A kubernetes cluster running the monitoring stack, i.e., prometheus and grafana, running in 2 nodes (in different AZs) to make it highly available.

*Application Load Balancer*: The LB is used to distribute the traffic load among the EC2 instances.

*RDS database*: The main database runs in the main region and the secondary database runs in the DR region acting as a replica of the main database. There is high availability since each cluster runs in more than one AZ.

## DR Plan
### Pre-Steps:

1. Make sure the main region is properly deployed.
2. Make sure the DR region is an exact replica of the main region assets.
3. Make sure that the database replica is working properly and moving the data between regions.

## Steps:

1. In Route 54, we have configured the API domain to point to the Application Load Balancer in the main region.
2. In case of a disaster in the main region, we need to modify the DNS records so the API domain points now to the Application Load Balancer in the DR region.

Nothing to do with the database since AWS will promote the replica cluster to regional cluster.
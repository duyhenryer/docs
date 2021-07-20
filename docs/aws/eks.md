# EKS

!!! note "Work in progress"
    This document is a work in progress.

## Pod networking (CNI)
```
Amazon EKS supports native VPC networking via the Amazon VPC CNI plugin for Kubernetes. Using this CNI plugin allows
Kubernetes pods to have the same IP address inside the pod as they do on the VPC network.
```
From https://docs.aws.amazon.com/eks/latest/userguide/pod-networking.html

This is really a great feature. But there is a one import limitation we will encounter if you want to run so many Pods in a EC2 worker node.
Whenever you deploy a Pod in the EKS worker node, EKS creates a new IP address from VPC subnet and attach to the instance.

You can check how many ethernet interfaces and max IP per interface from https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#AvailableIpPerENI


| Instance type   |      Maximum network interfaces      |  Private IPv4 addresses per interface |  IPv6 addresses per interface |
|-----------------|:------------------------------------:|--------------------------------------:|------------------------------:|
| m4.large	      |     2	                             |       10                              |	    10 |
| m4.xlarge	      |     4	|       15	    |       15 |
| m4.2xlarge      |	    4	|       15	    |       15 |
| m4.4xlarge      |  	8	|       30	    |       30 |
| m4.10xlarge     |  	8	|       30	    |       30 |
| m4.16xlarge     |  	8	|       30	    |       30 |
| m5.large        |	    3	|       10	    |       10 |
| m5.xlarge       |	    4	|       15	    |       15 |
| m5.2xlarge      |	    4	|       15	    |       15 |

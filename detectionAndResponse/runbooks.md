# Runbooks

## Compromised EC2 Instance

Steps to address compromised instances:

1) **Capture the instance's metadata**
2) **Enable Termination Protection**
3) **Isolate the instance** (replace instance's SG's - no outbound traffic authorized)
4) **Detach the instance from any ASG (Suspend processes)**
5) **Deregister the instance from any ELB**
6) **Snapshot the EBS volumes** (deep analysis)
7) **Tag the EC2 instance** (e.g., investigation ticket)

- **Offline investigation**: shutdown instance
- **Online investigation**: (e.g., snapshot memory or capture network traffic)
- Automate the isolation process: **Lambda**
- Automate the memory capture: **SSM Run Command**

---

## Compromised S3 Bucket

1) Identify the compromised S3 bucket using **GuardDuty**
2) Identify the source of the malicious activity (e.g., IAM user, role) and the API calls using **CloudTrail or Amazon Detective**
3) Identify whether the source was authorized to make those API calls
4) Secure your S3 bucket, recommended settings:

    - S3 Block Public Access Settings, S3 Bucket Policies and User Policies, VPC Endpoints for S3, S3 Presigned URLs, S3 Access Points, S3 ACLs

---

## Compromised ECS Cluster

1) Identify the source of the malicious activity (e.g., container images, tasks)
2) Isoloate the impacted tasks (deny all ingress/egress traffic to the task using security groups)
3) Evaluate the presence of malicious activity (e.g., malware)

---

## Compromised Standalone Container

1) Identify the malicious container using **GuardDuty**
2) Isolate the malicious container (deny all ingress.egress traffic to the container)
3) Suspend all processes in the container (pause the container)
or
4) Or stop the container and look at EBS Snapshots retained by GuardDutym(snapshot retention feature)
5) Evaluate the presense of malicious activity (e.g., malware)

---

## Compromised RDS Database Instance

1) Identify the affected DB instance and DB user using **GuardDuty**
2) If it is NOT legitimate behavior:

    - Restrict network access (SGs & NACLs)
    - Restrict the DB access for the suspected DB user

3) Rotate the suspected DB users' passwords
4) Review DB Audit Logs to identify leaked data
5) Secure your RDS DB insance, recommended settings:

    - Use Secrets Manager to rotate the DB password
    - Use IAM DB Authentication to manage DB users' access without passwords

---

## Compromised AWS Credentials

1) Identiry the affected IAM user using **GuardDuty**
2) Rotate the exposed AWS Credentials
3) Invalidate temporary credentials by **attaching an explicit Deny policy** to the affected IAM user with an STS date condition (see IAM section)
4) Check **CloudTrail logs** for other unauthorized activity
5) Review your AWS resources (e.g., delete unauthorized resources)
6) Varify your AWS account information

---

## Compromised IAM Role

1) Invalidate temporary credentials by **attaching an explicit Deny policy** to the affected IAM user with an STS date condition (see IAM section)
2) Revoke access for the identity to the linked AD if any
3) Check **CloudTrail logs** for other unauthorized activity
4) Review your AWS resources (e.g., delete unauthorized resources)
5) Verify your AWS account information

---

## Compromised Account

1) Rotate and delete exposed AWS Access Keys
2) Rotate and delete any unauthorized IAM user credentials (rotate existing IAM users' passwords)
3) Rotate and delete all EC2 Key Pairs
4) Check **CloudTrail logs** for other unauthorized activity
5) Review your AWS resources (e.g., delete unauthorized resources)
6) Verify your AWS account information

---


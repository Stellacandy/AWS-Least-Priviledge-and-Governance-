
AWS Cloud Security: Identity Governance & Audit Logging 

Project Overview
This project demonstrates the implementation of the Principle of Least Privilege (PoLP) and Governance Monitoring within an AWS environment. By leveraging AWS IAM, CloudTrail, and S3, I created a secure framework to prevent unauthorized resource modification while ensuring a verifiable audit trail for compliance.

This project aligns with the ISC2 Certified in Cybersecurity (CC) domains:
- Domain 2: Incident Response
- Domain 3: Access Control Concepts
- Domain 4: Network Security


Architecture
The architecture focuses on three core pillars of security: Identification, Protection, and Detection.

1. IAM (Identity & Access Management): Custom JSON policies to restrict administrative power.
2. CloudTrail: Continuous logging of all API calls for accountability (Non-Repudiation).
3. S3 (Simple Storage Service): A hardened "Log Vault" with blocked public access and versioning enabled.
   

Technical Implementation

1. Hardening Storage (Amazon S3)
- Created a private bucket to house security logs.
- Security Control: Enabled "Block all public access" to prevent data leakage.
- Integrity Control: Enabled Bucket Versioning to protect logs from accidental or malicious deletion.

2. Implementation of Logging (AWS CloudTrail)
- Configured a management trail to monitor account-wide activity.
- Linked the trail to the secure S3 bucket.
- Verified that all "Write" and "Read" events are captured for forensic readiness.

3. Enforcing Least Privilege (IAM)
- Created a custom IAM policy, Auditor-Read-Only, designed for a "Junior Auditor" persona. The policy allows metadata visibility but explicitly denies destructive actions.

Policy Snippet (JSON):
JSON{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowConsoleVisibility",
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*",
                "ec2:Describe*",
                "iam:Get*",
                "iam:List*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ExplicitDenyDelete",
            "Effect": "Deny",
            "Action": [
                "s3:DeleteBucket",
                "s3:DeleteObject"
            ],
            "Resource": "*"
        }
    ]
}



4. Security Validation (The Test)
- To verify the effectiveness of the controls, I performed a "Breach Simulation"
- Assumption of Identity: Logged in as the junior-auditor user.
- Unauthorized Action: Attempted to delete the production log bucket via the S3 Console.
- Result: The action was Blocked by the IAM engine (Access Denied).
- Forensic Trail: Confirmed that the failed attempt was logged in CloudTrail, capturing the User IP, Timestamp, and the specific API call (DeleteBucket).


5. Key Learning Outcomes
- Governance: Understood how CloudTrail provides the "Who, What, and When" of account activity.
- Policy Evaluation Logic: Learned that an Explicit Deny always overrides an Allow in AWS.
- Data Integrity: Implemented S3 security best practices to ensure audit logs remain immutable and private.




Contact
Stella Ojiuba
Aspiring Cloud Security Professional 
www.linkedin.com/in/stellaojiuba
https://foul-crepe-4c8.notion.site/Cloud-Security-Project-Portfolio-Stella-Chinenyewa-Ojiuba

# AWS Cloud Security Posture Assessment (CSPM) Demo
> Version: 2025-09-03

> A practical, evidence-based walkthrough that simulates an insecure AWS account, detects misconfigurations with **AWS-native services**, remediates them, and produces an **executive-ready compliance summary**.

## 🔗 Report
- **Full PDF report:** [AWS_CSPM_Demo.pdf](https://github.com/louis-cyber-security234/Cloud-Security-Posture-Audit/blob/main/AWS_CSPM_Demo.pdf)

## Why this matters
Cloud misconfigurations remain a leading cause of breaches. This demo shows how to surface and fix high-impact risks using **Config, CloudTrail, Security Hub, and GuardDuty**, with clear evidence and remediation steps.

## Scope covered in this demo
- Identifies **publicly exposed S3 buckets** and **insecure IAM admin credentials**
- Closes **open SSH access** `0.0.0.0/0` and `::/0` on Security Groups
- Enables comprehensive **CloudTrail** account activity logging to S3
- Deploys **CIS AWS Foundations** **conformance packs** via **AWS Config**
- Shows **rule-level** status: `COMPLIANT`, `NON_COMPLIANT`, `INSUFFICIENT_DATA`
- Provides **pack-level** compliance roll-up
- Integrates **Security Hub** and **GuardDuty** for ongoing threat detection
- Produces an **executive/audit-ready** posture summary with remediation guidance

## Evidence map > where to see it in the PDF
> Page numbers below refer to the linked PDF.

- **Executive outcomes** > p.2  
  High-level summary of the demo’s results and value.
- **Public S3 bucket exposure** > p.1, p.19  
  Shows public object access and later remediation/validation.
- **Insecure IAM admin credentials** > p.2, p.26  
  Demonstrates risky credentials and their clean-up.
- **Open SSH to the world (0.0.0.0/0, ::/0)** > p.1, p.2, p.5, p.21, p.26  
  Evidence of exposure, rule tightening, and re-evaluation.
- **CloudTrail logging enabled and verified** > p.1–2, p.5–8, p.20–21, p.27  
  Trail creation, S3 delivery, and verification artefacts.
- **CIS AWS Foundations conformance pack** > p.1–2, p.11, p.14, p.17–18, p.22–24, p.27  
  Pack deployment, rule evaluations, and roll-ups.
- **Compliance statuses (rule-level)** > p.2, p.18–19, p.21–22  
  Shows `COMPLIANT / NON_COMPLIANT / INSUFFICIENT_DATA` with screenshots.
- **Security Hub integration** > p.1–2, p.17, p.23–25, p.27  
  Standards enabled, severity summaries, and posture view.
- **GuardDuty integration** > p.2, p.16–17, p.25, p.27  
  Detector enabled, sample findings, and integration with Security Hub.

> If your PDF uses a different pagination after edits, adjust the page references above accordingly.

## Architecture and services
- **AWS Config** > Conformance packs, rule evaluations, compliance roll-ups
- **AWS CloudTrail** > Account activity logging to S3 (audit/forensics)
- **Amazon S3** > Log storage + public bucket exposure demonstration
- **Amazon EC2 / Security Groups** > SSH ingress demonstration and fix
- **AWS IAM** > Temporary insecure admin artefacts for findings, then remediation
- **Amazon GuardDuty** > Ongoing threat detection findings
- **AWS Security Hub** > Posture aggregation, standards, severity summaries

## How to reproduce (high level)
1. **Baseline risks**  
   Create a test **public S3 bucket**, a **temporary over-permissive IAM** user/role, and **open SSH** on a Security Group.
2. **Enable logging**  
   Create **CloudTrail** writing to an S3 log bucket, then confirm `.json.gz` objects arrive.
3. **Enable detection & posture**  
   Turn on **AWS Config** and deploy the **CIS AWS Foundations** conformance pack; enable **GuardDuty** and **Security Hub** standards.
4. **Collect evidence**  
   Capture **rule evaluations**, **pack roll-ups**, and **Security Hub severity** summaries.
5. **Remediate & re-check**  
   Block S3 public access, close SSH `0.0.0.0/0` and `::/0`, deprovision insecure IAM, then re-run evaluations.
6. **Export report**  
   Compile screenshots and notes into the provided PDF.

> Important: run in a **sandbox/test account**. Do **not** create insecure resources in production.

## Cleanup (to avoid charges)
Run only what applies to what you created for this demo. Replace placeholders as needed.

```bash
# GuardDuty (per region)
gd_detector_id=$(aws guardduty list-detectors --query "DetectorIds[0]" --output text)
aws guardduty delete-publishing-destination --detector-id "$gd_detector_id" --id "$(aws guardduty list-publishing-destinations --detector-id "$gd_detector_id" --query 'Destinations[0].DestinationId' --output text)" 2>/dev/null || true
aws guardduty delete-detector --detector-id "$gd_detector_id" 2>/dev/null || true

# Security Hub
aws securityhub disable-security-hub 2>/dev/null || true

# AWS Config conformance pack
aws configservice delete-conformance-pack --conformance-pack-name CIS-Foundations 2>/dev/null || true

# CloudTrail (and its S3 bucket only if created solely for this demo)
trail_name="OrgTrail-CSPM-Demo"
aws cloudtrail stop-logging --name "$trail_name" 2>/dev/null || true
aws cloudtrail delete-trail --name "$trail_name" 2>/dev/null || true
# If you created a dedicated S3 bucket just for CloudTrail logs in this demo, empty it before deletion:
# aws s3 rm s3://<your-cloudtrail-bucket> --recursive
# aws s3api delete-bucket --bucket <your-cloudtrail-bucket>

# Security Group ingress (close SSH 0.0.0.0/0 and ::/0)
aws ec2 revoke-security-group-ingress --group-id sg-XXXX --ip-permissions IpProtocol=tcp,FromPort=22,ToPort=22,IpRanges='[{CidrIp=0.0.0.0/0}]' 2>/dev/null || true
aws ec2 revoke-security-group-ingress --group-id sg-XXXX --ip-permissions IpProtocol=tcp,FromPort=22,ToPort=22,Ipv6Ranges='[{CidrIpv6=::/0}]' 2>/dev/null || true

# Insecure IAM artefacts (delete demo user/keys/role)
aws iam delete-access-key --user-name demo-admin --access-key-id AKIA... 2>/dev/null || true
aws iam delete-user --user-name demo-admin 2>/dev/null || true
# For roles/policies, detach then delete as required.
```

> Double-check **all regions** you used. Some services (e.g., GuardDuty, Config) are regional.

## Suggested repo structure
```
.
├── README.md
├── AWS_CSPM_Demo.pdf
├── scripts/
│   └── cleanup.sh
└── evidence/    > optional: raw screenshots or exported JSON
```

## Licence
MIT. See `LICENCE` if included.

---
> Maintained by Louis Moyo · LM Consulting


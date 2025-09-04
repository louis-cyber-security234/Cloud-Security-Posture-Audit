# AWS Cloud Security Posture Audit (CSPM) — Demo

A fast, CLI-first demo showing how to **detect, remediate, and prove** common AWS misconfigurations using native services (CloudTrail, AWS Config, Security Hub, GuardDuty).

📕 Guide: [AWS CSPM Demo Guide (PDF)](https://github.com/louis-cyber-security234/Cloud-Security-Posture-Audit/blob/main/AWS_CSPM_Demo_Guide.pdf)

**What you’ll see**
- Baseline visibility (CloudTrail + AWS Config) and Security Hub/GuardDuty enabled.
- High-risk fixes: S3 public access blocked, internet-exposed SSH removed, SSM public sharing disabled, ECR enhanced scanning on.
- Final posture proof for the demo scope: CloudTrail **ON**, Config **SUCCESS**, GuardDuty **ENABLED**, Security Hub active findings **0/0/0** (Crit/High/Med).

**What’s next**
- A concise **90-day remediation roadmap** at the end of the guide to prevent drift and operationalize the controls.

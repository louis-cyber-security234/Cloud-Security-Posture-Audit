# AWS Cloud Security Posture Assessment (CSPM) Demo

A practical, evidence-based walkthrough that simulates an insecure AWS account, detects misconfigurations with AWS-native services, remediates them, and produces an executive-ready compliance summary.

## Report
- **Full PDF:** https://github.com/louis-cyber-security234/Cloud-Security-Posture-Audit/blob/main/AWS_CSPM_Demo.pdf

## What this demo covers
- Identifies publicly exposed S3 buckets and insecure IAM admin credentials
- Closes open SSH access `0.0.0.0/0` and `::/0`
- Enables comprehensive activity logging via AWS CloudTrail
- Deploys CIS AWS Foundations conformance packs with AWS Config
- Shows rule-level status: `COMPLIANT`, `NON_COMPLIANT`, `INSUFFICIENT_DATA`
- Provides pack-level compliance roll-up
- Integrates Security Hub and GuardDuty for ongoing threat detection
- Generates an executive and audit-ready summary with remediation guidance

## Architecture and services
- **AWS Config** - conformance packs, rule evaluations, compliance roll-ups
- **AWS CloudTrail** - account activity logging to S3
- **Amazon S3** - log storage and public bucket demonstration
- **Amazon EC2 / Security Groups** - SSH ingress demonstration and fix
- **AWS IAM** - temporary over-permissive admin for findings, then remediation
- **Amazon GuardDuty** - threat detection findings
- **AWS Security Hub** - posture aggregation, standards, severity summaries

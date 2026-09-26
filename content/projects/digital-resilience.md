---
title: "Digital Resilience: Failover-Resilient Serverless Architecture"
summary: "A high-availability AWS architecture with dual-origin failover, built and live-tested for the EDN Hackathon Future Builders (Case B: Digital Resilience)."
date: 2026-09-01
---

## Context

Built independently for the EDN Hackathon "Future Builders" (Escola da Nuvem × AWS),
Case B: Digital Resilience — a challenge to design infrastructure that stays available
under regional failure conditions.

## Architecture

- **AWS WAF + CloudFront** in front of a dual-origin setup: S3 primary in `sa-east-1`,
  secondary failover origin in `us-east-1`
- **API Gateway** with direct-to-SQS integration — decoupling ingest from processing so a
  backlog doesn't become an outage
- **Lambda worker** consuming from SQS, writing to **DynamoDB**
- **CloudWatch + SNS** for observability and alerting across the failover path

## What I actually tested

This wasn't a diagram exercise. I validated the architecture end-to-end with real form
submissions and DynamoDB scans — confirming data landed correctly under both normal and
failover conditions. I also audited a teammate's Terraform-provisioned environment against
the project requirements, which surfaced gaps a diagram review alone wouldn't have caught.

## Why this design

Coming from 25 years of carrier-grade uptime requirements, the instinct here wasn't
"make it work" — it was "make it survive losing a region without anyone noticing." The
dual-origin failover and SQS decoupling are the same pattern I'd apply to any system where
downtime has a real cost, just expressed in AWS-native primitives instead of telecom
infrastructure.

---

*Code and architecture diagrams: [GitHub](https://github.com/drlandi)*

<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Proposed  
Date: 2026-10-22

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- GCP
- Azure
- Self-hosted

## Decision Outcome

Chosen option: AWS, because AWS provides managed PostgreSQL via RDS, container hosting via ECS Fargate for the backend and MinIO, and S3+CloudFront for the frontend. It is likely where the organization already runs infrastructure for an internal company tool.

## Consequences

AWS costs more than self-hosting for a tiny pilot. ECS Fargate has a minimum cost per task. The trade-off is operational simplicity and a clear growth path to autoscaling, authentication, and logging.
<!-- sdlc:auto:end adr -->
# Flask Three-Tier To-Do Application

A customized Flask To-Do application deployed on AWS as part of a Terraform-managed three-tier architecture.

**Application release:** `v0.1.0`  
**Verified commit:** `4068d45`

## Application Structure

- `WebLayer/` — Flask web interface served by Gunicorn behind Nginx.
- `ApplicationLayer/` — Flask API, SQLAlchemy models, and MySQL database integration.
- `ApplicationLayer/parameters.py` — Retrieves runtime database configuration from AWS Systems Manager Parameter Store.

## My Changes

The original application experienced a startup race when multiple Gunicorn workers attempted to create the same MySQL table simultaneously, producing error `1050: Table 'todotable' already exists`.

Release `v0.1.0` introduces coordinated database schema initialization. The modified application was deployed through an EC2 Auto Scaling instance refresh and a new application instance was verified to start without the previous duplicate-table error.

The AWS infrastructure is maintained in a separate Terraform repository.

## Deployment Architecture

Browser → External ALB → Nginx / Flask Web Tier → Internal ALB → Flask API Tier → Amazon RDS MySQL

The application tiers run on private EC2 instances in two Availability Zones.

## Future Work

Database migration tooling, automated tests, dependency version pinning, and application-level monitoring.

## Attribution

Forked from [aws-samples/three-tier-web-application-flask](https://github.com/aws-samples/three-tier-web-application-flask).

Original project license: MIT-0. The LICENSE file is retained.
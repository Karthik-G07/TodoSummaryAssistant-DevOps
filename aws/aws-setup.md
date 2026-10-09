# AWS Setup

## Architecture
- Frontend: React application served by Nginx in Docker on EC2.
- Backend: Spring Boot REST API in Docker on EC2.
- Database: Amazon RDS for MySQL.
- Monitoring: Prometheus, Node Exporter and Grafana containers.
- Delivery: GitHub Actions builds Docker images, publishes versioned images and deploys through AWS Systems Manager.

## EC2
The Ubuntu EC2 instance hosts Docker containers. HTTP traffic enters through port 80. SSH should be restricted to the administrator's current IP. Monitoring ports should not be publicly exposed; restrict access to trusted IPs or use a secure access method.

## RDS
The MySQL RDS instance is configured without public access. Its security group should allow TCP 3306 only from the EC2 security group. Store database credentials in an environment file on the instance or a managed secrets service, never in source control.

## IAM and deployment
The EC2 instance uses an IAM role for Systems Manager. GitHub Actions uses OpenID Connect to assume a dedicated deployment role and invokes deployment commands through Systems Manager. Avoid long-lived AWS access keys in GitHub secrets.

## Configuration
Provide database connection details, API keys and webhook URLs through environment variables. `.env` files containing real credentials must not be committed.

## Validation
- Verify the application at the EC2 HTTP endpoint.
- Verify backend health at `/actuator/health`.
- Verify Prometheus targets at `/targets`.
- Verify alert rules at `/api/v1/rules`.
- Verify GitHub Actions build and deployment workflow status.

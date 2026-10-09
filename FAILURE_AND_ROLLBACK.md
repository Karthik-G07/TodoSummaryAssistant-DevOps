# Failure and Rollback

## 1. Backend container fails
Check `docker logs todo-backend`, environment variables, database reachability and health endpoint. Correct the configuration and restart the container.

## 2. Frontend returns errors
Check `docker logs todo-frontend`, Nginx configuration and browser network requests. Verify that `/api/` is proxied to the backend.

## 3. Database connection fails
Verify the RDS endpoint, database name, credentials, security group source and TCP 3306 connectivity. Never make RDS publicly accessible as a workaround.

## 4. CI/CD build fails
Review the failed GitHub Actions step and build logs. Fix the Dockerfile, dependencies or test failure, then push a corrective commit.

## 5. Deployment fails
Inspect the Systems Manager command output and container logs. Check that the expected image tag exists and that the EC2 instance can pull it.

## 6. New release is unhealthy
Retain the previously deployed image tag. Stop the unhealthy containers and restart the backend and frontend with the last known-good image tags and the same Docker network and environment configuration. Re-run health checks.

## Post-rollback validation
Check the frontend HTTP response, `/api/todos`, backend actuator health, Prometheus targets and alert states. Record the cause and corrective action.

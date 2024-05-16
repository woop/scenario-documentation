# Symptom

Ads not loading or displaying warnings.

# Potential Causes

- Ad service crashing due to Out Of Memory (OOM) error.
- Missing or incorrect environment variables.

# Investigation Steps

1. Confirm the issue by reproducing it.
2. Check the status of the ad service deployment in Kubernetes:


    kubectl get pods -l app=ad-service

3. Review the logs of the ad service for any errors:


    kubectl logs <ad-service-pod-name>

4. Check the environment variables of the ad service. Ensure environment=PROD is set for proper memory allocation in production.

# Resolution Steps

- If OOM error is present, ensure environment=PROD is set. If not, add it and rerun the service.
- If the issue persists, escalate to the development team.
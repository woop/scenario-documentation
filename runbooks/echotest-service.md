# EchoTest Service Runbook

## Steps to assess
1. Analyze logs for errors:
   - Check the pod logs for the echotest service. Make sure there are no errors.
   - Make sure that the entrypoint for the application is correctly defined, otherwise you may run into an import problem to load “main”
2. Check for any specific events if the pod is in Crashloop:
   - If the echotest pod appears to be in crashloop, check the events of the pod to analyse the reason for the Crashloop
3. Check pod status
   - Look at the last known termination state for pods to confirm the reason for failure by describing pods using a Kubernetes client or CLI
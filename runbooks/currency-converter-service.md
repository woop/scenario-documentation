# Currency Converter Service Runbook

## Steps to assess

1. Analyze logs for errors:
   - Check the pod logs for the demo-currency-converter pod. Make sure there are no errors.
2. Check for any specific events if the pod is in Crashloop:
   - If the demo-currency-converter pod appears to be in crashloop, check the events of the pod to analyse the reason for the Crashloop
3. Check deployment for currency converter
   - Use the Kubernetes CLI or client to describe the pod for demo-currency-converter.
     - Are the memory and cpu limits/requests correct?
     - Are there any failure events?

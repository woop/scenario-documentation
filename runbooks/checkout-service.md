# Checkout Service Runbook

## Links

[Grafana dashboard](https://grafana.demo-04.demo.app.cleric.io/d/W2gX2zHVk/demo-dashboard?orgId=1&var-service=checkoutservice)

## Symptom
Users unable to checkout when attempting to complete their order.

## Potential Causes

- Problems are often due to issues with one of the checkoutservice dependencies, eg:
  - paymentservice 
  - shippingservice 
  - currencyservice 
  - cartservice

## Investigation Steps

1. Confirm the issue by reproducing it.
2. If the problem was reported by users, check the frontend logs for any errors:


    kubectl logs -l app.kubernetes.io/component=frontend

3. Check traces on the jaeger dashboard for any abnormal spans. In the UI, set the service to checkoutservice and tags to error=true.
4. Check the logs for the checkoutservice and downstream dependencies such as the paymentservice and shippingservice.
5. 
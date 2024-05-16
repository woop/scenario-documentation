# AWS Authorisation Error Debugging Runbook

## Initial Assessment

1. Identify the Resource and Action:

- Determine which AWS resource (e.g., S3, Lambda) and action (e.g., read, write) are experiencing access issues.

2. Identify the Principal:

- Identify the user, role, or service attempting the action. Obtain its ARN if possible.

### Step-by-Step Debugging

1. Review Attached IAM Policies:
   - Script: Use list_identity_policies.py with the principal’s ARN. 
   - Purpose: Lists both managed and inline policies attached to the principal. Checks for permissions related to the action and resource. 
   - Command: python list_identity_policies.py <PrincipalARN>
   - Managed policies are often recommend but not necessary. If no managed policy is found, proceed to check in-line 
2. Inline Policy Details:
   - If the principal has inline policies that may be affecting access:
   - Script: Use get_inline_policy_details.py with the principal’s ARN and policy name. 
   - Purpose: Retrieves and displays the content of the inline policy. 
   - Command: python get_inline_policy_details.py <PrincipalARN> <PolicyName>
3. Managed Policy Details:
   - If a managed policy is attached, and you need more details:
   - Script: Use get_managed_policy_details.py with the policy’s ARN. 
   - Purpose: Fetches the content of the managed policy to inspect specific permissions. 
   - Command: python get_managed_policy_details.py <PolicyARN/Name>
4. Review Resource-specific Policies:
   - Script: Use get_resource_policies.py for resources like S3, Lambda, SNS, and SQS. 
   - Purpose: Retrieves the resource’s policy to check if the principal is allowed or denied access. 
   - Command: python get_resource_policies.py <ResourceType> <ResourceName/ARN>
   - Replace <ResourceType> with s3, lambda, sns, or sqs accordingly. 
5. Check for Explicit Deny:
   - Guidance: Within the outputs of the above scripts, look for any Deny statements that could be affecting access. Remember, Deny overrides Allow.
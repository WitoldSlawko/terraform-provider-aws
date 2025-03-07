---
subcategory: "Organizations"
layout: "aws"
page_title: "AWS: aws_organizations_policy"
description: |-
  Terraform data source for managing an AWS Organizations Policy.
---


<!-- Please do not edit this file, it is generated. -->
# Data Source: aws_organizations_policy

Terraform data source for managing an AWS Organizations Policy.

## Example Usage

### Basic Usage

```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import Fn, Token, TerraformStack
#
# Provider bindings are generated by running `cdktf get`.
# See https://cdk.tf/provider-generation for more details.
#
from imports.aws. import DataAwsOrganizationalPolicies
from imports.aws.data_aws_organizations_organization import DataAwsOrganizationsOrganization
from imports.aws.data_aws_organizations_policies_for_target import DataAwsOrganizationsPoliciesForTarget
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name):
        super().__init__(scope, name)
        DataAwsOrganizationalPolicies(self, "test",
            policy_id=Fn.lookup_nested(current.policies, ["0", "id"])
        )
        data_aws_organizations_organization_current =
        DataAwsOrganizationsOrganization(self, "current")
        data_aws_organizations_policies_for_target_current =
        DataAwsOrganizationsPoliciesForTarget(self, "current_2",
            filter="SERVICE_CONTROL_POLICY",
            target_id=Token.as_string(
                Fn.lookup_nested(data_aws_organizations_organization_current.roots, ["0", "id"
                ]))
        )
        # This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.
        data_aws_organizations_policies_for_target_current.override_logical_id("current")
```

## Argument Reference

The following arguments are required:

* `policy_id` - (Required) The unique identifier (ID) of the policy that you want more details on. Policy id starts with a "p-" followed by 8-28 lowercase or uppercase letters, digits, and underscores.

## Attribute Reference

This data source exports the following attributes in addition to the arguments above:

* `arn` - The Amazon Resource Name of the policy.
* `aws_managed` - Indicates if a policy is an AWS managed policy.
* `content` - The text content of the policy.
* `description` - The description of the policy.
* `name` - The friendly name of the policy.
* `type` - The type of policy values can be `SERVICE_CONTROL_POLICY | TAG_POLICY | BACKUP_POLICY | AISERVICES_OPT_OUT_POLICY`

<!-- cache-key: cdktf-0.20.0 input-64f486a6cfb19466cfa54daa60bbce856a5f3e8a11f79a4bf1dce126e773a631 -->
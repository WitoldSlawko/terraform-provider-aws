---
subcategory: "IAM Access Analyzer"
layout: "aws"
page_title: "AWS: aws_accessanalyzer_analyzer"
description: |-
  Manages an Access Analyzer Analyzer
---


<!-- Please do not edit this file, it is generated. -->
# Resource: aws_accessanalyzer_analyzer

Manages an Access Analyzer Analyzer. More information can be found in the [Access Analyzer User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html).

## Example Usage

### Account Analyzer

```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import TerraformStack
#
# Provider bindings are generated by running `cdktf get`.
# See https://cdk.tf/provider-generation for more details.
#
from imports.aws.accessanalyzer_analyzer import AccessanalyzerAnalyzer
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name):
        super().__init__(scope, name)
        AccessanalyzerAnalyzer(self, "example",
            analyzer_name="example"
        )
```

### Organization Analyzer

```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import TerraformStack
#
# Provider bindings are generated by running `cdktf get`.
# See https://cdk.tf/provider-generation for more details.
#
from imports.aws.accessanalyzer_analyzer import AccessanalyzerAnalyzer
from imports.aws.organizations_organization import OrganizationsOrganization
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name):
        super().__init__(scope, name)
        example = OrganizationsOrganization(self, "example",
            aws_service_access_principals=["access-analyzer.amazonaws.com"]
        )
        aws_accessanalyzer_analyzer_example = AccessanalyzerAnalyzer(self, "example_1",
            analyzer_name="example",
            depends_on=[example],
            type="ORGANIZATION"
        )
        # This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.
        aws_accessanalyzer_analyzer_example.override_logical_id("example")
```

## Argument Reference

The following arguments are required:

* `analyzer_name` - (Required) Name of the Analyzer.

The following arguments are optional:

* `tags` - (Optional) Key-value map of resource tags. If configured with a provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.
* `type` - (Optional) Type of Analyzer. Valid values are `ACCOUNT`, `ORGANIZATION`, `ACCOUNT_UNUSED_ACCESS `, `ORGANIZATION_UNUSED_ACCESS`. Defaults to `ACCOUNT`.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `arn` - ARN of the Analyzer.
* `id` - Analyzer name.
* `tags_all` - Map of tags assigned to the resource, including those inherited from the provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).

## Import

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import Access Analyzer Analyzers using the `analyzer_name`. For example:

```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import TerraformStack
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name):
        super().__init__(scope, name)
```

Using `terraform import`, import Access Analyzer Analyzers using the `analyzer_name`. For example:

```console
% terraform import aws_accessanalyzer_analyzer.example example
```

<!-- cache-key: cdktf-0.20.0 input-4062f5c13fefb0112227bba24e101e8cb8dee9426f907b5735a0434c43b3e06c -->
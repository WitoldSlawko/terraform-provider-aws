---
subcategory: ""
layout: "aws"
page_title: "Terraform AWS Provider Resource Tagging"
description: |-
  Managing resource tags with the Terraform AWS Provider.
---


<!-- Please do not edit this file, it is generated. -->
# Resource Tagging

Many AWS services implement [resource tags](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) as an essential part of managing components. These arbitrary key-value pairs can be utilized for billing, ownership, automation, [access control](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html), and many other use cases. Given that these tags are an important aspect of successfully managing an AWS environment, the Terraform AWS Provider implements additional functionality beyond the typical one-to-one resource lifecycle management for easier and more customized implementations.

-> Not all AWS resources support tagging, which can differ across AWS services and even across resources within the same service. Browse the individual Terraform AWS Provider resource documentation pages for the `tags` argument, to see which support resource tagging. If the AWS API implements tagging support for a resource and it is missing from the Terraform AWS Provider resource, a [feature request](https://github.com/hashicorp/terraform-provider-aws/issues/new?labels=enhancement&template=Feature_Request.md) can be submitted.

<!-- TOC depthFrom:2 -->

- [Getting Started with Resource Tags](#getting-started-with-resource-tags)
- [Ignoring Changes to Specific Tags](#ignoring-changes-to-specific-tags)
    - [Ignoring Changes in Individual Resources](#ignoring-changes-in-individual-resources)
    - [Ignoring Changes in All Resources](#ignoring-changes-in-all-resources)
- [Managing Individual Resource Tags](#managing-individual-resource-tags)
- [Propagating Tags to All Resources](#propagating-tags-to-all-resources)

<!-- /TOC -->

## Getting Started with Resource Tags

Terraform AWS Provider resources that support resource tags implement a consistent argument named `tags` which accepts a key-value map, e.g.,

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Vpc } from "./.gen/providers/aws/vpc";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new Vpc(this, "example", {
      tags: {
        Name: "MyVPC",
      },
    });
  }
}

```

The tags for the resource are wholly managed by Terraform except tag keys beginning with `aws:` as these are managed by AWS services and cannot typically be edited or deleted. Any non-AWS tags added to the VPC outside of Terraform will be proposed for removal on the next Terraform execution. Missing tags or those with incorrect values from the Terraform configuration will be proposed for addition or update on the next Terraform execution. Advanced patterns that can adjust these behaviors for special use cases, such as Terraform AWS Provider configurations that affect all resources and the ability to manage resource tags for resources not managed by Terraform, can be found later in this guide.

For most environments and use cases, this is the typical implementation pattern, whether it be in a standalone Terraform configuration or within a [Terraform Module](https://www.terraform.io/docs/modules/). The Terraform configuration language also enables less repetitive configurations via [variables](https://www.terraform.io/docs/configuration/variables.html), [locals](https://www.terraform.io/docs/configuration/locals.html), or potentially a combination of these, e.g.,

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import {
  VariableType,
  TerraformVariable,
  Fn,
  Token,
  TerraformStack,
} from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Vpc } from "./.gen/providers/aws/vpc";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    /*Terraform Variables are not always the best fit for getting inputs in the context of Terraform CDK.
    You can read more about this at https://cdk.tf/variables*/
    const additionalTags = new TerraformVariable(this, "additional_tags", {
      default: [{}],
      description: "Additional resource tags",
      type: VariableType.map(VariableType.STRING),
    });
    new Vpc(this, "example", {
      tags: Token.asStringMap(
        Fn.merge([
          additionalTags.value,
          {
            Name: "MyVPC",
          },
        ])
      ),
    });
  }
}

```

## Ignoring Changes to Specific Tags

Systems outside of Terraform may automatically interact with the tagging associated with AWS resources. These external systems may be for administrative purposes, such as a Configuration Management Database, or the tagging may be required functionality for those systems, such as Kubernetes. This section shows methods to prevent Terraform from showing differences for specific tags.

### Ignoring Changes in Individual Resources

All Terraform resources support the [`lifecycle` configuration block `ignore_changes` argument](https://www.terraform.io/docs/configuration/meta-arguments/lifecycle.html#ignore_changes), which can be used to explicitly ignore all tags changes on a resource beyond an initial configuration or individual tag values.

In this example, the `Name` tag will be added to the VPC on resource creation, however any external changes to the `Name` tag value or the addition/removal of any tag (including the `Name` tag) will be ignored:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Vpc } from "./.gen/providers/aws/vpc";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new Vpc(this, "example", {
      lifecycle: {
        ignoreChanges: [tags],
      },
      tags: {
        Name: "MyVPC",
      },
    });
  }
}

```

In this example, the `Name` and `Owner` tags will be added to the VPC on resource creation, however any external changes to the value of the `Name` tag will be ignored while any changes to other tags (including the `Owner` tag and any additions) will still be proposed:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Vpc } from "./.gen/providers/aws/vpc";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new Vpc(this, "example", {
      lifecycle: {
        ignoreChanges: [name],
      },
      tags: {
        Name: "MyVPC",
        Owner: "Operations",
      },
    });
  }
}

```

### Ignoring Changes in All Resources

As of version 2.60.0 of the Terraform AWS Provider, there is support for ignoring tag changes across all resources under a provider. This simplifies situations where certain tags may be externally applied more globally and enhances functionality beyond `ignore_changes` to support cases such as tag key prefixes.

In this example, all resources will ignore any addition of the `LastScanned` tag:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { AwsProvider } from "./.gen/providers/aws/provider";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new AwsProvider(this, "aws", {
      ignoreTags: [
        {
          keys: ["LastScanned"],
        },
      ],
    });
  }
}

```

In this example, all resources will ignore any addition of tags with the `kubernetes.io/` prefix, such as `kubernetes.io/cluster/name` or `kubernetes.io/role/elb`:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { AwsProvider } from "./.gen/providers/aws/provider";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new AwsProvider(this, "aws", {
      ignoreTags: [
        {
          keyPrefixes: ["kubernetes.io/"],
        },
      ],
    });
  }
}

```

Any of the `ignoreTags` configurations can be combined as needed.

The provider ignore tags configuration applies to all Terraform AWS Provider resources under that particular instance (the `default` provider instance in the above cases). If multiple, different Terraform AWS Provider configurations are being used (e.g., [multiple provider instances](https://www.terraform.io/docs/configuration/providers.html#alias-multiple-provider-instances)), the ignore tags configuration must be added to all applicable provider configurations.

## Managing Individual Resource Tags

Certain Terraform AWS Provider services support a special resource for managing an individual tag on a resource without managing the resource itself. One example is the [`aws_ec2_tag` resource](/docs/providers/aws/r/ec2_tag.html). These resources enable tagging where resources are created outside Terraform such as EC2 Images (AMIs), shared across accounts via Resource Access Manager (RAM), or implicitly created by other means such as EC2 VPN Connections implicitly creating a taggable EC2 Transit Gateway VPN Attachment.

~> **NOTE:** This is an advanced use case and can cause conflicting management issues when improperly implemented. These individual tag resources should not be combined with the Terraform resource for managing the parent resource. For example, using `aws_vpc` and `aws_ec2_tag` to manage tags of the same VPC will cause a perpetual difference where the `aws_vpc` resource will try to remove the tag being added by the `aws_ec2_tag` resource.

-> Not all services supported by the Terraform AWS Provider implement these resources. Browse the Terraform AWS Provider resource documentation pages for a resource with a type ending in `_tag`. If there is a use case where this type of resource is missing, a [feature request](https://github.com/hashicorp/terraform-provider-aws/issues/new?labels=enhancement&template=Feature_Request.md) can be submitted.

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Ec2Tag } from "./.gen/providers/aws/ec2-tag";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new Ec2Tag(this, "example", {
      key: "Owner",
      resourceId: Token.asString(
        awsVpnConnectionExample.transitGatewayAttachmentId
      ),
      value: "Operations",
    });
  }
}

```

To manage multiple tags for a resource in this scenario, [`for_each`](https://www.terraform.io/docs/configuration/meta-arguments/for_each.html) can be used:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, TerraformIterator, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Ec2Tag } from "./.gen/providers/aws/ec2-tag";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    /*In most cases loops should be handled in the programming language context and 
    not inside of the Terraform context. If you are looping over something external, e.g. a variable or a file input
    you should consider using a for loop. If you are looping over something only known to Terraform, e.g. a result of a data source
    you need to keep this like it is.*/
    const exampleForEachIterator = TerraformIterator.fromList(
      Token.asAny("[object Object]")
    );
    new Ec2Tag(this, "example", {
      key: exampleForEachIterator.key,
      resourceId: Token.asString(
        awsVpnConnectionExample.transitGatewayAttachmentId
      ),
      value: exampleForEachIterator.value,
      forEach: exampleForEachIterator,
    });
  }
}

```

The inline map provided to `for_each` in the example above is used for brevity, but other Terraform configuration language features similar to those noted at the beginning of this guide can be used to make the example more extensible.

### Propagating Tags to All Resources

As of version 3.38.0 of the Terraform AWS Provider, the Terraform Configuration language also enables provider-level tagging as an alternative to the methods described in the [Getting Started with Resource Tags](#getting-started-with-resource-tags) section above.
This functionality is available for all Terraform AWS Provider resources that currently support `tags`, with the exception of the [`aws_autoscaling_group`](/docs/providers/aws/r/autoscaling_group.html.markdown) resource. Refactoring the use of [variables](https://www.terraform.io/docs/configuration/variables.html) or [locals](https://www.terraform.io/docs/configuration/locals.html) may look like:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { AwsProvider } from "./.gen/providers/aws/provider";
import { Vpc } from "./.gen/providers/aws/vpc";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new AwsProvider(this, "aws", {
      defaultTags: [
        {
          tags: {
            Environment: "Production",
            Owner: "Ops",
          },
        },
      ],
    });
    new Vpc(this, "example", {
      tags: {
        Name: "MyVPC",
      },
    });
  }
}

```

In this example, the `Environment` and `Owner` tags defined within the provider configuration block will be added to the VPC on resource creation, in addition to the `Name` tag defined within the VPC resource configuration.
To access all the tags applied to the VPC resource, use the read-only attribute `tagsAll`, e.g., `aws_vpc.example.tags_all`.

<!-- cache-key: cdktf-0.20.0 input-d76ea6fe81ad5c47295d3452558feb86a504b39383ae3e8bd97aba4cffd8dba7 -->
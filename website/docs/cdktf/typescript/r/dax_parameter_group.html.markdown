---
subcategory: "DynamoDB Accelerator (DAX)"
layout: "aws"
page_title: "AWS: aws_dax_parameter_group"
description: |-
  Provides an DAX Parameter Group resource.
---


<!-- Please do not edit this file, it is generated. -->
# Resource: aws_dax_parameter_group

Provides a DAX Parameter Group resource.

## Example Usage

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { DaxParameterGroup } from "./.gen/providers/aws/dax-parameter-group";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new DaxParameterGroup(this, "example", {
      name: "example",
      parameters: [
        {
          name: "query-ttl-millis",
          value: "100000",
        },
        {
          name: "record-ttl-millis",
          value: "100000",
        },
      ],
    });
  }
}

```

## Argument Reference

This resource supports the following arguments:

* `name` – (Required) The name of the parameter group.

* `description` - (Optional, ForceNew) A description of the parameter group.

* `parameters` – (Optional) The parameters of the parameter group.

## parameters

`parameters` supports the following:

* `name` - (Required) The name of the parameter.
* `value` - (Required) The value for the parameter.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `id` - The name of the parameter group.

## Import

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import DAX Parameter Group using the `name`. For example:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
  }
}

```

Using `terraform import`, import DAX Parameter Group using the `name`. For example:

```console
% terraform import aws_dax_parameter_group.example my_dax_pg
```

<!-- cache-key: cdktf-0.20.0 input-3baebb2b38580e9504f432df3ad10f9401086de59d9fede3a77fa269c4b381fa -->
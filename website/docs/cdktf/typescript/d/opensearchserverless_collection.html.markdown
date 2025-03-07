---
subcategory: "OpenSearch Serverless"
layout: "aws"
page_title: "AWS: aws_opensearchserverless_collection"
description: |-
  Terraform data source for managing an AWS OpenSearch Serverless Collection.
---


<!-- Please do not edit this file, it is generated. -->
# Data Source: aws_opensearchserverless_collection

Terraform data source for managing an AWS OpenSearch Serverless Collection.

## Example Usage

### Basic Usage

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { DataAwsOpensearchserverlessCollection } from "./.gen/providers/aws/data-aws-opensearchserverless-collection";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new DataAwsOpensearchserverlessCollection(this, "example", {
      name: "example",
    });
  }
}

```

## Argument Reference

The following arguments are required:

* `id` - (Required) ID of the collection. Either `id` or `name` must be provided.
* `name` - (Required) Name of the collection. Either `name` or `id` must be provided.

## Attribute Reference

This data source exports the following attributes in addition to the arguments above:

* `arn` - Amazon Resource Name (ARN) of the collection.
* `collectionEndpoint` - Collection-specific endpoint used to submit index, search, and data upload requests to an OpenSearch Serverless collection.
* `createdDate` - Date the Collection was created.
* `dashboardEndpoint` - Collection-specific endpoint used to access OpenSearch Dashboards.
* `description` - Description of the collection.
* `kmsKeyArn` - The ARN of the Amazon Web Services KMS key used to encrypt the collection.
* `lastModifiedDate` - Date the Collection was last modified.
* `standbyReplicas` - Indicates whether standby replicas should be used for a collection.
* `tags` - A map of tags to assign to the collection.
* `type` - Type of collection.

<!-- cache-key: cdktf-0.20.0 input-79e6c503e86fe55805958b61d9a2c695b0a69c2339520c66ae6030c95e6cff54 -->
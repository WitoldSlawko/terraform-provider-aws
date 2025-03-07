---
subcategory: "DMS (Database Migration)"
layout: "aws"
page_title: "AWS: aws_dms_event_subscription"
description: |-
  Provides a DMS (Data Migration Service) event subscription resource.
---


<!-- Please do not edit this file, it is generated. -->
# Resource: aws_dms_event_subscription

Provides a DMS (Data Migration Service) event subscription resource.

## Example Usage

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { DmsEventSubscription } from "./.gen/providers/aws/dms-event-subscription";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    new DmsEventSubscription(this, "example", {
      enabled: true,
      eventCategories: ["creation", "failure"],
      name: "my-favorite-event-subscription",
      snsTopicArn: Token.asString(awsSnsTopicExample.arn),
      sourceIds: [
        Token.asString(awsDmsReplicationTaskExample.replicationTaskId),
      ],
      sourceType: "replication-task",
      tags: {
        Name: "example",
      },
    });
  }
}

```

## Argument Reference

This resource supports the following arguments:

* `name` - (Required) Name of event subscription.
* `enabled` - (Optional, Default: true) Whether the event subscription should be enabled.
* `eventCategories` - (Optional) List of event categories to listen for, see `DescribeEventCategories` for a canonical list.
* `sourceType` - (Required) Type of source for events. Valid values: `replication-instance` or `replication-task`
* `sourceIds` - (Required) Ids of sources to listen to.
* `snsTopicArn` - (Required) SNS topic arn to send events on.
* `tags` - (Optional) Map of resource tags to assign to the resource. If configured with a provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `arn` - Amazon Resource Name (ARN) of the DMS Event Subscription.
* `tagsAll` - A map of tags assigned to the resource, including those inherited from the provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).

## Timeouts

[Configuration options](https://developer.hashicorp.com/terraform/language/resources/syntax#operation-timeouts):

- `create` - (Default `10m`)
- `update` - (Default `10m`)
- `delete` - (Default `10m`)

## Import

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import event subscriptions using the `name`. For example:

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

Using `terraform import`, import event subscriptions using the `name`. For example:

```console
% terraform import aws_dms_event_subscription.test my-awesome-event-subscription
```

<!-- cache-key: cdktf-0.20.0 input-7ab29fa8b2cfaf3933b10cc81fb97251c9b7fed17847deff37a767a43abec6cb -->
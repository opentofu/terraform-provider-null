---

<!-- Please do not edit this file, it is generated. -->
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "null_resource Resource - terraform-provider-null"
subcategory: ""
description: |-
  The null_resource resource implements the standard resource lifecycle but takes no further action. On Terraform 1.4 and later, use the terraform_data resource type https://developer.hashicorp.com/terraform/language/resources/terraform-data instead. Terraform 1.9 and later support the moved configuration block from null_resource to terraform_data.
  The triggers argument allows specifying an arbitrary set of values that, when changed, will cause the resource to be replaced.
---

# null_resource

The `null_resource` resource implements the standard resource lifecycle but takes no further action. On Terraform 1.4 and later, use the [`terraform_data` resource type](https://developer.hashicorp.com/terraform/language/resources/terraform-data) instead. Terraform 1.9 and later support the `moved` configuration block from `null_resource` to `terraform_data`.

The `triggers` argument allows specifying an arbitrary set of values that, when changed, will cause the resource to be replaced.

## Example Usage

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Token, TerraformCount, Fn, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Resource } from "./.gen/providers/null/resource";
import { Instance } from "./.gen/providers/aws/instance";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    /*In most cases loops should be handled in the programming language context and 
    not inside of the Terraform context. If you are looping over something external, e.g. a variable or a file input
    you should consider using a for loop. If you are looping over something only known to Terraform, e.g. a result of a data source
    you need to keep this like it is.*/
    const clusterCount = TerraformCount.of(Token.asNumber("3"));
    const cluster = new Instance(this, "cluster", {
      ami: "ami-0dcc1e21636832c5d",
      instanceType: "m5.large",
      count: clusterCount,
    });
    const nullProviderResourceCluster = new Resource(this, "cluster_1", {
      connection: {
        host: Fn.element(Fn.lookupNested(cluster, ["*", "public_ip"]), 0),
      },
      triggers: [
        {
          cluster_instance_ids: Fn.join(
            ",",
            Token.asList(Fn.lookupNested(cluster, ["*", "id"]))
          ),
        },
      ],
      provisioners: [
        {
          type: "remote-exec",
          inline: [
            "bootstrap-cluster.sh " +
              Token.asString(
                Fn.join(
                  " ",
                  Token.asList(Fn.lookupNested(cluster, ["*", "private_ip"]))
                )
              ),
          ],
        },
      ],
    });
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    nullProviderResourceCluster.overrideLogicalId("cluster");
  }
}

```

<!-- schema generated by tfplugindocs -->
## Schema

### Optional

- `triggers` (Map of String) A map of arbitrary strings that, when changed, will force the null resource to be replaced, re-running any associated provisioners.

### Read-Only

- `id` (String) This is set to a random value at create time.


<!-- cache-key: cdktf-0.20.8 input-89ea6e8dae79e8917aacba471b3635623c77f324236527d549276a55d3fa9c13 -->
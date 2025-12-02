+++
date = 2025-12-02
title = "terraforming snowflake: module exploration"
description = "An investigation into how to best use modules to provision Snowflake objects."
tags = ["terraform", "snowflake"]
+++

I had the privilege of working with a terraformed Snowflake instance at a prior role.  It's been a few years since that job, however that Snowflake instance has always stuck with me.
It had a great level of abstraction where folks with limited working knowledge of `terraform` could still easily get resources spun up or modified (shoutout [Atlantis](https://www.runatlantis.io/)).  And even if you dug a bit deeper past the `.yml` files, the corresponding `.tf` files were easy to read.

In contrast, I've seen a number of Snowflake accounts since then and dealing with objects was always... _messy_ without terraform.  Inconsistent configs, manually created objects all over the place, or latent Snowflake objects just waiting to create a data disaster.

This article is a bid to make terraforming Snowflake a regular part of my workflow.

## Modules Overview
What is a terraform module?  In Hashicorp's (HC) words
> A module is a collection of resources that Terraform manages together.

When I first read that line, I wasn't _quite_ sure how to best approach using modules in my Snowflake account.  My first thought was to make a warehouse module that could provide some standard best-practice configs for folks.  That was until I came across a **very** clear line in HC's docs:

> We do not recommend writing modules that are just thin wrappers around single other resource types.

**Whoops**.  There goes my idea of making a thin wrapper around warehouses.  I spent some time wondering then what resources might go well together? In this market, controlling costs is priority one for a lot of companies.  So what better way to provision new warehouses than by automatically bundling them with resource monitors?

## Module File Structure
I wanted to keep my module implementation as simple as possible to start.  The below tree lives inside of my broader `/terraform` directory:

```bash
└── warehouse
    ├── README.md
    ├── main.tf
    └── variables.tf
```
`main.tf` contains the module definition.  `variables.tf` contains variable declarations and these variables are ultimately exposed to the user so they can provision warehouses as needed.  Just as important, some configurations are _not_ exposed to the user to make sure we're following best practices!  The `README.md` is a nice touch which contains any pointers about using the module.

### `main.tf` Setup

```bash
terraform {
  required_providers {
    snowflake = {
      source  = "snowflakedb/snowflake"
    }
  }
}
```

Our module starts off with a provider declaration.  In this case, we'll be using `snowflakedb/snowflake` plugin.

From here, we can configure both our resource monitor and warehouse:

```bash
resource "snowflake_resource_monitor" "this" {
  name                      = upper("${var.warehouse_name}_resource_monitor")
  credit_quota              = 7
  frequency                 = "monthly"
  start_timestamp           = "immediately"
  suspend_immediate_trigger = 100
  notify_users              = [upper(var.account_admin_user)]
}

resource "snowflake_warehouse" "this" {
  name           = upper(var.warehouse_name)
  warehouse_size = var.warehouse_size
  comment        = "${var.warehouse_comment}. Warehouse is managed via Terraform."

  auto_suspend        = 60
  initially_suspended = true

  resource_monitor = snowflake_resource_monitor.this.name
}
```

The above highlights some cool features:
- We can enforce that all warehouses auto-suspend at 60 seconds.
- User entered comments are married with a quick note that the warehouse is managed via Terraform.
- We'll enforce that all warehouses have a monthly quota of 7 credits
- Resource monitor names correspond to the warehouse name via `{var.warehouse_name}`

Naturally, warehouses provisioned in a production/enterprise setting would come with a entire extra set of configs.  However this is for my personal Snowflake instance and this module falls squarely in the bucket of 'tinkering'.

### Using the module
Using a module is much more trivial than creating one.  The syntax is short, easy to digest, and removes a lot of decision making for the person making the warehouse:

```bash
module "compute_wh" {
  source = "./modules/warehouse"

  warehouse_name      = "compute_wh"
  warehouse_comment   = "Primary compute warehouse."
  account_admin_user  = var.account_admin_user
}
```

### So What Are We Solving?
As alluded to in the intro, modules help to ensure that resources are:
1. Provisioned using best practice configurations
2. Automatically paired with complementary Snowflake objects

I'd also argue that reading a `terraform` repo is MUCH easier than running a bunch of disparate `show` commands in Snowflake to try and grapple with your setup.

### Pros and Cons
Overall I was quite pleased with this first pass at modules.  Implementation wasn't too complex and the code is quite readable.  For a con, I'm not sure there's a graceful way to handle new module instantiations and always having to run `terraform init` over and over.  Maybe this is part of a normal developer workflow?  I'll be exploring more here.

## Outro
The primary goal of this exercise was to get modules working quickly.  This was a very cool exercise and you can easily see how a central infra team can enforce best practices all the while enabling less technical folks to provision what they need quickly!

# Terraform AWS Lambda Example

Example of how you can deploy and manage AWS Lambda with Terraform.

It includes fully isolated infrastructure and app
deployment, which unlocks the possibility of splitting
infrastructure and application code to different repositories
and manage their deployments separately.

This also means that it is possible to only deploy whatever got
changed instead of rebuilding all functions after slightly
modifying infrastructure configuration. 

Example here includes aggregator Makefile, that combines
deployment procedure into one command with flags to disable
deployment of application or infrastructure (by default, both
will be deployed).

Every Makefile includes default help target, which can be shown
by executing `make` or `make help`.

## Prerequisites

Required tools:

* GNU Make
* zip
* Go
* Terraform
* AWS CLI

You also need to have working Terraform setup for your cloud account.

That means:

* You have configured Terraform backend (I am using S3).
* You have created a role/user for Terraform to use, or any 
  other mechanism to allow Terraform manage your infrastructure.

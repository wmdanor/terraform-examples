# Terraform AWS Lambda Example

Example of how you can deploy and manage AWS Lambda with Terraform.

It includes fully isolated infrastructure and app
deployment, which unlocks the possibility of splitting
infrastructure and application code to different repositories
and manage their deployments separately.

This also means that it is possible to only deploy whatever got
changed instead of rebuilding all functions after slightly
modifying infrastructure configuration. 

Every Makefile includes default help target, which can be shown
by executing `make` or `make help`.

Example here includes aggregator Makefile, that combines
deployment procedure into one command with flags to disable
deployment of application or infrastructure (by default, both
will be deployed).

It is higly advisable to use paralellisation for deploying
functions code using `--jobs` option
(https://www.gnu.org/software/make/manual/html_node/Parallel.html),
as `aws lambda update-function-code` for each function will be
executed in sequence otherwise, which is unnecessary slowdown.

Using parallelisation is straightforward, when using application
Makefile directly, but has some caveats, if you are going to use
aggregator Makefile as it is done in this example, read
[this](https://www.gnu.org/software/make/manual/html_node/Options_002fRecursion.html)
for more details. Generally, it should not be a problem, this is
something you need to keep in mid.

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

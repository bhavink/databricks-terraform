# Databricks Terraform Provider

[![Build Status](https://travis-ci.org/databrickslabs/databricks-terraform.svg?branch=master)](https://travis-ci.org/databrickslabs/databricks-terraform)

## Quickstart: Building and Using the Provider

### Quick install

To quickly install the binary please execute the following curl command in your shell.

```bash
$ curl https://raw.githubusercontent.com/databrickslabs/databricks-terraform/master/godownloader-databricks-provider.sh | bash -s -- -b $HOME/.terraform.d/plugins
```

The command should have moved the binary into your `~/.terraform.d/plugins` folder.

You can `ls` the previous directory to verify. 

### Install using go 

Please note that there is a Makefile which contains all the commands you would need to run this project.

This code base to contribute to requires the following software:

* [golang 1.13.X](https://golang.org/dl/)
* [terraform v0.12.x](https://www.terraform.io/downloads.html)
* make command

To make sure everything is installed correctly please run the following commands:

Testing go installation:
```bash
$ go version 
go version go1.13.3 darwin/amd64
```

Testing terraform installation:
```bash
$ terraform --version
Terraform v0.12.19

Your version of Terraform is out of date! The latest version
is 0.12.24. You can update by downloading from https://www.terraform.io/downloads.html

```

Testing make installation:
```bash
$ make --version
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.

This program built for i386-apple-darwin11.3.0
```

### Downloading the source code and installing the artifact

* After installing `golang`, `terraform`, and `make` you will now build the artifact.

```bash
$ go get -v -u github.com/databrickslabs/databricks-terraform && cd $GOPATH/src/github.com/databrickslabs/databricks-terraform 
```

:warning: If you are fetching from a private repository please use the following command:

```bash
$ GOSUMDB=off GOPROXY=direct go get -v -u github.com/databrickslabs/databricks-terraform && cd $GOPATH/src/github.com/databrickslabs/databricks-terraform
```

* When you are in the root directory of the repository please run:

```bash
$ make build
```

* Locate your [terraform plugins directory](https://www.terraform.io/docs/extend/how-terraform-works.html#plugin-locations) 
    or the root folder of your terraform code

* Copy the `terraform-provider-databricks` artifact to that terraform plugins locations

```bash
$ mkdir -p ~/.terraform.d/plugins/ && cp terraform-provider-databricks ~/.terraform.d/plugins/terraform-provider-databricks
``` 

Now your plugin for the Databricks Terraform provider is installed correctly. You can actually use the provider. 

### Basic Terraform example

Sample terraform code

```terraform
provider "databricks" {
  host = "http://databrickshost.com"
  token = "dapitokenhere"
}

// Creating a basic user
resource "databricks_scim_user" "my-user" {
  user_name = join("", ["test-user", "+",count.index,"@databricks.com"])
  display_name = "Test User"
}
```

Then run `terraform init` then `terraform apply` to apply the hcl code to your databricks workspace.

Please refer to the detailed documentation provided in the html documentation for detailed use of the providers.

Also refer to these [examples](examples/) for more scenarios. 

### Provider Documentation

Provider documentation can be located in the releases tab and documentation is packaged up along with 
the binary of choice.

### Docker commands

To install and build the code if you dont want to install golang, terraform, etc. All you need is docker and git.

First make sure you clone the repository and you are in the directory.

Then build the docker image with this command:

```bash
$ docker build -t databricks-terraform . 
```

Then run the execute the terraform binary via the following command and volume mount. Make sure that you are in the directory
 with the terraform code. The following command you can execute the following commands and additional ones as part of 
 the terraform binary.
 
```bash
$ docker run -it -v $(pwd):/workpace -w /workpace databricks-terraform init
$ docker run -it -v $(pwd):/workpace -w /workpace databricks-terraform plan
$ docker run -it -v $(pwd):/workpace -w /workpace databricks-terraform apply

```

## Project Components

### Databricks Terraform Provider Resources State

| Resource                         | Implemented        | Import Support       | Acceptance Tests     | Documentation        | Reviewed             | Finalize Schema      |
|----------------------------------|--------------------|----------------------|----------------------|----------------------|----------------------|----------------------|
| databricks_token                 | :white_check_mark: | :white_large_square: | :white_check_mark:   | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_secret_scope          | :white_check_mark: | :white_large_square: | :white_check_mark:   | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_secret                | :white_check_mark: | :white_large_square: | :white_check_mark:   | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_secret_acl            | :white_check_mark: | :white_large_square: | :white_check_mark:   | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_instance_pool         | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_scim_user             | :white_check_mark: | :white_large_square: | :white_check_mark:   | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_scim_group            | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_notebook              | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_cluster               | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_job                   | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_dbfs_file             | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_dbfs_file_sync        | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_instance_profile      | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_aws_s3_mount          | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_azure_blob_mount      | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_azure_adls_gen1_mount | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |
| databricks_azure_adls_gen2_mount | :white_check_mark: | :white_large_square: | :white_large_square: | :white_check_mark:   | :white_large_square: | :white_large_square: |

### Databricks Terraform Data Sources State

| Data Source                 | Implemented          | Acceptance Tests     | Documentation        | Reviewed             |
|-----------------------------|----------------------|----------------------|----------------------|----------------------|
| databricks_notebook         | :white_check_mark:   | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_notebook_paths   | :white_check_mark:   | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_dbfs_file        | :white_check_mark:   | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_dbfs_file_paths  | :white_check_mark:   | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_zones            | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_runtimes         | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_instance_pool    | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_scim_user        | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_scim_group       | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_cluster          | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_job              | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_mount            | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_instance_profile | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_database         | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |
| databricks_table            | :white_large_square: | :white_large_square: | :white_large_square: | :white_large_square: |


## Testing

* [ ] Integration tests should be run at a client level against both azure and aws to maintain sdk parity against both apis **(currently only on one cloud)**
* [ ] Terraform acceptance tests should be run against both aws and azure to maintain parity of provider between both cloud services **(currently only on one cloud)**

## Project Support
Please note that all projects in the /databrickslabs github account are provided for your exploration only, and are not formally supported by Databricks with Service Level Agreements (SLAs).  They are provided AS-IS and we do not make any guarantees of any kind.  Please do not submit a support ticket relating to any issues arising from the use of these projects.

Any issues discovered through the use of this project should be filed as GitHub Issues on the Repo.  They will be reviewed as time permits, but there are no formal SLAs for support.

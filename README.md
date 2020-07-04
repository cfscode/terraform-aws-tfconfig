# Terraform AWS tfconfig

This script is a companion to [terraform-aws-bootstrap](https://github.com/vexingcodes/terraform-aws-bootstrap). It
allows a `.tfconfig` file to be created that this script will read and use to create a Terraform file that configures
the AWS provider(s) and backend appropriately. The `.tfconfig` file can be checked into source control so a Terraform
user doesn't need to remember anything in order to configure Terraform other than their access key and secret key.

# Dependencies

The `tfconfig` script assumes that the AWS CLI and `jq` are installed. Their installation varies by operating system and
is beyond the scope of this document.

# Usage

Make a file called `.tfconfg` with a format as described below, then run the `tfconfig` script from the directory
containing the `.tfconfig` file. The `tfconfig` script will read the `.tfconfig` file, read the Amazon Secrets Manager
secret specified by the `.tfconfig` file, and then use the information in the secret to create a Terraform file (default
`config.tf`) that specifies the AWS `provider` and `backend` Terraform blocks. The Amazon Secrets Manager secret is
created by the `boostrap` module noted above.

# Example `.tfconfig` File

```json
{
  "key": "example.tfstate",
  "profile": "default",
  "secret": "terraform",
  "target": "config.tf",
  "extra_regions": [
    "us-east-2",
    "us-west"
  ]
}
```

# Field Details

* `key` -- The S3 blob to read for state storage. Usually ends in .tfstate. Required.
* `profile` -- The AWS CLI profile name to use. Optional, defaults to `default`.
* `secret` -- The name of the secret to read to get the configuration values. Optional, defaults to `terraform`.
* `target` -- The name of the terraform file to write. Optional, defaults to `config.tf`.
* `extra_regions` -- An array of region names. Each region gets its own provider block where the provider alias is the
  same as the region name. Optional, defaults to `[]`.

# Common Usage

The simplest .tfconfig file is:

```json
{
  "key": "mykey.tfstate"
}
```

This should likely be the most common usage. More advanced options should rarely be required.

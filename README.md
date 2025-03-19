# Steampipe Setup Guide
https://aws.amazon.com/ko/blogs/infrastructure-and-automation/simplify-sql-queries-to-aws-api-operations-using-steampipe-and-aws-plugin/

## Pre-Work
### Install AWS CLI
[Official AWS CLI Installation Guide](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions)

```sh
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

### Install Google Cloud CLI
[Official GCloud CLI Installation Guide](https://cloud.google.com/sdk/docs/install?hl=ko#installation_instructions)

```sh
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

## Installing Steampipe
[Download Steampipe](https://steampipe.io/downloads?install=linux)

```sh
sudo /bin/sh -c "$(curl -fsSL https://steampipe.io/install/steampipe.sh)"
```

## Installing Plugins
[Managing Plugins](https://steampipe.io/docs/managing/plugins)

```sh
steampipe plugin install aws
steampipe plugin install gcp
steampipe plugin update --all
```

## Configuration
[Connection Configuration Files](https://steampipe.io/docs/managing/connections#connection-configuration-files)

> Connection configurations are defined using HCL in one or more Steampipe config files. Steampipe will load ALL configuration files from `~/.steampipe/config` that have a `.spc` extension. A config file may contain multiple connections.

### Querying Multiple Connections
[Querying Multiple Connections](https://steampipe.io/docs/managing/connections#querying-multiple-connections)

```hcl
connection "aws_profile" {
  plugin      = "aws"
  profile     = "profile"
  regions     = ["*"]
}

connection "gcp_profile" {
  plugin      = "gcp"
  project     = "profile"
  credentials = "/home/test/credentials.json"
}
```

### Using Aggregators
[Using Aggregators](https://steampipe.io/docs/managing/connections#using-aggregators)

```hcl
connection "aws_all" {
  type        = "aggregator"
  plugin      = "aws"
  connections = ["aws_*"]
}

connection "gcp_all" {
  type        = "aggregator"
  plugin      = "gcp"
  connections = ["gcp_*"]
}
```

## Running Steampipe
```sh
steampipe service start
```
> Default port: `9193`

### (Optional) Open Firewall Port
If your system uses `firewalld`, you may need to open port `9193` for external access:

```sh
firewall-cmd --zone=public --add-port=9193/tcp --permanent
firewall-cmd --reload
```


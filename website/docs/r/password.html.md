---
layout: "random"
page_title: "Random: random_password"
sidebar_current: "docs-random-resource-password"
description: |-
  Produces a random string of a length using alphanumeric characters and optionally special characters. The result will not be displayed to console.
---

# random\_password

~> **Note:** Requires random provider version >= 2.2.0

Identical to [random_string](string.html) with the exception that the
result is treated as sensitive and, thus, _not_ displayed in console output.

~> **Note:** All attributes including the generated password will be stored in
the raw state as plain-text. [Read more about sensitive data in
state](/docs/state/sensitive-data.html).

This resource *does* use a cryptographic random number generator.

## Example Usage

```hcl
resource "random_password" "password" {
  length = 16
  special = true
  override_special = "_%@"
}

resource "aws_db_instance" "example" {
  instance_class = "db.t3.micro"
  allocated_storage = 64
  engine = "mysql"
  username = "someone"
  password = random_password.password.result
}
```
## Import

Random Password can be imported by specifying the value of the string along with the applied settings.
All settings are comma separated key=value 

```
terraform import random_password.password "securepassword length=14"
```

~> *Note*: `override_special` should be the last specified key if specified

All settings from the schema could be provided 

```
terraform import random_password.password "password keepers=nil,length=8,special=true,upper=true,lower=true,number=true,min_numeric=0,min_upper=0,min_lower=0,min_special=0,override_special=_%@"
```

If not specified, then default settings are applied.

```hcl
resource "random_password" "test" {
  length           = 16
  special          = true
  upper            = true
  lower            = true
  number           = true
  min_numeric      = 0
  min_upper        = 0
  min_lower        = 0
  min_special      = 0
  override_special = ""
}
```

~> *Note*: `keepers` will have a default value for a `map[string]interface{}`, `nil`

When importing `keepers`, a jsonString should be then specified without spaces

```
terreform import random_password.test "234567 length=6,special=false,keepers=={\"bla\":\"dibla\",\"key\":\"value\"}"
```

for a matching resource:

```hcl
resource "random_password" "test" {
  keepers          = {
    bla: "dibla"
    key: "value"
  }
  length           = 6
  special          = false
  upper            = true
  lower            = true
  number           = true
  min_numeric      = 0
  min_upper        = 0
  min_lower        = 0
  min_special      = 0
  override_special = ""
}
```
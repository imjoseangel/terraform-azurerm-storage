# terraform-azurerm-storage

[![Terraform](https://github.com/imjoseangel/terraform-azurerm-storage/actions/workflows/terraform.yml/badge.svg)](https://github.com/imjoseangel/terraform-azurerm-storage/actions/workflows/terraform.yml)

## Deploys a Azure Storage Account with versioning enabled, soft-delete capabilities and other security options

This Terraform module deploys a Storage Account on Azure.

### NOTES

* Name Convention specified as `sa<string><randomstring>. <randomstring>` is calculated with `random_string` resource.

## Usage in Terraform 1.0

```terraform
module "storage" {
  source = "github.com/imjoseangel/terraform-azurerm-storage"

  resource_group_name   = var.terraform_rsg
  create_resource_group = false
  location              = var.location
  name                  = var.tf_name
  skuname               = "Standard_ZRS"

  containers_list = [
    { name = "tfstate", access_type = "private" }
  ]

  tags = {
    role        = "terraform"
    environment = "development"
  }

  depends_on = [
    module.keyvault
  ]
}

resource "azurerm_key_vault_secret" "storage" {
  name         = var.tf_name
  value        = module.storage.storage_primary_access_key
  key_vault_id = module.keyvault.id
}
```

## Authors

Originally created by [imjoseangel](http://github.com/imjoseangel)

## License

[MIT](LICENSE)

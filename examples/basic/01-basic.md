# Basic Log Analytics Workspace setup


## Main.tf file content
  Please replace the modules source and version with your relevant information  

```hcl  
// Core modules

## Region Object Definition
module "regions_master" {
  source  = "app.terraform.io/<ORGANIZATION>/regions-master/azurem"
  version = "x.y.z"
  
  azure_region = var.location # example eu-west
}

## Core Tagging Definition
module "base_tagging" {
  source  = "app.terraform.io/<ORGANIZATION>/base-tagging/azurerm"
  version = "x.y.z"

  environment = var.environment
  application = var.application
  spoc =  var.spoc
  change = var.change
  cost_center = var.cost_center
  owner = var.owner
  tlp_colour = var.tlp_colour
  cia_rating = var.cia_rating
  technical_owner = var.technical_owner
}

## Resource Group
module "ressource" {
    source  = "app.terraform.io/<ORGANIZATION>/rg/azurerm"
    version = "x.y.z"

    environment = var.environment
    stack       = var.stack
    default_tags = module.base-tagging.base_tags
    landing_zone_slug = var.landing_zone_slug
    location    = module.regions_master.location
    location_short  = module.regions_master.location_short
}

module "log_analytics_workspace" {
    source  = "app.terraform.io/<ORGANIZATION>/log-analytics-workspace/azurerm"
    version = "x.y.z"

    environment = var.environment
    stack       = var.stack
    default_tags = module.base-tagging.base_tags
    landing_zone_slug = var.landing_zone_slug
    location    = module.regions_master.location
    location_short  = module.regions_master.location_short
}
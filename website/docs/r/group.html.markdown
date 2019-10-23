---
layout: "opennebula"
page_title: "OpenNebula: opennebula_group"
sidebar_current: "docs-opennebula-resource-group"
description: |-
  Provides an OpenNebula group resource.
---

# opennebula_group

Provides an OpenNebula group resource.

This resource allows you to manage groups on your OpenNebula clusters. When applied,
a new group will be created. When destroyed, that group will be removed.

## Example Usage

```hcl
data "template_file" "grptpl" {
  template = "${file("group_template.txt")}"
}

resource "opennebula_group" "group" {
    name = "terraform"
    template = "${data.template_file.grptpl.rendered}"
    delete_on_destruction = true
    quotas {
        datastore {
            datastore_id = 1
            images = 3
            size = 10000
        }
        vm {
            cpu = 3
            running_cpu = 3
            memory = 2048
            running_memory = 2048
        }
        network = {
            network_id = 10
            leases = 6
        }
        network = {
            network_id = 11
            leases = 4
        }
        image = {
            image_id = 8
            running_vms = 1
        }
        image = {
            image_id = 9
            running_vms = 1
        }
    }
}
```

with `group_template.txt` file with Sunstone information:
```php
SUNSTONE = [
  DEFAULT_VIEW = "cloud",
  GROUP_ADMIN_DEFAULT_VIEW = "groupadmin",
  GROUP_ADMIN_VIEWS = "cloud,groupadmin",
  VIEWS = "cloud"
]
```


## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the group.
* `template` - (Required) Group template content in OpenNebula XML or String format. Used to provide SUSNTONE arguments.
* `delete_on_destruction` - (Optional) Flag to delete the group on destruction. Defaults to `false`.
* `admins` - (Optional) List of Administrator user IDs part of the group.
* `quotas` - (Optional) See [Quotas parameters](#quotas) below for details

### Quotas parameters

`quotas` supports the following arguments:

* `datastore` - (Optional) List of datastore quotas. See [Datastore quotas parameters](#datastore-quotas) below for details.
* `network` - (Optional) List of network quotas. See [Network quotas parameters](#network-quotas) below for details.
* `image` - (Optional) List of image quotas. See [Image quotas parameters](#image-quotas) below for details
* `vm` - (Optional) See [Virtual Machine quotas parameters](#vm-quotas) below for details

#### Datastore quotas parameters

`datastore` supports the following arguments:

* `datastore_id` - (Required) Datastore ID.
* `images` - (Optional) Maximum number of images allowed on the datastore. Defaults to `unlimited`
* `size` - (Optional) Total size in MB allowed on the datastore. Defaults to `unlimited`

#### Network quotas parameters

`network` supports the following arguments:

* `network_id` - (Required) Network ID.
* `leases` - (Optional) Maximum number of ip leases allowed on the network. Defaults to `unlimited`

#### Image quotas parameters

`image` supports the following arguments:

* `image_id` - (Required) Image ID.
* `running_vms` - (Optional) Maximum number of Virtual Machines in `RUNNING` state with this image ID attached. Defaults to `unlimited`

#### Virtual Machine quotas parameters

`vm` supports the following arguments:

* `cpu` - (Optional) Maximum number of CPU allowed (in total). Defaults to `unlimited`.
* `memory` - (Optional) Maximum memory (in MB) allowed (in total). Defaults to `unlimited`.
* `vms` - (Optional) Maximum number of Virtual Machines allowed. Defaults to `unlimited`.
* `running_cpu` - (Optional) Number of CPUs of Virtual Machine in `RUNNING` state allowed. Defaults to `unlimited`.
* `running_memory` - (Optional) Memory (in MB) of Virtual Machine in `RUNNING` state allowed. Defaults to `unlimited`.
* `running_vms` - (Optional) Number of Virtual Machines in `RUNNING` state allowed. Defaults to `unlimited`.
* `system_disk_size` - (Optional) Maximum disk size (in MB) on a `SYSTEM` datastore allowed (in total). Defaults to `unlimited`.

## Attribute Reference

The following attribute is exported:
* `id` - ID of the image.

## Import

Not tested yet


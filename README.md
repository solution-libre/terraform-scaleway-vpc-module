<!-- BEGIN_TF_DOCS -->
# Scaleway VPC Terraform module

Terraform module that can be used to deploy VPC resources on Scaleway. Common deployment examples can be found in [examples/](./examples).

## Usage

The example below provision a basic VPC with a Public Gateway and a Load Balancer with some instances behind it.

``` hcl
module "vpc" {
  # to modify
  source  = "scaleway/vpc-module/scaleway"
  version = ">= 1.0.0"

  public_gateway_name  = "my_public_gw"
  private_network_name = "my_vpc"
  zones                = ["fr-par-1"]
}
```

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_scaleway"></a> [scaleway](#requirement\_scaleway) | >= 2.17 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_scaleway"></a> [scaleway](#provider\_scaleway) | >= 2.17 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [scaleway_vpc_gateway_network.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_gateway_network) | resource |
| [scaleway_vpc_private_network.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_private_network) | resource |
| [scaleway_vpc_public_gateway.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_public_gateway) | resource |
| [scaleway_vpc_public_gateway_dhcp.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_public_gateway_dhcp) | resource |
| [scaleway_vpc_public_gateway_ip.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_public_gateway_ip) | resource |
| [scaleway_vpc_public_gateway_ip_reverse_dns.main](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/resources/vpc_public_gateway_ip_reverse_dns) | resource |
| [scaleway_vpc_public_gateway_dhcp_reservation.reservations](https://registry.terraform.io/providers/scaleway/scaleway/latest/docs/data-sources/vpc_public_gateway_dhcp_reservation) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_gateway_dhcp_address"></a> [gateway\_dhcp\_address](#input\_gateway\_dhcp\_address) | IP address of the DHCP server. This will be the gateway's address in the Private Network. Defaults to the first address of the subnet. (IP address) | `string` | `"192.168.0.1"` | no |
| <a name="input_gateway_dhcp_dns_search"></a> [gateway\_dhcp\_dns\_search](#input\_gateway\_dhcp\_dns\_search) | Array of DNS server IP addresses used to override the DNS server list pushed to DHCP clients, instead of the gateway itself | `list(string)` | `null` | no |
| <a name="input_gateway_dhcp_dns_server_servers_override"></a> [gateway\_dhcp\_dns\_server\_servers\_override](#input\_gateway\_dhcp\_dns\_server\_servers\_override) | A list of additional Array of DNS server IP addresses used to override the DNS server list pushed to DHCP clients, instead of the gateway itself. Default the `gateway_dhcp_address` | `list(string)` | `[]` | no |
| <a name="input_gateway_dhcp_enable_dynamic"></a> [gateway\_dhcp\_enable\_dynamic](#input\_gateway\_dhcp\_enable\_dynamic) | Defines whether to enable dynamic pooling of IPs. When false, only pre-existing DHCP reservations will be handed out. Defaults to true | `bool` | `true` | no |
| <a name="input_gateway_dhcp_pool_high"></a> [gateway\_dhcp\_pool\_high](#input\_gateway\_dhcp\_pool\_high) | High IP (inclusive) of the dynamic address pool. Must be in the config's subnet. Defaults to the last address of the subnet. (IP address) | `string` | `"192.168.0.254"` | no |
| <a name="input_gateway_dhcp_pool_low"></a> [gateway\_dhcp\_pool\_low](#input\_gateway\_dhcp\_pool\_low) | Low IP (inclusive) of the dynamic address pool. Must be in the config's subnet. Defaults to the second address of the subnet. (IP address) | `string` | `"192.168.0.2"` | no |
| <a name="input_gateway_dhcp_push_default_route"></a> [gateway\_dhcp\_push\_default\_route](#input\_gateway\_dhcp\_push\_default\_route) | Defines whether the gateway should push a default route to DHCP clients or only hand out IPs. Defaults to true | `bool` | `true` | no |
| <a name="input_gateway_dhcp_push_dns_server"></a> [gateway\_dhcp\_push\_dns\_server](#input\_gateway\_dhcp\_push\_dns\_server) | Defines whether the gateway should push custom DNS servers to clients. This allows for Instance hostname -> IP resolution. Defaults to true. | `bool` | `true` | no |
| <a name="input_gateway_dhcp_rebind_timer"></a> [gateway\_dhcp\_rebind\_timer](#input\_gateway\_dhcp\_rebind\_timer) | After how long a DHCP client will query for a new lease if previous renews fail. Must be 30s lower than `valid_lifetime`. Defaults to 51m (3060s). (in seconds) | `number` | `3060` | no |
| <a name="input_gateway_dhcp_renew_timer"></a> [gateway\_dhcp\_renew\_timer](#input\_gateway\_dhcp\_renew\_timer) | After how long a renew will be attempted. Must be 30s lower than `rebind_timer`. Defaults to 50m (3000s). (in seconds) | `number` | `3000` | no |
| <a name="input_gateway_dhcp_subnet"></a> [gateway\_dhcp\_subnet](#input\_gateway\_dhcp\_subnet) | Subnet for the DHCP server. | `string` | `"192.168.0.0/24"` | no |
| <a name="input_gateway_dhcp_valid_lifetime"></a> [gateway\_dhcp\_valid\_lifetime](#input\_gateway\_dhcp\_valid\_lifetime) | How long DHCP entries will be valid for. Defaults to 1h (3600s). (in seconds | `number` | `3600` | no |
| <a name="input_gateway_network_cleanup_dhcp"></a> [gateway\_network\_cleanup\_dhcp](#input\_gateway\_network\_cleanup\_dhcp) | Defines whether to clean up attached DHCP configurations (if any, and if not attached to another Gateway Network) | `bool` | `true` | no |
| <a name="input_gateway_network_enable_masquerade"></a> [gateway\_network\_enable\_masquerade](#input\_gateway\_network\_enable\_masquerade) | Defines whether the gateway should masquerade traffic for the attached Private Network (i.e. whether to enable dynamic NAT) | `bool` | `true` | no |
| <a name="input_gateway_reverse_dns"></a> [gateway\_reverse\_dns](#input\_gateway\_reverse\_dns) | Create (reserve) a new flexible IP address that can be used for a Public Gateway in a specified Scaleway Project | `bool` | `false` | no |
| <a name="input_instances"></a> [instances](#input\_instances) | A map of interface and/or instance mac addresses containing their properties | `any` | `{}` | no |
| <a name="input_list_reservations"></a> [list\_reservations](#input\_list\_reservations) | Defines whether to list reservations addresses) | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | Name to be used on all the resources as identifier | `string` | `""` | no |
| <a name="input_private_network_ipv4_subnet"></a> [private\_network\_ipv4\_subnet](#input\_private\_network\_ipv4\_subnet) | IPv4 subnet to be used on private network resource | `string` | `"192.168.0.0/24"` | no |
| <a name="input_private_network_name"></a> [private\_network\_name](#input\_private\_network\_name) | Name to be used on private network resource as identifier | `string` | `""` | no |
| <a name="input_public_gateway_bastion_enabled"></a> [public\_gateway\_bastion\_enabled](#input\_public\_gateway\_bastion\_enabled) | Defines whether SSH bastion is enabled on the gateway | `bool` | `true` | no |
| <a name="input_public_gateway_enable_smtp"></a> [public\_gateway\_enable\_smtp](#input\_public\_gateway\_enable\_smtp) | Defines whether SMTP is allowed on the gateway | `bool` | `false` | no |
| <a name="input_public_gateway_name"></a> [public\_gateway\_name](#input\_public\_gateway\_name) | Name to be used on gateway resource as identifier | `string` | `""` | no |
| <a name="input_reverse_dns_zone"></a> [reverse\_dns\_zone](#input\_reverse\_dns\_zone) | Reverse domain name for the IP address | `string` | `false` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | A list of tags to add to all resources | `list(string)` | `[]` | no |
| <a name="input_timeouts"></a> [timeouts](#input\_timeouts) | Define maximum timeout for creating, updating, and deleting VPC resources | `map(string)` | `{}` | no |
| <a name="input_vpc_public_gateway_type"></a> [vpc\_public\_gateway\_type](#input\_vpc\_public\_gateway\_type) | Type to be used on gateway resource as default | `string` | `"VPC-GW-S"` | no |
| <a name="input_vpc_tags"></a> [vpc\_tags](#input\_vpc\_tags) | Additional tags for the VPC | `list(string)` | `[]` | no |
| <a name="input_wait_reservations"></a> [wait\_reservations](#input\_wait\_reservations) | Determines whether wait reservations are available | `bool` | `true` | no |
| <a name="input_zones"></a> [zones](#input\_zones) | A list of availability zones in the region | `list(string)` | `[]` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_dhcp_reservations"></a> [dhcp\_reservations](#output\_dhcp\_reservations) | ID of VPC Gateway DHCP reservations. |
| <a name="output_private_network_id"></a> [private\_network\_id](#output\_private\_network\_id) | ID of private networks. |
| <a name="output_public_gateway_id"></a> [public\_gateway\_id](#output\_public\_gateway\_id) | ID of public gateways. |
| <a name="output_public_gateway_ip_address"></a> [public\_gateway\_ip\_address](#output\_public\_gateway\_ip\_address) | Address of the public gateway IP. |
| <a name="output_public_gateway_ip_id"></a> [public\_gateway\_ip\_id](#output\_public\_gateway\_ip\_id) | ID of gateway IP. |

## Refresh documentation

To create the Readme.md, we use [Terraform-docs](https://terraform-docs.io/). The configuration is in the file `.terraform-docs.yml`.
If you want to refresh the `Readme.md`, from the root of the module execute the following command:

``` shell
terraform-docs .
```
<!-- END_TF_DOCS -->

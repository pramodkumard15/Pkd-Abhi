{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_pkd_name": {
            "defaultValue": "pkd",
            "type": "String"
        },
        "bastionHosts_pkd_Bastion_name": {
            "defaultValue": "pkd-Bastion",
            "type": "String"
        },
        "azureFirewalls_pkd_Firewall_name": {
            "defaultValue": "pkd-Firewall",
            "type": "String"
        },
        "virtualMachines_webapp_nginx_name": {
            "defaultValue": "webapp-nginx",
            "type": "String"
        },
        "publicIPAddresses_pkd_bastion_name": {
            "defaultValue": "pkd-bastion",
            "type": "String"
        },
        "sshPublicKeys_webapp_nginx_key_name": {
            "defaultValue": "webapp-nginx_key",
            "type": "String"
        },
        "publicIPAddresses_pkd_firewall_name": {
            "defaultValue": "pkd-firewall",
            "type": "String"
        },
        "firewallPolicies_pkd_firewall_policy_name": {
            "defaultValue": "pkd-firewall-policy",
            "type": "String"
        },
        "networkInterfaces_webapp_nginx131_z1_name": {
            "defaultValue": "webapp-nginx131_z1",
            "type": "String"
        },
        "networkSecurityGroups_webapp_nginx_nsg_name": {
            "defaultValue": "webapp-nginx-nsg",
            "type": "String"
        },
        "publicIPAddresses_pkd_traffic_management_name": {
            "defaultValue": "pkd-traffic-management",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Compute/sshPublicKeys",
            "apiVersion": "2024-07-01",
            "name": "[parameters('sshPublicKeys_webapp_nginx_key_name')]",
            "location": "eastus",
            "properties": {
                "publicKey": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC77DQwj1woUu+MD4clejL/AaDQpw+JOIgDiRhryBiTCqgvkStlDeKfWdledzuUB/+AU2xWVidIwKLnczFgItfhuYV4VdF4H1ymLrlEUysRxYTekzc4UmVaV7TEgTUjOUwWimC5ZOnL89PEciPjCNQWDpAH8IourI3jVxqoZE8hOgD7FaIWqiJP60No+9s0AF4Xxy5OdOWA/jo3lwNoIIu5jvFB6pBbq+6wiTluJ0ddSf6PcycqXUymvwqZO1TIiyJDt89tCgiHz0fdtcTNkJgX2kDUew2F9HoS9fnxwUwf9fCVCDg0sC6cWTfKl7A2gmkFoLyn4grcX7ubkOpR6MsPtki6mU5ZjNwD2v7zK+wUas/7PEuCImEj/+mrOMDbNTcgBo0onldYj3VX2ecZomdnms9b0ga8ONOIHFms6kJEgr3UcoLPLLcM+x6yGmhn4Bmt6Q5DQI9YBxjA6Y8tMos14g2XMBsNvk1QGVF3s+N9tdJkaB0hatdwa7oEfPMDKA0= generated-by-azure"
            }
        },
        {
            "type": "Microsoft.Network/firewallPolicies",
            "apiVersion": "2024-01-01",
            "name": "[parameters('firewallPolicies_pkd_firewall_policy_name')]",
            "location": "eastus",
            "properties": {
                "sku": {
                    "tier": "Basic"
                },
                "threatIntelMode": "Alert"
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkSecurityGroups_webapp_nginx_nsg_name')]",
            "location": "eastus",
            "properties": {
                "securityRules": [
                    {
                        "name": "SSH",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_webapp_nginx_nsg_name'), 'SSH')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "22",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 300,
                            "direction": "Inbound",
                            "sourcePortRanges": [],
                            "destinationPortRanges": [],
                            "sourceAddressPrefixes": [],
                            "destinationAddressPrefixes": []
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_pkd_bastion_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "properties": {
                "ipAddress": "172.190.192.238",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": [],
                "ddosSettings": {
                    "protectionMode": "VirtualNetworkInherited"
                }
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_pkd_firewall_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "properties": {
                "ipAddress": "52.168.38.34",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_pkd_traffic_management_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "properties": {
                "ipAddress": "52.168.34.195",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2024-01-01",
            "name": "[parameters('virtualNetworks_pkd_name')]",
            "location": "eastus",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "encryption": {
                    "enabled": false,
                    "enforcement": "AllowUnencrypted"
                },
                "subnets": [
                    {
                        "name": "WebApp-Subnet",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'WebApp-Subnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.0.0/24"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "AzureFirewallSubnet",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.1.64/26"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "AzureBastionSubnet",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureBastionSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.1.0/26"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "AzureFirewallManagementSubnet",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallManagementSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.1.128/26"
                            ],
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2024-07-01",
            "name": "[parameters('virtualMachines_webapp_nginx_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_webapp_nginx131_z1_name'))]"
            ],
            "zones": [
                "1"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "Standard_B2ats_v2"
                },
                "additionalCapabilities": {
                    "hibernationEnabled": false
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "canonical",
                        "offer": "0001-com-ubuntu-server-focal",
                        "sku": "20_04-lts-gen2",
                        "version": "latest"
                    },
                    "osDisk": {
                        "osType": "Linux",
                        "name": "[concat(parameters('virtualMachines_webapp_nginx_name'), '_OsDisk_1_d6621434bb864ce19d35d9b619e48872')]",
                        "createOption": "FromImage",
                        "caching": "ReadWrite",
                        "managedDisk": {
                            "storageAccountType": "Premium_LRS",
                            "id": "[resourceId('Microsoft.Compute/disks', concat(parameters('virtualMachines_webapp_nginx_name'), '_OsDisk_1_d6621434bb864ce19d35d9b619e48872'))]"
                        },
                        "deleteOption": "Delete",
                        "diskSizeGB": 30
                    },
                    "dataDisks": [],
                    "diskControllerType": "SCSI"
                },
                "osProfile": {
                    "computerName": "[parameters('virtualMachines_webapp_nginx_name')]",
                    "adminUsername": "azureuser",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": true,
                        "ssh": {
                            "publicKeys": [
                                {
                                    "path": "/home/azureuser/.ssh/authorized_keys",
                                    "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC77DQwj1woUu+MD4clejL/AaDQpw+JOIgDiRhryBiTCqgvkStlDeKfWdledzuUB/+AU2xWVidIwKLnczFgItfhuYV4VdF4H1ymLrlEUysRxYTekzc4UmVaV7TEgTUjOUwWimC5ZOnL89PEciPjCNQWDpAH8IourI3jVxqoZE8hOgD7FaIWqiJP60No+9s0AF4Xxy5OdOWA/jo3lwNoIIu5jvFB6pBbq+6wiTluJ0ddSf6PcycqXUymvwqZO1TIiyJDt89tCgiHz0fdtcTNkJgX2kDUew2F9HoS9fnxwUwf9fCVCDg0sC6cWTfKl7A2gmkFoLyn4grcX7ubkOpR6MsPtki6mU5ZjNwD2v7zK+wUas/7PEuCImEj/+mrOMDbNTcgBo0onldYj3VX2ecZomdnms9b0ga8ONOIHFms6kJEgr3UcoLPLLcM+x6yGmhn4Bmt6Q5DQI9YBxjA6Y8tMos14g2XMBsNvk1QGVF3s+N9tdJkaB0hatdwa7oEfPMDKA0= generated-by-azure"
                                }
                            ]
                        },
                        "provisionVMAgent": true,
                        "patchSettings": {
                            "patchMode": "AutomaticByPlatform",
                            "automaticByPlatformSettings": {
                                "rebootSetting": "IfRequired"
                            },
                            "assessmentMode": "ImageDefault"
                        }
                    },
                    "secrets": [],
                    "allowExtensionOperations": true,
                    "requireGuestProvisionSignal": true
                },
                "securityProfile": {
                    "uefiSettings": {
                        "secureBootEnabled": true,
                        "vTpmEnabled": true
                    },
                    "securityType": "TrustedLaunch"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_webapp_nginx131_z1_name'))]",
                            "properties": {
                                "deleteOption": "Delete"
                            }
                        }
                    ]
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true
                    }
                }
            }
        },
        {
            "type": "Microsoft.Network/firewallPolicies/ruleCollectionGroups",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('firewallPolicies_pkd_firewall_policy_name'), '/DefaultDnatRuleCollectionGroup')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/firewallPolicies', parameters('firewallPolicies_pkd_firewall_policy_name'))]"
            ],
            "properties": {
                "priority": 100,
                "ruleCollections": [
                    {
                        "ruleCollectionType": "FirewallPolicyNatRuleCollection",
                        "action": {
                            "type": "Dnat"
                        },
                        "rules": [
                            {
                                "ruleType": "NatRule",
                                "name": "nginx-rule",
                                "translatedAddress": "10.0.0.4",
                                "translatedPort": "80",
                                "ipProtocols": [
                                    "TCP"
                                ],
                                "sourceAddresses": [
                                    "50.245.22.113"
                                ],
                                "sourceIpGroups": [],
                                "destinationAddresses": [
                                    "52.168.38.34"
                                ],
                                "destinationPorts": [
                                    "100"
                                ]
                            }
                        ],
                        "name": "firewall-ngnix-rule",
                        "priority": 100
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_webapp_nginx_nsg_name'), '/SSH')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_webapp_nginx_nsg_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "22",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 300,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_pkd_name'), '/AzureBastionSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_pkd_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.0.1.0/26"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_pkd_name'), '/AzureFirewallManagementSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_pkd_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.0.1.128/26"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_pkd_name'), '/AzureFirewallSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_pkd_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.0.1.64/26"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_pkd_name'), '/WebApp-Subnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_pkd_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.0.0.0/24"
                ],
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Network/bastionHosts",
            "apiVersion": "2024-01-01",
            "name": "[parameters('bastionHosts_pkd_Bastion_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_bastion_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureBastionSubnet')]"
            ],
            "sku": {
                "name": "Basic"
            },
            "properties": {
                "dnsName": "bst-ce2b65ca-ad30-45dd-a245-c62eb096423c.bastion.azure.com",
                "scaleUnits": 2,
                "ipConfigurations": [
                    {
                        "name": "IpConf",
                        "id": "[concat(resourceId('Microsoft.Network/bastionHosts', parameters('bastionHosts_pkd_Bastion_name')), '/bastionHostIpConfigurations/IpConf')]",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_bastion_name'))]"
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureBastionSubnet')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkInterfaces_webapp_nginx131_z1_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'WebApp-Subnet')]",
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_webapp_nginx_nsg_name'))]"
            ],
            "kind": "Regular",
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "id": "[concat(resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_webapp_nginx131_z1_name')), '/ipConfigurations/ipconfig1')]",
                        "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
                        "properties": {
                            "privateIPAddress": "10.0.0.4",
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'WebApp-Subnet')]"
                            },
                            "primary": true,
                            "privateIPAddressVersion": "IPv4"
                        }
                    }
                ],
                "dnsSettings": {
                    "dnsServers": []
                },
                "enableAcceleratedNetworking": false,
                "enableIPForwarding": false,
                "disableTcpStateTracking": false,
                "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_webapp_nginx_nsg_name'))]"
                },
                "nicType": "Standard",
                "auxiliaryMode": "None",
                "auxiliarySku": "None"
            }
        },
        {
            "type": "Microsoft.Network/azureFirewalls",
            "apiVersion": "2024-01-01",
            "name": "[parameters('azureFirewalls_pkd_Firewall_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_traffic_management_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallManagementSubnet')]",
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_firewall_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallSubnet')]",
                "[resourceId('Microsoft.Network/firewallPolicies', parameters('firewallPolicies_pkd_firewall_policy_name'))]"
            ],
            "properties": {
                "sku": {
                    "name": "AZFW_VNet",
                    "tier": "Basic"
                },
                "threatIntelMode": "Alert",
                "additionalProperties": {},
                "managementIpConfiguration": {
                    "name": "managementIpConfig",
                    "id": "[concat(resourceId('Microsoft.Network/azureFirewalls', parameters('azureFirewalls_pkd_Firewall_name')), '/azureFirewallIpConfigurations/managementIpConfig')]",
                    "properties": {
                        "publicIPAddress": {
                            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_traffic_management_name'))]"
                        },
                        "subnet": {
                            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallManagementSubnet')]"
                        }
                    }
                },
                "ipConfigurations": [
                    {
                        "name": "ipConfig",
                        "id": "[concat(resourceId('Microsoft.Network/azureFirewalls', parameters('azureFirewalls_pkd_Firewall_name')), '/azureFirewallIpConfigurations/ipConfig')]",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_pkd_firewall_name'))]"
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_pkd_name'), 'AzureFirewallSubnet')]"
                            }
                        }
                    }
                ],
                "networkRuleCollections": [],
                "applicationRuleCollections": [],
                "natRuleCollections": [],
                "firewallPolicy": {
                    "id": "[resourceId('Microsoft.Network/firewallPolicies', parameters('firewallPolicies_pkd_firewall_policy_name'))]"
                }
            }
        }
    ]
}

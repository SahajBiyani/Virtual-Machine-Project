# Virtual-Machine-Project

Walkthrough Video: https://www.youtube.com/watch?v=KKWnRdr6NqQ
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachines_VM_VMP_name": {
            "value": null
        },
        "bastionHosts_BASTION_VMP_name": {
            "value": null
        },
        "virtualNetworks_VNET_VMP_name": {
            "value": null
        },
        "publicIPAddresses_VMIP_VMP_name": {
            "value": null
        },
        "sshPublicKeys_VM_VMP_SSHkey_name": {
            "value": null
        },
        "publicIPAddresses_VM_VMP_ip_name": {
            "value": null
        },
        "networkSecurityGroups_NSG_VMP_name": {
            "value": null
        },
        "networkInterfaces_vm_vmp851_z3_name": {
            "value": null
        },
        "publicIPAddresses_BASTION_VMP_ip_name": {
            "value": null
        }
    }
}


{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualMachines_VM_VMP_name": {
            "defaultValue": "VM-VMP",
            "type": "String"
        },
        "bastionHosts_BASTION_VMP_name": {
            "defaultValue": "BASTION-VMP",
            "type": "String"
        },
        "virtualNetworks_VNET_VMP_name": {
            "defaultValue": "VNET-VMP",
            "type": "String"
        },
        "publicIPAddresses_VMIP_VMP_name": {
            "defaultValue": "VMIP-VMP",
            "type": "String"
        },
        "sshPublicKeys_VM_VMP_SSHkey_name": {
            "defaultValue": "VM-VMP_SSHkey",
            "type": "String"
        },
        "publicIPAddresses_VM_VMP_ip_name": {
            "defaultValue": "VM-VMP-ip",
            "type": "String"
        },
        "networkSecurityGroups_NSG_VMP_name": {
            "defaultValue": "NSG-VMP",
            "type": "String"
        },
        "networkInterfaces_vm_vmp851_z3_name": {
            "defaultValue": "vm-vmp851_z3",
            "type": "String"
        },
        "publicIPAddresses_BASTION_VMP_ip_name": {
            "defaultValue": "BASTION-VMP-ip",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Compute/sshPublicKeys",
            "apiVersion": "2024-07-01",
            "name": "[parameters('sshPublicKeys_VM_VMP_SSHkey_name')]",
            "location": "eastus",
            "properties": {
                "publicKey": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCyZ+9rqxDvdfeEdp2YNLDfO1jNUnNBcU43rRjqnnkhD5w6yCWBkwfM1N6TXoTESKRJsa7cpd/+hZ+o54uX4KsLqkCaZ18t+QXPEuCXD8QO2Keyw39YQdOf0Sp3JXP3/IqvWCuXZIDwrlavvngQ3WHniKhvXdGQq+znr/BoSYCAxmCv/yyrTDvxgWbA1p+kbrm1oKxyh8sK99Ky/rv0uRxKO5yxmRV7/8aGqGeZWHcSK4lMsWPbhHWPA+o4f4238aH048xwlD1hmgoP6QdNuF9FXw6oAb6hZc9TQ50sjWTbfjCm5hLuzGY0ryhlDsYlCJ6Q42VCzTORa44uwgSRdy9riHxE6Ui7vMfeoGIMcVlqSdbXGZ7D3ufO+PBOTHVGE1tIceDlglzxlnN9KkWou5px2i5Oetu5L/BWt9A3DOKmFkNx/Rb/9ock6nTwqGkivie6VuicVvOKk7N4ZLOGpZB65df4HUxplua5iv3Ndbb7mwYqffls7KvFA135KgxBLBE= generated-by-azure"
            }
        },
        {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkSecurityGroups_NSG_VMP_name')]",
            "location": "eastus",
            "properties": {
                "securityRules": [
                    {
                        "name": "HTTPS-VMP",
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups/securityRules', parameters('networkSecurityGroups_NSG_VMP_name'), 'HTTPS-VMP')]",
                        "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "443",
                            "sourceAddressPrefix": "104.202.246.237",
                            "destinationAddressPrefix": "10.0.0.4",
                            "access": "Allow",
                            "priority": 100,
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
            "name": "[parameters('publicIPAddresses_BASTION_VMP_ip_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "zones": [
                "1",
                "3",
                "2"
            ],
            "properties": {
                "ipAddress": "51.8.48.221",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_VMIP_VMP_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "properties": {
                "ipAddress": "172.203.158.9",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "dnsSettings": {
                    "domainNameLabel": "beya",
                    "fqdn": "beya.eastus.cloudapp.azure.com"
                },
                "ipTags": [],
                "ddosSettings": {
                    "protectionMode": "VirtualNetworkInherited"
                }
            }
        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2024-01-01",
            "name": "[parameters('publicIPAddresses_VM_VMP_ip_name')]",
            "location": "eastus",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
            "zones": [
                "3"
            ],
            "properties": {
                "ipAddress": "20.84.42.165",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2024-07-01",
            "name": "[parameters('virtualMachines_VM_VMP_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_vm_vmp851_z3_name'))]"
            ],
            "zones": [
                "3"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "Standard_B1s"
                },
                "additionalCapabilities": {
                    "hibernationEnabled": false
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "canonical",
                        "offer": "0001-com-ubuntu-server-jammy",
                        "sku": "22_04-lts-gen2",
                        "version": "latest"
                    },
                    "osDisk": {
                        "osType": "Linux",
                        "name": "[concat(parameters('virtualMachines_VM_VMP_name'), '_disk1_b49d0f3ada0248cabe5253feed15bbb8')]",
                        "createOption": "FromImage",
                        "caching": "ReadWrite",
                        "managedDisk": {
                            "storageAccountType": "Premium_LRS",
                            "id": "[resourceId('Microsoft.Compute/disks', concat(parameters('virtualMachines_VM_VMP_name'), '_disk1_b49d0f3ada0248cabe5253feed15bbb8'))]"
                        },
                        "deleteOption": "Delete",
                        "diskSizeGB": 30
                    },
                    "dataDisks": [],
                    "diskControllerType": "SCSI"
                },
                "osProfile": {
                    "computerName": "[parameters('virtualMachines_VM_VMP_name')]",
                    "adminUsername": "beya",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": true,
                        "ssh": {
                            "publicKeys": [
                                {
                                    "path": "/home/beya/.ssh/authorized_keys",
                                    "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCyZ+9rqxDvdfeEdp2YNLDfO1jNUnNBcU43rRjqnnkhD5w6yCWBkwfM1N6TXoTESKRJsa7cpd/+hZ+o54uX4KsLqkCaZ18t+QXPEuCXD8QO2Keyw39YQdOf0Sp3JXP3/IqvWCuXZIDwrlavvngQ3WHniKhvXdGQq+znr/BoSYCAxmCv/yyrTDvxgWbA1p+kbrm1oKxyh8sK99Ky/rv0uRxKO5yxmRV7/8aGqGeZWHcSK4lMsWPbhHWPA+o4f4238aH048xwlD1hmgoP6QdNuF9FXw6oAb6hZc9TQ50sjWTbfjCm5hLuzGY0ryhlDsYlCJ6Q42VCzTORa44uwgSRdy9riHxE6Ui7vMfeoGIMcVlqSdbXGZ7D3ufO+PBOTHVGE1tIceDlglzxlnN9KkWou5px2i5Oetu5L/BWt9A3DOKmFkNx/Rb/9ock6nTwqGkivie6VuicVvOKk7N4ZLOGpZB65df4HUxplua5iv3Ndbb7mwYqffls7KvFA135KgxBLBE= generated-by-azure"
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
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_vm_vmp851_z3_name'))]",
                            "properties": {
                                "deleteOption": "Detach"
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
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('networkSecurityGroups_NSG_VMP_name'), '/HTTPS-VMP')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_NSG_VMP_name'))]"
            ],
            "properties": {
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "443",
                "sourceAddressPrefix": "104.202.246.237",
                "destinationAddressPrefix": "10.0.0.4",
                "access": "Allow",
                "priority": 100,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2024-01-01",
            "name": "[parameters('virtualNetworks_VNET_VMP_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_NSG_VMP_name'))]"
            ],
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
                        "name": "SNET-VMP",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'SNET-VMP')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.0.0/24"
                            ],
                            "networkSecurityGroup": {
                                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_NSG_VMP_name'))]"
                            },
                            "delegations": [],
                            "privateEndpointNetworkPolicies": "Disabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        },
                        "type": "Microsoft.Network/virtualNetworks/subnets"
                    },
                    {
                        "name": "AzureBastionSubnet",
                        "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'AzureBastionSubnet')]",
                        "properties": {
                            "addressPrefixes": [
                                "10.0.1.0/26"
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
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_VNET_VMP_name'), '/AzureBastionSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_VNET_VMP_name'))]"
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
            "type": "Microsoft.Network/bastionHosts",
            "apiVersion": "2024-01-01",
            "name": "[parameters('bastionHosts_BASTION_VMP_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_BASTION_VMP_ip_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'AzureBastionSubnet')]"
            ],
            "sku": {
                "name": "Standard"
            },
            "properties": {
                "dnsName": "bst-bbf85b7d-68f7-4135-b50c-c48e1b99d66e.bastion.azure.com",
                "scaleUnits": 2,
                "enableTunneling": false,
                "enableIpConnect": false,
                "disableCopyPaste": false,
                "enableShareableLink": false,
                "enableKerberos": false,
                "enableSessionRecording": false,
                "ipConfigurations": [
                    {
                        "name": "IpConf",
                        "id": "[concat(resourceId('Microsoft.Network/bastionHosts', parameters('bastionHosts_BASTION_VMP_name')), '/bastionHostIpConfigurations/IpConf')]",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_BASTION_VMP_ip_name'))]"
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'AzureBastionSubnet')]"
                            }
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2024-01-01",
            "name": "[parameters('networkInterfaces_vm_vmp851_z3_name')]",
            "location": "eastus",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_VMIP_VMP_name'))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'SNET-VMP')]"
            ],
            "kind": "Regular",
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "id": "[concat(resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaces_vm_vmp851_z3_name')), '/ipConfigurations/ipconfig1')]",
                        "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
                        "properties": {
                            "privateIPAddress": "10.0.0.4",
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_VMIP_VMP_name'))]"
                            },
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworks_VNET_VMP_name'), 'SNET-VMP')]"
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
                "nicType": "Standard",
                "auxiliaryMode": "None",
                "auxiliarySku": "None"
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2024-01-01",
            "name": "[concat(parameters('virtualNetworks_VNET_VMP_name'), '/SNET-VMP')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_VNET_VMP_name'))]",
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_NSG_VMP_name'))]"
            ],
            "properties": {
                "addressPrefixes": [
                    "10.0.0.0/24"
                ],
                "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_NSG_VMP_name'))]"
                },
                "delegations": [],
                "privateEndpointNetworkPolicies": "Disabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        }
    ]
}


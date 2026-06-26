# sailpoint
az vm create \
  --resource-group <ResourceGroupName> \
  --name <ServerName> \
  --image MicrosoftWindowsServer:WindowsServer:2022-datacenter-g2:latest \
  --size Standard_D8as_v7 \
  --admin-username azureuser \
  --admin-password VueIsland@123 \
  --location <Location> \
  --os-disk-size-gb 256 \
  --data-disk-sizes-gb 256 256 \
  --vnet-name <Vnet_Name> \
  --subnet <SubnetName> \
  --nsg <NetworkSecurityGroupName> \
  --computer-name <ServerName> \
  --no-wait \
  --public-ip-address ""

==

#!/bin/bash

RESOURCE_GROUP="AZRG-VUE-NE-NPRD-UK"
NSG_NAME="NSG-UKAZ-NETST-EXT-CINOPS05"

# Inbound Rules
az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowApplicationGatewayTraffic --priority 4096 --direction Inbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-range 65200-65535

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowBastionInBound --priority 1120 --direction Inbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-ranges 22 3389

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowDNSInBound --priority 1130 --direction Inbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-ranges 53 88 389 445 135

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowATADATAInBound --priority 1140 --direction Inbound --access Allow --protocol '*' --source-address-prefix 10.147.10.4 --source-port-range '*' --destination-address-prefix '*' --destination-port-ranges 22 3389 135 139 445 49512-65535

# Outbound Rules
az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowBastionOutBound --priority 1110 --direction Outbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-range '*'

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowDNSOutBound --priority 1130 --direction Outbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-ranges 53 88 389 135 445

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowHTTPS --priority 1140 --direction Outbound --access Allow --protocol TCP --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-range 443

az network nsg rule create --resource-group $RESOURCE_GROUP --nsg-name $NSG_NAME --name AllowATADATAOutBound --priority 1150 --direction Outbound --access Allow --protocol '*' --source-address-prefix '*' --source-port-range '*' --destination-address-prefix '*' --destination-port-ranges 22 3389 135 139 445 49512-65535

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

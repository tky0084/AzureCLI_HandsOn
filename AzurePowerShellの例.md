```
$ResourceGroupName = "sample-rg"
$Location          = "japaneast"
$VMName            = "sample-vm"
$VMSize            = "Standard_D2s_v3"

# 既存VNet / Subnet
$VnetName   = "sample-vnet"
$SubnetName = "default"

# 管理者アカウント
$Credential = Get-Credential

New-AzVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $Location `
  -Name $VMName `
  -VMSize $VMSize `
  -VirtualNetworkName $VnetName `
  -SubnetName $SubnetName `
  -Image "Win2022Datacenter" `
  -Credential $Credential
```
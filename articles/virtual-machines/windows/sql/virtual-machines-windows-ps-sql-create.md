---
title: Como criar VMs do SQL Server com o Azure PowerShell | Microsoft Docs
description: "Fornece etapas e comandos do PowerShell para criar uma VM do Azure com imagens da galeria de máquinas virtuais do SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/29/2017
ms.author: jroth
ms.openlocfilehash: e6d1f36d998ac8726e3a74b31772a5dd5a24bd58
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-create-sql-server-virtual-machines-with-azure-powershell"></a>Como criar máquinas virtuais do SQL Server com o Azure PowerShell

Este guia explica as opções para criar VMs do SQL Server do Windows com o Azure PowerShell. Para obter um exemplo do PowerShell do Azure simplificado com mais valores padrão, consulte o [início rápido da VM do SQL do Azure PowerShell](quickstart-sql-vm-create-powershell.md).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Este início rápido requer o módulo Azure PowerShell versão 3.6 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para encontrar a versão. Se você precisa instalar ou atualizar, confira [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="configure-your-subscription"></a>Configurar sua assinatura

1. Abra o PowerShell e estabeleça o acesso à sua conta do Azure executando o comando **Add-AzureRmAccount**.

   ```PowerShell
   Add-AzureRmAccount
   ```

1. Você deve ver uma tela de entrada para inserir suas credenciais. Use o mesmo email e senha usados para entrar no Portal do Azure.

## <a name="define-image-variables"></a>Definir variáveis de imagem
Para simplificar as criações de reutilização e script, inicie com a definição de um número de variáveis. Altere os valores de parâmetro como achar melhor, mas lembre-se de restrições de nomenclatura relacionadas a tamanhos de nome e caracteres especiais ao modificar os valores fornecidos.

### <a name="location-and-resource-group"></a>Local e Grupo de Recursos
Use duas variáveis para definir a região de dados e o grupo de recursos no qual você cria outros recursos para a máquina virtual.

Modifique conforme desejado e, em seguida, execute os seguintes cmdlets para inicializar essas variáveis.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Propriedades de armazenamento
Use as seguintes variáveis para definir a conta de armazenamento e o tipo de armazenamento a ser usado pela máquina virtual.

Modifique conforme desejado e, em seguida, execute o seguinte cmdlet para inicializar essas variáveis. Observe que, neste exemplo estamos usando o [Armazenamento Premium](../premium-storage.md), que é recomendado para cargas de trabalho de produção.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Propriedades da rede
Use as seguintes variáveis para definir a interface de rede, o método de alocação de TCP/IP, o nome da rede virtual, o nome da sub-rede virtual, o intervalo de endereços IP para a rede virtual, o intervalo de endereços IP para a sub-rede e o rótulo de nome de domínio público a ser usado pela rede na máquina virtual.

Modifique conforme desejado e, em seguida, execute o seguinte cmdlet para inicializar essas variáveis.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>Propriedades de máquina virtual
Use as seguintes variáveis para definir o nome da máquina virtual, o nome do computador, o tamanho da máquina virtual e o nome do disco do sistema operacional da máquina virtual.

Modifique conforme desejado e, em seguida, execute o seguinte cmdlet para inicializar essas variáveis.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Escolha uma imagem do SQL Server
Use as seguintes variáveis para definir a imagem do SQL Server a ser usada para a máquina virtual.

1. Primeiro, liste todas as ofertas de imagem do SQL Server com o comando **Get-AzureRmVMImageOffer**:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Para este tutorial, uso as seguintes variáveis para especificar o SQL Server 2017 no Windows Server 2016.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Em seguida, liste as edições disponíveis para sua oferta.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Para este tutorial, use o edição do Desenvolvedor do SQL Server 2017 (**SQLDEV**). A edição Developer é licenciada livremente para teste e desenvolvimento, e você só paga o custo da execução da VM.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Com o modelo de implantação do Gerenciador de Recursos, o primeiro objeto criado por você é o grupo de recursos. Uso o cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) para criar um grupo de recursos do Azure e seus recursos com o nome e localização do grupo de recursos definidos pelas variáveis que você inicializou previamente.

Execute o seguinte cmdlet para criar novo grupo de recursos.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
A máquina virtual exige recursos de armazenamento para o disco do sistema operacional para os arquivos de log de dados do SQL Server. Para simplificar, criamos um único disco para ambos. Você pode anexar discos adicionais posteriormente usando o cmdlet [Adicionar Disco do Azure](/powershell/module/azure/add-azuredisk) para colocar seus dados do SQL Server e o log de arquivos em discos dedicados. Use o cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) para criar uma conta de armazenamento standard em seu novo grupo de recursos e com o nome da conta de armazenamento, um nome de SKU de armazenamento e uma localização definida usando as variáveis inicializadas anteriormente.

Execute o cmdlet a seguir para criar sua nova conta de armazenamento.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Criar uma conta de armazenamento pode demorar alguns minutos.

## <a name="create-network-resources"></a>Criar recursos da rede
A máquina virtual requer um número de recursos de rede para conectividade de rede.

* Cada máquina virtual exige um rede virtual.
* Uma rede virtual deve ter pelo menos uma sub-rede definida.
* Uma interface de rede deve ser definida com um público ou um endereço IP privado.

### <a name="create-a-virtual-network-subnet-configuration"></a>Criar uma configuração de sub-rede da rede virtual
Comece criando uma configuração de sub-rede para nossa rede virtual. Para nosso tutorial, crie uma sub-rede padrão usando o cmdlet [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Criamos nossa configuração de sub-rede da rede virtual com o prefixo de nome e endereço de sub-rede definido usando as variáveis inicializadas anteriormente.

> [!NOTE]
> Você pode definir propriedades adicionais da configuração de sub-rede da rede virtual usando esse cmdlet, mas isso está além do escopo deste tutorial.

Execute o seguinte cmdlet para criar a configuração da sub-rede virtual.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
Em seguida, crie sua rede virtual usando o cmdlet [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Crie a rede virtual no novo grupo de recursos, com o nome, local e endereço de prefixo definido usando as variáveis inicializadas anteriormente e com a configuração de sub-rede que você definiu na etapa anterior.

Execute o seguinte cmdlet para criar a configuração da rede virtual.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Criar um endereço IP público
Agora que temos nossa rede virtual definida, precisamos configurar um endereço IP para conectividade com a máquina virtual. Para este tutorial, criamos um endereço IP público usando endereçamento IP dinâmico para dar suporte à conectividade com a Internet. Use o cmdlet [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) para criar o endereço IP público no grupo de recursos criado anteriormente e com o nome, a localização, o método de alocação e o rótulo de nome de domínio DNS definido usando as variáveis inicializadas anteriormente.

> [!NOTE]
> Você pode definir propriedades adicionais do endereço IP público usando esse cmdlet, mas isso está além do escopo deste tutorial. Você também pode criar um endereço particular ou um endereço com um endereço estático, mas isso também está além do escopo deste tutorial.

Execute o seguinte cmdlet para criar o endereço IP público.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Crie o grupo de segurança de rede
Para proteger o tráfego do SQL Server e da VM, cria um grupo de segurança de rede.

1. Primeiro, crie uma regra de grupo de segurança de rede para RDP para permitir conexões à área de trabalho remota.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. Configure uma regra de grupo de segurança de rede que permita tráfego na porta TCP 1433. Isto habilita conexões ao SQL Server pela internet.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Em seguida, crie o grupo de segurança de rede.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Criar a interface de rede
Agora estamos prontos para criar a interface de rede que nossa máquina virtual usará. Chamamos o cmdlet [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) para criar nossa interface de rede no grupo de recursos criado anteriormente e com o nome, a localização, a sub-rede e o endereço IP público definidos anteriormente.

Execute o seguinte cmdlet para criar sua interface de rede.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Configurar um objeto da VM
Agora que temos recursos de armazenamento e rede definidos, estamos prontos para definir recursos de computação para a máquina virtual. Para nosso tutorial, especificamos várias propriedades do sistema operacional e tamanhos de máquina virtual, especificar a interface de rede criada anteriormente, definir o armazenamento de blobs e, em seguida, especificar o disco do sistema operacional.

### <a name="create-the-vm-object"></a>Criar o objeto da VM
Começamos especificando o tamanho da máquina virtual. Para este tutorial, estamos especificando um DS13. Chamamos o cmdlet [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) para criar um objeto de máquina virtual configurável com o nome e o tamanho definidos usando as variáveis inicializadas anteriormente.

Execute o seguinte cmdlet para criar o objeto da máquina virtual.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Criar um objeto de credencial para armazenar o nome e a senha para as credenciais de administrador local
Antes que possamos definir as propriedades de sistema operacional da máquina virtual, precisamos fornecer as credenciais da conta de administrador local como uma cadeia de caracteres segura. Para fazer isso, usamos o cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx).

Execute o cmdlet a seguir e, na janela de solicitação de credencial do PowerShell, digite o nome e a senha a ser usada para a conta de administrador local na máquina virtual.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Definir as propriedades de sistema operacional da máquina virtual
Agora estamos prontos para configurar as propriedades do sistema operacional da máquina virtual com o cmdlet [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) para definir o tipo de sistema operacional como Windows, para exigir que o [agente da máquina virtual](../agent-user-guide.md) seja instalado, para especificar que o cmdlet habilite a atualização automática e para definir o nome da máquina virtual, o nome do computador e a credencial usando as variáveis que você inicializou anteriormente.

Execute o seguinte cmdlet para definir as propriedades de sistema operacional de sua máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Adicionar a interface de rede à máquina virtual
Em seguida, adicionamos a interface de rede que criamos anteriormente à máquina virtual. Chame o cmdlet [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) para adicionar a interface de rede usando a variável de interface de rede que você definiu anteriormente.

Execute o seguinte cmdlet para definir a interface de rede para sua máquina virtual.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Definir o local de armazenamento de blobs para o disco a ser usado pela máquina virtual
Em seguida, defina o local de armazenamento de blobs para o disco a ser usado pela máquina virtual usando as variáveis que você definiu anteriormente.

Execute o seguinte cmdlet para definir o local de armazenamento de blobs.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Definir as propriedades de disco do sistema operacional para a máquina virtual
Em seguida, defina as propriedades de disco do sistema operacional para a máquina virtual. Use o cmdlet [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) para especificar que o sistema operacional da máquina virtual será proveniente de uma imagem, para definir o caching para somente leitura (pois o SQL Server é instalado no mesmo disco) e para definir o nome da máquina virtual e o disco do sistema operacional definido usando as variáveis que determinamos anteriormente.

Execute o seguinte cmdlet para definir as propriedades de disco do sistema operacional de sua máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Especificar a imagem da plataforma para a máquina virtual
Nossa última etapa da configuração é especificar a imagem de plataforma para nossa máquina virtual. Para nosso tutorial, estamos usando a imagem mais recente do SQL Server 2016 CTP. Use o cmdlet [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) para usar essa imagem, conforme definido pelas variáveis que você definiu anteriormente.

Execute o seguinte cmdlet para especificar a imagem da plataforma para sua máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>Criar a VM de SQL
Agora que você concluiu as etapas de configuração, você está pronto para criar a máquina virtual. Use o cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) para criar a máquina virtual usando as variáveis que definimos.

Execute o seguinte cmdlet para criar sua máquina virtual.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

A máquina virtual é criada.

> [!NOTE]
> Você pode ignorar o erro sobre o diagnóstico de inicialização. Uma conta de armazenamento padrão é criada para o diagnóstico de inicialização porque a conta de armazenamento especificada para o disco da máquina virtual é uma conta de armazenamento premium.

## <a name="install-the-sql-iaas-agent"></a>Instalar o SQL IaaS Agent
As máquinas virtuais do SQL Server suportam funcionalidades de gerenciamento automático com a [Extensão SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md). Para instalar o agente na nova VM, execute o seguinte comando após sua criação.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remove-a-test-vm"></a>Remova uma VM de teste

Se você não precisa que a VM seja executada continuamente, é possível interrompê-la quando não estiver em uso para evitar encargos desnecessários. O comando a seguir interrompe a VM, mas a deixa disponível para uso futuro.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Você também pode excluir permanentemente todos os recursos associados à máquina virtual com o comando **Remove-AzureRmResourceGroup**. Isso excluirá permanentemente a máquina virtual também, portanto, use esse comando com cuidado.

## <a name="example-script"></a>Script de exemplo
O script a seguir contém o script completo do PowerShell para este tutorial. Ele pressupõe que você já configurou a assinatura do Azure para usar com os comandos **Add-AzureRmAccount** e **Select-AzureRmSubscription**.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Próximas etapas
Depois que a máquina virtual for criada, você pode:

- Conectar-se à máquina virtual usando a área de trabalho remota (RDP).
- Configurar configurações do SQL Server no portal para sua VM, incluindo:
   - [Configurações de armazenamento](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Automatizar tarefas de gerenciamento](virtual-machines-windows-sql-server-agent-extension.md)
- [Configurar conectividade](virtual-machines-windows-sql-connect.md).
- Conecte clientes e aplicativos para a nova instância do SQL Server.


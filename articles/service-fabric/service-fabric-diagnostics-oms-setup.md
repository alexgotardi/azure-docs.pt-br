---
title: "Azure Service Fabric – Configurando o monitoramento com o Log Analytics do OMS | Microsoft Docs"
description: Saiba como configurar o OMS para visualizar e analisar eventos para o monitoramento de clusters do Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: dekapur
ms.openlocfilehash: 53b06c5a1395f34c96d4011366835a920d5c670b
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>Configurar o Log Analytics do OMS para um cluster

Você pode configurar um espaço de trabalho do OMS por meio do Azure Resource Manager, PowerShell ou Azure Marketplace. Se você estiver mantendo um modelo do Resource Manager atualizado de sua implantação, para uso futuro, utilize o mesmo modelo para configurar o ambiente do OMS. Implantar por meio do Marketplace será mais fácil se você já tiver um cluster implantado, com o Diagnóstico habilitado. Caso não tenha acesso ao nível de assinatura na conta à qual está implantando o OMS, utilize o PowerShell ou implante por meio do modelo do Resource Manager.

> [!NOTE]
> Você precisa ter o Diagnóstico habilitado para o cluster para exibir os eventos do nível do cluster/plataforma para poder configurar o OMS para monitorar o cluster com êxito.

## <a name="deploying-oms-using-azure-marketplace"></a>Implantando o OMS usando o Microsoft Azure Marketplace

Se você preferir adicionar um espaço de trabalho de OMS depois de implantar um cluster, acesse o Azure Marketplace (no Portal) e procure *"Análise do Service Fabric."*

1. Clique em **Novo** no menu de navegação esquerdo. 

2. Pesquise *Análise do Service Fabric*. Clique no recurso que aparece.

3. Clique em **Criar**

    ![Análise do OMS SF no Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Na janela de criação de Análise do Service Fabric, clique em **Selecionar um espaço de trabalho** para o campo *Espaço de trabalho do OMS* e, em seguida, **Criar um novo espaço de trabalho**. Preencha as entradas necessárias. O único requisito é que a assinatura para o cluster do Service Fabric e o espaço de trabalho de OMS devem ser iguais. Uma vez que suas entradas foram validadas, o espaço de trabalho da OMS começará a ser implantado. Isso deve levar apenas alguns minutos.

5. Quando terminar, clique em **Criar** novamente na parte inferior da janela de criação de Análise do Service Fabric. Verifique se o novo espaço de trabalho será exibido em *Espaço de trabalho do OMS*. Isso adiciona a solução ao espaço de trabalho que você acabou de criar.

Se estiver usando o Windows, continue com as seguintes etapas para conectar OMS à conta de Armazenamento onde seus eventos de cluster são armazenados. Habilitar essa experiência corretamente para clusters Linux ainda está em andamento. Enquanto isso, continue com a adição do Agente do OMS para seu cluster.  

1. O espaço de trabalho ainda precisa estar conectado aos dados de diagnóstico provenientes do seu cluster. Navegue até o grupo de recursos onde você criou a solução de Análise do Service Fabric. Você deve ver *ServiceFabric (\<nameOfOMSWorkspace\>)*. Clique na solução para navegar até a página de visão geral, de onde você pode alterar as configurações de solução, configurações de espaço de trabalho e navegue até o portal do OMS.

2. No menu de navegação à esquerda, clique em **Logs das contas de armazenamento**, em *Fontes de dados de espaço de trabalho*.

3. Na página *Logs das contas de armazenamento*, clique em **Adicionar** na parte superior para adicionar os logs do cluster ao espaço de trabalho.

4. Clique em **Conta de armazenamento** para adicionar a conta apropriada criada no cluster. Se você usou o nome padrão, a conta de armazenamento será denominada *sfdg\<resourceGroupName\>*. Você também pode confirmar isso verificando o modelo do Azure Resource Manager usado para implantar o cluster, verificando o valor usado para o `applicationDiagnosticsStorageAccountName`. Se o nome não aparecer, talvez você precisará rolar para baixo e clicar em **Carregar mais**. Clique no nome da conta de armazenamento adequado para selecioná-lo.

5. Em seguida, você precisará especificar o *Tipo de Dados*, que deve ser definido como **Eventos do Fabric Service**.

6. A *Fonte* deve ser definida automaticamente para *WADServiceFabric\*EventTable*.

7. Clique em **OK** para conectar o espaço de trabalho aos seus logs do cluster.

    ![Adicionar logs de conta de armazenamento ao OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

A conta agora deve aparecer como parte dos seus *Logs de conta de armazenamento* nas fontes de dados do seu espaço de trabalho.

Com isso você acaba de adicionar a solução de Análise do Service Fabric em um espaço de trabalho do Log Analytics do OMS que agora está conectado corretamente à plataforma do cluster e à tabela de log do aplicativo. Você pode adicionar outras fontes ao espaço de trabalho dessa mesma forma.


## <a name="deploying-oms-using-a-resource-manager-template"></a>Implantando o OMS usando um modelo do Resource Manager

Ao implantar um cluster usando um modelo do Resource Manager, o modelo deve criar um novo espaço de trabalho de OMS, adicionar a Solução do Service Fabric a ele e configurá-lo para ler dados das tabelas de armazenamento apropriadas.

[Aqui](https://azure.microsoft.com/resources/templates/service-fabric-oms/) está um modelo de exemplo que você pode usar e modificar conforme a necessidade. Mais modelos que oferecem diferentes opções para configurar um espaço de trabalho do OMS podem ser encontradas em [Modelos do Service Fabric e do OMS](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

As principais alterações feitas são as seguintes:

1. Adicionar `omsWorkspaceName` e `omsRegion` aos parâmetros. Isso significa adicionar o trecho de código a seguir aos parâmetros definidos em seu arquivo *template.json*. Fique à vontade para modificar os valores padrão como desejar. Você também deve adicionar dois novos parâmetros em seu *parameters.json* para definir seus valores para a implantação de recursos:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    Os valores `omsRegion` devem estar em conformidade com um conjunto específico de valores. Você deve escolher um que seja mais próximo à implantação do cluster.

2. Se você estiver enviando qualquer log do aplicativo ao OMS, confirme se o `applicationDiagnosticsStorageAccountType` e o `applicationDiagnosticsStorageAccountName` estão incluídos como parâmetros em seu modelo. Se não estiverem, adicione-os à seção de variáveis assim e edite seus valores conforme necessário. Você também pode inclui-los como parâmetros, se desejar, seguindo o formato usado acima.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Adicione a solução do OMS do Service Fabric às variáveis do seu modelo:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Adicionando o seguinte ao final da seção de recursos, depois de onde o recurso de cluster do Service Fabric é declarado.

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```
    
    > [!NOTE]
    > Se você adicionou `applicationDiagnosticsStorageAccountName` como uma variável, certifique-se de modificar cada referência para `variables('applicationDiagnosticsStorageAccountName')` em vez de `parameters('applicationDiagnosticsStorageAccountName')`.

5. Implantar o modelo como uma atualização do Gerenciador de Recursos ao cluster. Isso é feito usando a API `New-AzureRmResourceGroupDeployment` no módulo do AzureRM PowerShell. Um exemplo de comando pode ser:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    O Azure Resource Manager será capaz de detectar que essa é uma atualização para um recurso existente. Ele processará somente as alterações entre o modelo levando à implantação existente e o novo modelo fornecido.

## <a name="deploying-oms-using-azure-powershell"></a>Implantando o OMS usando o Azure PowerShell

Você também pode implantar o recurso OMS Log Analytics por meio do PowerShell. Isto é conseguido usando o comando `New-AzureRmOperationalInsightsWorkspace`. Para fazer isso, certifique-se de que o [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1) está instalado. Use esse script para criar um novo espaço de trabalho do OMS Log Analytics e adicionar a solução do Service Fabric a ele: 

```ps

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Uma vez que estiver concluído, se seu cluster for um cluster do Windows, siga as etapas na seção acima para conectar o OMS Log Analytics à conta de armazenamento apropriada.

Também é possível adicionar outras soluções ou fazer outras modificações no espaço de trabalho do OMS usando o PowerShell. Para ler mais sobre isso, consulte [Gerenciar o Log Analytics usando o PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md)

## <a name="next-steps"></a>Próximas etapas
* [Implantar o Agente do OMS](service-fabric-diagnostics-oms-agent.md) para os nós para coletar os contadores de desempenho e coletar logs e estatísticas do docker para seus contêineres
* Familiarize-se com os recursos de [pesquisa e consulta de logs](../log-analytics/log-analytics-log-searches.md) oferecidos como parte do Log Analytics
* [Use o Designer de Exibição para criar exibições personalizadas no Log Analytics](../log-analytics/log-analytics-view-designer.md)

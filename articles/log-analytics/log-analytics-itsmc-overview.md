---
title: "Conector de Gerenciamento de Serviço de TI no Azure Log Analytics | Microsoft Docs"
description: "Este artigo fornece uma visão geral do ITSMC (Conector de Gerenciamento de Serviços de TI) e informações como usar esta solução para monitorar e gerenciar de forma centralizada os itens de trabalho de ITSM no Azure Log Analytics, bem como resolver problemas rapidamente."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: 56da2d4349a4a32eed783045381e504b529b1a1c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Conectar o Azure a ferramentas de ITSM usando o Conector de Gerenciamento de Serviços de TI

![Símbolo do Conector de Gerenciamento do Serviço de TI](./media/log-analytics-itsmc/itsmc-symbol.png)

O ITSMC (Conector de Gerenciamento de Serviços de TI) permite que você conecte o Azure e um produto/serviço de ITSM (Gerenciamento de Serviços de TI) compatível.

Os serviços do Azure como o Log Analytics e o Azure Monitor fornecem ferramentas para detectar, analisar e solucionar problemas com os recursos do Azure e não pertencentes ao Azure. No entanto, os itens de trabalho relacionados a um problema normalmente residem em um produto/serviço de ITSM. O conector ITSM fornece uma conexão bidirecional entre o Azure e as ferramentas de ITSM para ajudá-lo a resolver problemas mais rapidamente.

O ITSMC é compatível com conexões com as seguintes ferramentas de ITSM:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

Com o ITSMC, você pode

-  Criar itens de trabalho na ferramenta de ITSM, com base nos alertas do Azure (alertas de métricas, alertas do Log de Atividades e alertas do Log Analytics).
-  Opcionalmente, você pode sincronizar seu incidente e alterar os dados de solicitação na ferramenta de ITSM para um espaço de trabalho do Azure Log Analytics.


Comece usando o Conector ITSM usando as seguintes etapas:

1.  [Adicionar a solução Conector ITSM](#adding-the-it-service-management-connector-solution)
2.  [Criar uma conexão de ITSM](#creating-an-itsm-connection)
3.  [Usar a conexão](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Adicionando a solução Conector de Gerenciamento de Serviço de TI

Antes de criar uma conexão, é necessário adicionar a Solução Conector ITSM.

1.  No portal do Azure, clique no ícone **+ Novo**.

    ![Novo recurso do Azure](./media/log-analytics-itsmc/azure-add-new-resource.png)

2.  Pesquise **Conector de Gerenciamento de Serviços de TI** no Marketplace e clique em **Criar**.

    ![Adicionar solução de ITSMC](./media/log-analytics-itsmc/add-itsmc-solution.png)

3.  Na seção **Espaço de Trabalho do OMS**, selecione o espaço de trabalho do Azure Log Analytics no qual deseja instalar a solução.
4.  Na seção **Configurações de Espaço de Trabalho do OMS**, selecione o ResourceGroup em que deseja criar o recurso de solução.

    ![Espaço de trabalho do ITSMC](./media/log-analytics-itsmc/itsmc-solution-workspace.png)

5.  Clique em **Criar**.

Quando o recurso de solução é implantado, uma notificação é exibida na parte superior direita da janela.


## <a name="creating-an-itsm--connection"></a>Criando uma conexão de ITSM

Depois de instalar a solução, crie uma conexão.

Para criar uma conexão, você precisará preparar a ferramenta de ITSM para permitir a conexão da solução Conector ITSM.  

Dependendo do produto de ITSM ao qual você está se conectando, use as seguintes etapas:

- [SCSM (System Center Service Manager)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

Depois de preparar as ferramentas de ITSM, siga as etapas abaixo para criar uma conexão:

1.  Acesse **Todos os Recursos** e procure **ServiceDesk(YourWorkspaceName)**.
2.  Em **FONTES DE DADOS DO ESPAÇO DE TRABALHO** no painel esquerdo, clique em **Conexões de ITSM**.
    ![Conexões de ITSM](./media/log-analytics-itsmc/itsm-connections.png)

    Esta página exibe a lista de conexões.
3.  Clique em **Adicionar Conexão**.

    ![Adicionar conexão de ITSM](./media/log-analytics-itsmc/add-new-itsm-connection.png)

4.  Especifique as configurações de conexão, conforme descrito no [artigo Configurando a conexão de ITSMC com seus produtos/serviços de ITSM](log-analytics-itsmc-connections.md).

    > [!NOTE]

    > Por padrão, o ITSMC atualiza os dados de configuração da conexão uma vez a cada 24 horas. Para atualizar os dados da conexão instantaneamente para as edições ou atualizações do modelo que você fizer, clique no botão “Atualizar” exibido ao lado de sua conexão.

    ![Atualização da conexão](./media/log-analytics-itsmc/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Usando a solução
   Usando a solução Conector ITSM, você pode criar itens de trabalho com base em alertas do Azure, alertas do Log Analytics e registros de log do Log Analytics.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Criar itens de trabalho de ITSM desde alertas do Azure

Depois de criar a conexão de ITSM, crie itens de trabalho na ferramenta de ITSM com base nos alertas do Azure, usando a **Ação de ITSM** em **Grupos de Ação**.

Os Grupos de Ações fornecem uma maneira modular e reutilizável de disparar ações para os Alertas do Azure. Use Grupos de Ação com alertas de métricas, alertas do Log de Atividades e alertas do Azure Log Analytics no portal do Azure.

Use este procedimento:

1. No portal do Azure, clique em **Monitorar**.
2. No painel esquerdo, clique em **Grupos de ações**. A janela **Adicionar grupo de ações** é exibida.

    ![Grupos de Ação](media/log-analytics-itsmc/action-groups.png)

3. Forneça um **nome** e um **ShortName** para o grupo de ações. Selecione o **Grupo de Recursos** e a **Assinatura** em que deseja criar seu grupo de ações.

    ![Detalhes dos grupos de ações](media/log-analytics-itsmc/action-groups-details.png)

4. Na lista Ações, selecione **ITSM** no menu suspenso para **Tipo de Ação**. Forneça um **Nome** para a ação e clique em **Editar detalhes**.
5. Selecione a **Assinatura** em que o espaço de trabalho do Log Analytics está localizado. Selecione o nome da **Conexão** (o nome do Conector de Gerenciamento de Serviços de TI) seguido pelo nome do Espaço de Trabalho. Por exemplo, "MyITSMMConnector(MyWorkspace)."

    ![Detalhes da ação de ITSM](./media/log-analytics-itsmc/itsm-action-details.png)

6. Selecione o tipo **Item de Trabalho** na lista suspensa.
   Escolha usar um modelo existente ou preencha os campos necessários para seu produto de ITSM.
7. Clique em **OK**.

Ao criar/editar uma regra de alerta do Azure, use um grupo de ações que tenha uma Ação de ITSM. Quando o alerta é disparado, o item de trabalho é criado/atualizado na ferramenta de ITSM.

>[!NOTE]

> Para obter informações sobre os preços da Ação de ITSM, visite a [página de preços](https://azure.microsoft.com/pricing/details/monitor/) dos Grupos de Ação.


## <a name="create-itsm-work-items-from-log-analytics-alerts"></a>Criar itens de trabalho de ITSM com base em alertas do Log Analytics

Configure regras de alerta no portal do Azure Log Analytics para criar itens de trabalho na ferramenta de ITSM, usando o procedimento a seguir.

1. Na janela **Pesquisa de Logs**, execute uma consulta da pesquisa de logs para exibir os dados. Os resultados da consulta são a fonte de itens de trabalho.
2. Em **Pesquisa de Logs**, clique em **Alerta** para abrir a página **Adicionar Regra de Alerta**.

    ![Tela do Log Analytics](./media/log-analytics-itsmc/itsmc-work-items-for-azure-alerts.png)

3. Na janela **Adicionar Regra de Alerta**, forneça os detalhes necessários para **Nome**, **Gravidade**, **Consulta de pesquisa** e **Critérios de alerta** (medição Janela de Tempo/Métrica).
4. Selecione **Sim** para **Ações de ITSM**.
5. Selecione a conexão de ITSM na lista **Selecionar Conexão**.
6. Forneça os detalhes conforme necessário.
7. Para criar um item de trabalho separado para cada entrada de log desse alerta, marque a caixa de seleção **Criar itens de trabalho individuais para cada entrada de log**.

    Ou

    deixe essa caixa de seleção desmarcada para criar apenas um item de trabalho para qualquer quantidade de entradas de log nesse alerta.

7. Clique em **Salvar**.

Exiba o alerta do Log Analytics que você criou em **Configurações > Alertas**. Os itens de trabalho da conexão de ITSM correspondentes são criados quando o critério do alerta especificado é atendido.


## <a name="create-itsm-work-items-from-log-analytics-log-records"></a>Criar itens de trabalho de ITSM com base em registros de log do Log Analytics

Você também pode criar itens de trabalho nas origens de ITSM conectadas diretamente de um registro de log. Isso pode ser usado para testar se a conexão está funcionando corretamente.


1. Na **Pesquisa de Logs**, pesquise os dados necessários, selecione os detalhes e clique em **Criar item de trabalho**.

    A janela **Criar item de trabalho de ITSM** é exibida:

    ![Tela do Log Analytics](media/log-analytics-itsmc/itsmc-work-items-from-azure-logs.png)

2.   Adicione os seguintes detalhes:

  - **Título do Item de trabalho**: título do item de trabalho.
  - **Descrição do Item de trabalho**: descrição do novo item de trabalho.
  - **Computador Afetado**: nome do computador no qual esses dados de logs foram encontrados.
  - **Selecionar Conexão**: conexão de ITSM na qual você deseja criar esse item de trabalho.
  - **Item de trabalho**: tipo de item de trabalho.

3. Para usar um modelo de item de trabalho existente para um incidente, clique em **Sim** na opção **Gerar item de trabalho com base no modelo** e clique em **Criar**.

    Ou,

    Clique em **Não** se desejar fornecer valores personalizados.

4. Forneça os valores apropriados nas caixas de texto **Tipo de Contato**, **Impacto**, **Urgência**, **Categoria** e **Subcategoria** e clique em **Criar**.


##<a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualizar e analisar os dados de incidente e solicitação de alteração

Com base na configuração feita durante a configuração de uma conexão, o conector ITSM pode sincronizar até 120 dias de dados de Incidente e Solicitação de alteração. O esquema de registro de log para esses dados é fornecido na [próxima seção](#additional-information).

Os dados de incidente e de solicitação de alteração podem ser visualizados usando o painel do Conector ITSM na solução.

![Tela do Log Analytics](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

O painel também fornece informações sobre o status do conector que podem ser usadas como um ponto de partida para analisar problemas com as conexões.

Também visualize os incidentes sincronizados nos computadores afetados, na solução Mapa do Serviço.

O Mapa do Serviço descobre automaticamente os componentes de aplicativos em sistemas Windows e Linux e mapeia a comunicação entre os serviços. Ele permite que você exiba os seus servidores da maneira como pensa neles – como sistemas interconectados que fornecem serviços essenciais. O Mapa do Serviço mostra conexões entre servidores, processos e portas em qualquer arquitetura conectada a TCP sem nenhuma configuração necessária além da instalação de um agente. [Saiba mais](../operations-management-suite/operations-management-suite-service-map.md).

Se estiver usando a solução Mapa do Serviço, exiba os itens da central de serviços criados nas soluções de ITSM, conforme mostrado no seguinte exemplo:

![Tela do Log Analytics](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)

Mais informações: [Mapa do Serviço](../operations-management-suite/operations-management-suite-service-map.md)


## <a name="additional-information"></a>Informações adicionais

### <a name="data-synced-from-itsm-product"></a>Dados sincronizados do produto de ITSM
Os incidentes e as solicitações de alteração são sincronizados do produto de ITSM para o espaço de trabalho do Log Analytics, de acordo com a configuração da conexão.

As informações a seguir mostram exemplos dos dados coletados pelo ITSMC:

> [!NOTE]

> Dependendo do tipo de item de trabalho importado para o Log Analytics, **ServiceDesk_CL** contém os seguintes campos:

**Item de trabalho:** **Incidentes**  
ServiceDeskWorkItemType_s="Incident"

**Campos**

- ServiceDeskConnectionName
- ID da Central de Serviços
- Estado
- Urgência
- Impacto
- Prioridade
- Escalonamento
- Criado por
- Resolvido por
- Fechado por
- Fonte
- Atribuído a
- Categoria
- Title
- DESCRIÇÃO
- Data de criação
- Data de fechamento
- Data de resolução
- Data da última modificação
- Computador


**Item de trabalho:** **Solicitações de alteração**

ServiceDeskWorkItemType_s="ChangeRequest"

**Campos**
- ServiceDeskConnectionName
- ID da Central de Serviços
- Criado por
- Fechado por
- Fonte
- Atribuído a
- Title
- type
- Categoria
- Estado
- Escalonamento
- Status do conflito
- Urgência
- Prioridade
- Risco
- Impacto
- Atribuído a
- Data de criação
- Data de fechamento
- Data da última modificação
- Data de solicitação
- Data de início prevista
- Data de término prevista
- Data de início do trabalho
- Data de término do trabalho
- DESCRIÇÃO
- Computador

## <a name="output-data-for-a-servicenow-incident"></a>Dados de saída de um incidente do ServiceNow

| Campo Log Analytics | Campo do ServiceNow |
|:--- |:--- |
| ServiceDeskId_s| Número |
| IncidentState_s | Estado |
| Urgency_s |Urgência |
| Impact_s |Impacto|
| Priority_s | Prioridade |
| CreatedBy_s | Aberto por |
| ResolvedBy_s | Resolvido por|
| ClosedBy_s  | Fechado por |
| Source_s| Tipo de contato |
| AssignedTo_s | Atribuído a  |
| Category_s | Categoria |
| Title_s|  Descrição breve |
| Description_s|  Observações |
| CreatedDate_t|  Aberto |
| ClosedDate_t| closed|
| ResolvedDate_t|Resolvido|
| Computador  | Item de configuração |

## <a name="output-data-for-a-servicenow-change-request"></a>Dados de saída para uma solicitação de alteração do ServiceNow

| Log Analytics | Campo do ServiceNow |
|:--- |:--- |
| ServiceDeskId_s| Número |
| CreatedBy_s | Solicitado por |
| ClosedBy_s | Fechado por |
| AssignedTo_s | Atribuído a  |
| Title_s|  Descrição breve |
| Type_s|  type |
| Category_s|  Categoria |
| CRState_s|  Estado|
| Urgency_s|  Urgência |
| Priority_s| Prioridade|
| Risk_s| Risco|
| Impact_s| Impacto|
| RequestedDate_t  | Solicitado por data |
| ClosedDate_t | Data de fechamento |
| PlannedStartDate_t  |     Data de início planejada |
| PlannedEndDate_t  |   Data de término planejada |
| WorkStartDate_t  | Data de início real |
| WorkEndDate_t | Data de término real|
| Description_s | DESCRIÇÃO |
| Computador  | Item de Configuração |


## <a name="troubleshoot-itsm-connections"></a>Solução de problemas de conexões de ITSM
1.  Se a conexão falhar na interface do usuário de origem conectada, com uma mensagem **Erro ao salvar conexão**, execute as seguintes etapas:
- Para conexões com o ServiceNow, o Cherwell e o Provance,  
           - verifique se você digitou corretamente o nome de usuário, a senha, a ID do cliente e o segredo do cliente para cada uma das conexões.  
           - verifique se você tem privilégios suficientes no produto de ITSM correspondente para fazer a conexão.  
- Para conexões do Service Manager,  
           - verifique se o aplicativo Web está implantado com êxito e se a conexão híbrida está criada. Para verificar se a conexão está estabelecida com êxito com o computador do Service Manager local, visite a URL do aplicativo Web conforme detalhado na documentação para fazer a [conexão híbrida](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).  

2.  Se os dados do ServiceNow não estiverem sendo sincronizados com o Log Analytics, verifique se a instância do ServiceNow não está em suspensão. As instâncias de desenvolvimento do ServiceNow, às vezes, entram em suspensão quando ficam ociosas por um longo período. Caso contrário, relate o problema.
3.  Se os alertas do OMS são disparados mas os itens de trabalho não são criados no produto de ITSM ou se os itens de configuração não são criados/vinculados aos itens de trabalho ou para obter qualquer outra informação genérica, procure nos seguintes locais:
 -  ITSMC: a solução mostra um resumo de conexões/itens de trabalho/computadores etc. Clique no bloco que mostra o **Status do Conector**, que o levará à **Pesquisa de Logs** com a consulta relevante. Examine os registros de log com LogType_S como ERROR para obter mais informações.
 - Página **Pesquisa de Log**: exiba os erros ou as informações relacionadas diretamente usando a consulta *Type=ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Solucionar problemas de implantação do aplicativo Web do Service Manager
1.  No caso de problemas com a implantação do aplicativo Web, verifique se que você tem permissões suficientes na assinatura mencionada para criar/implantar recursos.
2.  Se você receber um erro **"A referência de objeto não está definida para a instância de um objeto"** ao executar o [script](log-analytics-itsmc-service-manager-script.md), verifique se você inseriu valores válidos na seção **Configuração do Usuário**.
3.  Se você não conseguir criar o namespace de retransmissão do barramento de serviço, certifique-se de que o provedor de recursos necessário está registrado na assinatura. Se ele não estiver registrado, crie manualmente o namespace de retransmissão do barramento de serviço usando o Portal do Azure. Você também pode criá-lo ao [criar a conexão híbrida](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) do Portal do Azure.


## <a name="contact-us"></a>Fale conosco

Em caso de dúvidas ou comentários sobre o Conector de Gerenciamento de Serviço de TI, entre em contato conosco pelo email [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Próximas etapas
[Adicionar produtos/serviços de ITSM ao Conector de Gerenciamento de Serviço de TI](log-analytics-itsmc-connections.md).

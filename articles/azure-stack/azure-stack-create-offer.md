---
title: Criar uma oferta na pilha do Azure | Microsoft Docs
description: "Como um administrador de nuvem, saiba como criar uma oferta para os usuários na pilha do Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 2666aacbbaf44ae9b3bcf2df480e835457e6f449
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Criar uma oferta no Azure Stack

*Aplica-se a: Azure pilha integrado sistemas e o Kit de desenvolvimento de pilha do Azure*

[Oferece](azure-stack-key-features.md) são grupos de um ou mais planos provedores apresentam aos usuários para comprar ou assinar. Este documento mostra como criar uma oferta que inclui o [plano que você criou](azure-stack-create-plan.md) na última etapa. Esta oferta fornece a capacidade de provisionar máquinas virtuais de assinantes.

1. Entrar no portal do administrador do Azure pilha (https://adminportal.local.azurestack.external) > clique **novo** > **locatário oferece + planos**  >   **Oferecer**.

   ![](media/azure-stack-create-offer/image01.png)
2. No **oferecem nova** painel, preencha **nome de exibição** e **nome do recurso**e, em seguida, selecione um novo ou existente **grupo de recursos**. O nome de exibição é o nome amigável da oferta. Esse nome amigável é a única informação sobre a oferta que os usuários veem ao inscrever-se. Portanto, certifique-se de usar um nome intuitivo que ajuda o usuário a entender o que vem com a oferta. Somente o administrador pode ver o Nome do Recurso. Esse é o nome que os administradores usam para trabalhar com a oferta como um recurso do Gerenciador de Recursos do Azure.

   ![](media/azure-stack-create-offer/image01a.png)
3. Clique em **Base planos** para abrir o **planejar** painel, selecione os planos que você deseja incluir na oferta e, em seguida, clique em **selecione**. Clique em **Criar** para criar a oferta.

   ![](media/azure-stack-create-offer/image02.png)
4. Clique em **todos os recursos**, procure sua nova oferta, clique em nova oferta, clique em **alterar estado**e, em seguida, clique em **público**.

   ![](media/azure-stack-create-offer/image03.png)

Ofertas devem ser feitas públicas para os usuários a obter o modo de exibição completo ao inscrever-se. Ofertas podem ser:

* **Público**: visível para os usuários.
* **Privada**: só é visível para os administradores de nuvem. Útil ao projeto o plano ou a oferta, ou se o administrador de nuvem deseja [criar cada assinatura para usuários](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
* **Desativados**: fechados para novos assinantes. O administrador de nuvem pode usar encerrado para evitar futuras assinaturas, mas deixe os assinantes atuais inalterados.

As alterações para a oferta não ficam imediatamente visíveis para o usuário. Para ver as alterações, talvez você precise logoff/logon para ver a nova assinatura no "seletor de assinatura" durante a criação de grupos de recursos/recursos.

> [!NOTE]
>Você também pode criar cotas, planos e ofertas de padrão por meio do PowerShell, conforme explicado no [Leiame do administrador de serviço do Azure pilha](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Próximas etapas
[Criar assinaturas](azure-stack-subscribe-plan-provision-vm.md)      
[Provisionar uma máquina virtual](azure-stack-provision-vm.md)

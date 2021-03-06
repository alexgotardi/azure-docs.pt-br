---
title: "Perguntas frequentes sobre o Serviço de Contêiner do Azure"
description: "Fornece respostas para algumas perguntas comuns sobre o Serviço de Contêiner do Azure."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 73c49510512c9148f4fee98423b14770fa8602b9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Perguntas frequentes sobre o AKS (Serviço de Contêiner do Azure)

Este artigo aborda as perguntas frequentes sobre o AKS (Serviço de Contêiner do Azure).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Quais regiões do Azure terão o AKS (Serviço de Contêiner do Azure)? 

- Canadá Central 
- Leste do Canadá 
- Centro dos EUA 
- Leste dos EUA 
- Sudeste da Ásia 
- Europa Ocidental 
- Oeste dos EUA 2 

## <a name="when-will-additional-regions-be-added"></a>Quando outras regiões serão adicionadas? 

Outras regiões são adicionadas de acordo com o aumento de demanda.

## <a name="are-security-updates-applied-to-aks-nodes"></a>As atualizações de segurança são aplicadas aos nós do AKS? 

Os patches de segurança do sistema operacional são aplicados aos nós do seu cluster de acordo com um agendamento noturno, no entanto, não é executada uma reinicialização. Se necessário, os nós podem ser reinicializados por meio do portal ou da CLI do Azure. Ao atualizar um cluster, a imagem mais recente do Ubuntu será usada e todos os patches de segurança serão aplicados (com uma reinicialização).

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>É recomendável que os clientes usem ACS ou AKSs? 

Considerando que o AKS (Serviço de Contêiner do Azure) entrará em GA em uma data posterior, é recomendável que você crie clusters de desenvolvimento e teste de PoC no AKS, mas crie clusters de produção no ACS-Kubernetes.  

## <a name="when-will-acs-be-deprecated"></a>Quando o ACS será preterido? 

O ACS será preterido próximo do período em que o AKS entrará em GA. Você terá 12 meses, após essa data, para migrar os clusters para o AKS. Durante o período de 12 meses, você pode executar todas as operações do ACS.

## <a name="does-aks-support-node-autoscaling"></a>O AKS é compatível com o dimensionamento automático de nó? 

O dimensionamento automático de nó não é compatível, mas está previsto. Essa [implementação de dimensionamento automático][auto-scaler] de software livre pode ser interessante para você.

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que são criados dois grupos de recursos com o AKS? 

Cada cluster do AKS (Serviço de Contêiner do Azure) está contido em dois grupos de recursos. O primeiro é criado por você e contém somente o recurso AKS. O segundo grupo de recursos é automaticamente criado durante a implantação e contém todos os recursos de infraestrutura de cluster, tais como máquinas virtuais, redes e recursos de armazenamento. Este grupo de recursos é criado para permitir uma limpeza de recursos fácil. 

O grupo de recursos criado automaticamente tem um nome semelhante a:

```
MC_myResourceGRoup_myAKSCluster_eastus
```

Ao adicionar recursos do Azure a serem usados com o cluster Kubernetes, tais como contas de armazenamento ou um endereço IP público reservado, esses recursos precisam ser criados no grupo de recursos criado automaticamente.   

## <a name="is-azure-key-vault-integrated-with-aks"></a>O Azure Key Vault é integrado com o AKS? 

Não, mas essa integração está planejada. Enquanto isso, você pode testar essa solução da [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  
---
title: "Azure Active Directory Domain Services: Habilitar a delegação restrita de kerberos | Microsoft Docs"
description: "Habilitar a delegação restrita de kerberos nos domínios gerenciados do Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: maheshu
ms.openlocfilehash: b09c725609fe866b0c9ba2f5b5789e00f808b1ab
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Configurar a KCD (delegação Restrita de Kerberos) em um domínio gerenciado
Muitos aplicativos precisam acessar recursos no contexto do usuário. O Active Directory oferece suporte a um mecanismo chamado delegação de Kerberos, que permite que esse caso de uso. Além disso, você pode restringir a delegação para que somente recursos específicos possam ser acessados no contexto do usuário. Os domínios gerenciados do Azure AD Domain Services são diferentes dos domínios tradicionais do Active Directory, pois estão bloqueados com mais segurança.

Este artigo mostra como configurar a delegação restrita de Kerberos em um domínio gerenciado do Azure AD Domain Services.

## <a name="kerberos-constrained-delegation-kcd"></a>KCD (delegação restrita de Kerberos)
A delegação de Kerberos permite que uma conta represente outra entidade de segurança (como um usuário) para acessar recursos. Considere um aplicativo Web que acessa uma API da Web de back-end no contexto de um usuário. Neste exemplo, o aplicativo Web (em execução no contexto de uma conta de serviço ou uma conta de computador/máquina) representa o usuário ao acessar o recurso (API da Web de back-end). A delegação Kerberos não é segura, pois não restringir os recursos que podem ser acessados pela conta de representação no contexto do usuário.

A KCD (Delegação restrita de Kerberos) restringe os serviços/recursos aos quais o servidor especificado pode agir em nome de um usuário. A KCD tradicional exige privilégios de administrador de domínio para configurar uma conta de domínio para um serviço, e restringe a conta em um domínio único.

A KCD tradicional também tem alguns problemas associados. Em sistemas operacionais mais antigos, se o administrador de domínio tiver configurado a KCD com base na conta para o serviço, o administrador do serviço não tinha como saber quais serviços de front-end eram delegados aos serviços de recursos que eles possuíam. E qualquer serviço front-end que pudesse ser delegado a um serviço de recurso representava um possível ponto de ataque. Se um servidor que hospedava um serviço front-end fosse comprometido, e tivesse sido configurado para delegar aos serviços de recursos, os serviços de recurso também poderiam ser comprometidos.

> [!NOTE]
> Em um domínio gerenciado do Azure AD Domain Services, você não tem privilégios de administrador de domínio. Portanto, **não é possível configurar uma KCD tradicional em um domínio gerenciado**. Use o KCD baseado em recursos conforme descrito neste artigo. Esse mecanismo também é mais seguro.
>
>

## <a name="resource-based-kcd"></a>KCD baseado em recursos
Do Windows Server 2012 em diante, os administradores de serviço ganham a capacidade de configurar a delegação restrita para seus serviços. Neste modelo, o administrador do serviço de back-end pode permitir ou negar que serviços específicos de front-end utilizem a KCD. Esse modelo é conhecido como **KCD com base em recursos**.

A KCD Com base em recursos é configurado usando o PowerShell. Use os cmdlets `Set-ADComputer` ou `Set-ADUser`, dependendo se a conta de representação é uma conta de computador ou uma conta de serviço/conta de usuário.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Configurar a KCD baseada em recursos para uma conta de computador em um domínio gerenciado
Vamos supor que você tenha um aplicativo Web em execução no computador 'contoso100-webapp.contoso100.com'. Ele precisa acessar o recurso (uma API da Web em execução no 'contoso100-api.contoso100.com') no contexto de usuários do domínio. Veja como você pode configurar a KCD baseada em recursos para esse cenário.

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Configurar a KCD baseada em recursos para uma conta de usuário em um domínio gerenciado
Vamos supor que você tenha um aplicativo web em execução como uma conta de serviço 'appsvc' e ele precisa acessar o recurso (uma API da Web em execução como uma conta de serviço - 'backendsvc') no contexto de usuários do domínio. Veja como você pode configurar a KCD baseada em recursos para esse cenário.

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Conteúdo relacionado
* [Serviços de Domínio do Azure AD - guia de Introdução](active-directory-ds-getting-started.md)
* [Visão geral da Delegação Restrita de Kerberos](https://technet.microsoft.com/library/jj553400.aspx)

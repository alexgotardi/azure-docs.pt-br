---
title: "Tutorial: integração do Azure Active Directory ao Lesson.ly | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Active Directory do Azure e o Lesson.ly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 01e4395d5b2baaaf130f7a0fa7aaa29d611559b7
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/23/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a>Tutorial: integração do Active Directory do Azure com o Lesson.ly

Neste tutorial, você aprenderá a integrar o Lesson.ly ao Azure AD (Azure Active Directory).

A integração do Lesson.ly com o Azure AD oferece os seguintes benefícios:

- No AD do Azure, você pode controlar quem tem acesso ao Lesson.ly
- Você pode permitir que seus usuários façam logon automaticamente em Lesson.ly (Logon único) com as contas do AD do Azure
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do AD do Azure ao Lesson.ly, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Lesson.ly

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Lesson.ly da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-lessonly-from-the-gallery"></a>Adicionando o Lesson.ly da galeria
Para configurar a integração do Lesson.ly ao AD do Azure, você precisa adicionar o Lesson.ly da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Lesson.ly da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **Lesson.ly**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. No painel de resultados, selecione **Lesson.ly** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Lesson.ly, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Lesson.ly é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vinculação entre um usuário do Azure AD e o usuário relacionado do Lesson.ly.

No Lesson.ly, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do AD do Azure com o Lesson.ly, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do Lesson.ly](#creating-a-lessonly-test-user)**: para ter um equivalente de Brenda Fernandes no Lesson.ly que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no Portal do Azure e configura o logon único no aplicativo Lesson.ly.

**Para configurar o logon único do AD do Azure com o Lesson.ly, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **Lesson.ly**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. Na seção **Domínio e URLs do Lesson.ly**, execute as seguintes etapas:

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    >Ao referenciar um nome genérico, **companyname** precisará ser substituído por um nome real.
    
    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte do Cliente Lesson.ly](mailto:dev@lessonly.com) para obter esses valores. 

4. Na seção **Certificado de Autenticação do SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado no computador.

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. O aplicativo Lesson.ly espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados à sua configuração de **Atributos do Token SAML**. A seguinte captura de tela mostra um exemplo disso.

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. Na seção **Atributos de Usuário** da caixa de diálogo **Logon único**, configure o atributo do token SAML, conforme mostrado na imagem anterior e realize as seguintes etapas:

    | Nome do atributo   | Valor do atributo |
    | ---------------  | ----------------|
    | urn: oid:2.5.4.42 |user.givenname |
    | urn: oid:2.5.4.4  |user.surname |
    | urn:oid:0.9.2342.19200300.100.1.3 |user.mail |
    | urn:oid:1.3.6.1.4.1.5923.1.1.1.10 |user.objectid |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.
    
    d. Clique em **OK**.     

7. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. Na seção **Configuração do Lesson.ly**, clique em **Configurar o Lesson.ly** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. Para configurar o logon único no lado do **Lesson.ly**, é necessário enviar o **Certificado (Base64)** baixado, a **URL de Saída, ID da Entidade SAML e URL do Serviço de Logon Único SAML** para a [equipe de suporte do Lesson.ly](mailto:dev@lessonly.com).

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-lessonly-test-user"></a>Criar um usuário de teste Lesson.ly

O objetivo desta seção é criar um usuário chamado Brenda Fernandes em Lesson.ly. O Lesson.ly dá suporte ao provisionamento just-in-time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o Lesson.ly, caso ele ainda não exista.

> [!NOTE]
> Se precisar criar um usuário manualmente, entre em contato com a [equipe de suporte do Lesson.ly](mailto:dev@lessonly.com).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure ao conceder acesso ao Lesson.ly.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Lesson.ly, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Lesson.ly**.

    ![Configurar o logon único](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco Lesson.ly no Painel de Acesso, deverá ser automaticamente conectado ao seu aplicativo Lesson.ly.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png


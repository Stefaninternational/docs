---
title: Configurando o provisionamento de SCIM para usuários gerenciados pela empresa
shortTitle: Provisionamento de usuários gerenciados
intro: Você pode configurar seu provedor de identidade para fornecer novos usuários e gerenciar seus integrantes na sua empresa e equipes.
product: '{% data reusables.gated-features.emus %}'
redirect_from:
  - /github/setting-up-and-managing-your-enterprise/managing-your-enterprise-users-with-your-identity-provider/configuring-scim-provisioning-for-enterprise-managed-users
  - /admin/authentication/managing-your-enterprise-users-with-your-identity-provider/configuring-scim-provisioning-for-enterprise-managed-users
  - /admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/configuring-scim-provisioning-for-enterprise-managed-users
  - /admin/identity-and-access-management/using-enterprise-managed-users-and-saml-for-iam/configuring-scim-provisioning-for-enterprise-managed-users
versions:
  ghec: '*'
topics:
  - Accounts
  - Enterprise
---

## Sobre o provisionamento para {% data variables.product.prodname_emus %}

Você deve configurar o provisionamento para {% data variables.product.prodname_emus %} a fim de criar, gerenciar e desativar contas de usuário para os integrantes da sua empresa. Ao configurar o provisionamento para {% data variables.product.prodname_emus %}, os usuários atribuídos ao aplicativo de {% data variables.product.prodname_emu_idp_application %} no seu provedor de identidade serão provisionados como novas contas de usuário em {% data variables.product.prodname_dotcom %} por meio do SCIM e os usuários serão adicionados à sua empresa.

Ao atualizar as informações associadas à identidade de um usuário no seu IdP, este atualizará a conta do usuário no GitHub.com. When you unassign the user from the {% data variables.product.prodname_emu_idp_application %} application or deactivate a user's account on your IdP, your IdP will communicate with {% data variables.product.prodname_dotcom %} to invalidate any sessions and disable the member's account. As informações da conta desativada serão mantidas e seu nome de usuário será alterado para hash do seu nome de usuário original com o código curto anexado. Se você reatribuir um usuário para o aplicativo {% data variables.product.prodname_emu_idp_application %} ou reativar sua conta no seu IdP, a conta de {% data variables.product.prodname_managed_user %} em {% data variables.product.prodname_dotcom %} será reativada e o nome de usuário restaurado.

Os grupos no seu IdP podem ser usados para gerenciar a participação de equipe nas organizações de sua empresa, permitindo que você configure o acesso e as permissões do repositório por meio do seu IdP. Para obter mais informações, consulte "[Gerenciar associações de equipe com grupos de provedor de identidade](/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/managing-team-memberships-with-identity-provider-groups)".

## Pré-requisitos

Before you can configure provisioning for {% data variables.product.prodname_emus %}, you must configure SAML{% if oidc-for-emu %} or OIDC{% endif %} single-sign on. {% if oidc-for-emu %}

- For more information on configuring OIDC, see "[Configuring OIDC for Enterprise Managed Users](/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/configuring-oidc-for-enterprise-managed-users)"
- {% endif %}For information on configuring SAML, see "[Configuring SAML single sign-on for Enterprise Managed Users](/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/configuring-saml-single-sign-on-for-enterprise-managed-users)."

## Criar um token de acesso pessoal

Para configurar o provisionamento para o seu {% data variables.product.prodname_emu_enterprise %}, você precisa de um token de acesso pessoal com o escopo **admin:enterprise** que pertence ao usuário configurado.

{% warning %}

**Aviso:** Se o token vencer ou um usuário provisionado criar o token, o provisionamento do SCIM poderá parar de funcionar, inesperadamente. Certifique-se de criar o token enquanto estiver conectado como usuário de configuração e que o vencimento do token esteja definido como "Sem vencimento".

{% endwarning %}

1. Efetue o login em {% data variables.product.prodname_dotcom_the_website %} como usuário de configuração da sua nova empresa com o nome de usuário **@<em>SHORT-CODE</em>_admin**.
{% data reusables.user-settings.access_settings %}
{% data reusables.user-settings.developer_settings %}
{% data reusables.user-settings.personal_access_tokens %}
{% data reusables.user-settings.generate_new_token %}
1. Em **Observação**, dê ao seu token um nome descritivo. ![Captura de tela que mostra o nome do token](/assets/images/help/enterprises/emu-pat-name.png)
1. Selecione o menu suspenso **Expiração** e, em seguida, clique **Sem vencimento**. ![Captura de tela que mostra o vencimento do token definido como sem vencimento](/assets/images/help/enterprises/emu-pat-no-expiration.png)
1. Selecione o escopo **admin:enterprise**. ![Captura de tela que mostra o escopo admin:enterprise](/assets/images/help/enterprises/enterprise-pat-scope.png)
1. Clique em **Generate token** (Gerar token). ![Botão Generate token (Gerar token)](/assets/images/help/settings/generate_token.png)
1. Para copiar o token para a área de transferência, clique no {% octicon "paste" aria-label="The copy icon" %}. ![Token recém-criado](/assets/images/help/settings/personal_access_tokens.png)
2. Para salvar o token para usar mais tarde, armazene o novo token de forma segura em um gerenciador de senhas.

## Configurando provisionamento para {% data variables.product.prodname_emus %}

Depois de criar seu token de acesso pessoal e armazená-lo com segurança, você pode configurar o provisionamento no seu provedor de identidade.

{% data reusables.scim.emu-scim-rate-limit %}

To configure provisioning, follow the appropriate link from the table below.

| Provedor de identidade | SSO method | Mais informações                                                                                                                                                                                                                                              |
| ---------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |{% if oidc-for-emu %}
| Azure AD               | OIDC       | [Tutorial: Configure GitHub Enterprise Managed User (OIDC) for automatic user provisioning](https://docs.microsoft.com/azure/active-directory/saas-apps/github-enterprise-managed-user-oidc-provisioning-tutorial) in the Azure AD documentation 
{% endif %}
| Azure AD               | SAML       | [Tutorial: Configure GitHub Enterprise Managed User for automatic user provisioning](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/github-enterprise-managed-user-provisioning-tutorial) in the Azure AD documentation                    |
| Okta                   | SAML       | [Configurando o provisionamento do SCIM para usuários gerenciados pela empresa com Okta](/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/configuring-scim-provisioning-for-enterprise-managed-users-with-okta)               |

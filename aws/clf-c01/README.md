# AWS Certified Cloud Practitioner - CLF-C02


<div align="center">
<img src="https://github.com/hellotatiramos/estudos-certificacoes/assets/158481113/9942753b-3a1f-48e7-8d5a-f67a2d0d90c2" width="700px" />
</div>


### Autenticação de Usuários:

**Login e Registro:** O Cognito permite que os usuários se registrem e façam login no seu aplicativo usando endereços de e-mail, números de telefone ou provedores de identidade externa, como Google, Facebook e Amazon.
Verificação: Após o registro, o Cognito pode enviar e-mails ou mensagens de texto para verificar a identidade do usuário.
Gerenciamento de Usuários:

**Grupos de Usuários:** Você pode criar grupos para organizar e gerenciar seus usuários. Por exemplo, você pode ter um grupo para administradores e outro para usuários comuns.

* **Atributos Personalizados:** Além das informações básicas (como nome e e-mail), você pode adicionar atributos personalizados para armazenar mais informações sobre os usuários.

### Autorização:

**Regras de Acesso:** Você pode definir regras que controlam o que diferentes usuários ou grupos de usuários podem fazer no seu aplicativo.

**Tokens:** Depois que um usuário faz login, o Cognito fornece tokens (JWTs) que seu aplicativo usa para autorizar o acesso a diferentes partes do sistema.
Integração com Outros Serviços da AWS:

O Cognito se integra facilmente com outros serviços da AWS, como Amazon API Gateway e AWS Lambda, facilitando a construção de aplicações serverless (sem servidor).
Sincronização de Dados:

O Cognito também oferece uma funcionalidade chamada "Cognito Sync", que permite sincronizar dados de aplicativos em diferentes dispositivos, facilitando a criação de uma experiência consistente para os usuários.

`
Em resumo, o Amazon Cognito simplifica a implementação de um sistema de autenticação e gerenciamento de usuários seguro e escalável, permitindo que você se concentre no desenvolvimento das funcionalidades principais do seu aplicativo sem se preocupar com a complexidade da gestão de usuários e segurança.
`
## User Pools e IIdentity Pools: 

### Grupos de Usuários (User Pools)
Um User Pool no Amazon Cognito é um diretório de usuários que permite adicionar funcionalidades de registro (sign-up), login (sign-in), recuperação de senha, entre outras. Aqui estão alguns pontos chave sobre os User Pools:

### Autenticação e Registro:

* **Registro de Usuário:** Usuários podem se registrar no seu aplicativo com um endereço de e-mail, número de telefone ou usando provedores de identidade externa como Google, Facebook, e Amazon.
* **Login:** Após o registro, os usuários podem fazer login no aplicativo usando suas credenciais.
Gerenciamento de Usuários:

* **Grupos e Funções:** Você pode criar grupos dentro do User Pool para organizar usuários e aplicar políticas específicas. Por exemplo, você pode ter grupos para administradores, moderadores e usuários comuns, cada um com diferentes níveis de acesso e permissões.

* **Atributos Personalizados:** Além dos atributos padrão como nome, e-mail e número de telefone, você pode adicionar atributos personalizados para armazenar informações adicionais dos usuários.
Verificação e Segurança:

* **MFA (Autenticação Multifator):** Para aumentar a segurança, você pode configurar MFA, exigindo que os usuários forneçam uma segunda forma de autenticação, como um código enviado por SMS.
 
* **Políticas de Senha:** Você pode definir políticas de senha para garantir que os usuários criem senhas fortes.

* **Tokens:** Quando um usuário faz login, o Cognito gera tokens de segurança (ID token, access token, e refresh token) que podem ser usados para autorizar e autenticar chamadas de API subsequentes.

### Identidade de Usuários (Identity Pools)
Um Identity Pool no Amazon Cognito permite que você conceda acesso aos seus recursos da AWS, como buckets do Amazon S3 ou tabelas do Amazon DynamoDB. Com um Identity Pool, você pode gerenciar identidades autenticadas e não autenticadas. Aqui estão alguns pontos chave sobre os Identity Pools:

#### Identidades Autenticadas e Não Autenticadas:

* **Identidades Autenticadas:** São identidades de usuários que fizeram login via User Pools ou através de provedores de identidade federada como Google, Facebook, Apple, ou mesmo SAML.
  
* **Identidades Não Autenticadas:** Permite que usuários que ainda não se registraram ou fizeram login no seu aplicativo acessem determinados recursos da AWS com permissões limitadas.

* **Credenciais Temporárias:** O Identity Pool pode fornecer credenciais temporárias da AWS para que os usuários acessem diretamente os serviços da AWS com base nas permissões definidas em políticas IAM.

* **Integração com User Pools:** Você pode usar um Identity Pool junto com um User Pool. Primeiro, os usuários fazem login no User Pool e, em seguida, o Identity Pool usa essas credenciais para fornecer acesso aos recursos da AWS.

* **Controle de Acesso:** Você pode definir diferentes funções e permissões no IAM (Identity and Access Management) da AWS para diferentes tipos de usuários, controlando o que cada grupo pode fazer dentro dos seus recursos AWS.

`Resumindo: User Pools: Focado no gerenciamento de usuários e autenticação. Ideal para quando você precisa de funcionalidades como registro, login, recuperação de senha e autenticação multifator.
Identity Pools: Focado em fornecer credenciais temporárias da AWS para que os usuários possam acessar recursos da AWS. Ideal para gerenciar acessos a serviços AWS com base em identidades autenticadas e não autenticadas.
`


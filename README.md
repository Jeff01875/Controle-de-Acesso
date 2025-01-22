# Controle-de-Acesso
**Este repositório documenta um projeto aonde utilizei IAM, Cognito, KMS, S3 e autentificar usuários e implementar permissões de menor privilégio**  

## **Diagrama do Projeto**
 ![Diagrama_projeto.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/Diagrama_projeto.png)
 
## Descrição do projeto

### Objetivo
Implementar uma solução escalável e segura para autentificar e gerenciar acessos aos recursos armazenados do S3, garantindo práticas recomendadas e seguras de permissões de menor privilégio e criptografia para o conteúdo no S3
---
### Componetes e funcionalidades 
1. **Amazon Cognito**
  - **USER POOL**: Criado para ser um diretório seguro para armazenar credenciais dos usuário autenticados.
  - **IDENTITY POOL**: Será integrado ao User pool para gerar credenciais temporárias na aws, associando permissões específicas 
    de acesso aos usuários.
    
2. **KMS(key manager service)**
  - Utilizado para criptografar a fonte do conteúdo armazenado no S3
  - Só usuários autentificados poderão descriptograr o conteúdo. Garantindo maior controle e segurança do acesso ao conteúdo

3. **S3(Simple storage service)**
  - **Armazenamento**: O s3 é um serviço de armazenamento arquivos, fotos, músicas e etc.
  - **Alta disponibilidade**: Ele traz alta disponibilidade e resiliênca pois ele será replicados em diversas (AZs) em uma 
     única região.
  - **Otimização de Custo**: Fornece opções de camadas para armazenar seus objetos. Podendo analisar a melhor camada para o 
     cenário desejado.

 4. **IAM(Identity and Acess Manager)**
   - IAM é um serviço onde nos possibilida criar, gerenciar e excluir usuário, policys, roles
   - **Policy**: Definem permissões ou negações aplicadas nos usuários, grupos, roles garantindo controle de acesso baseado no  
     princípio de menor privilégio .
 ---
 # Passo a Passo da Criação
 1. IAM Policy

  - Iniciei criando minha policy/role onde ela seria associada ao Identity pool do cognito
  - Através dela, qualquer usuário que fosse autentificado pelo user pool, teria acesso aos conteúdo
   ![policy_role_cognito.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/policy_role_cognito.png)

  > **⚠️ Atenção:** No meio tempo de teste desse projeto, tive diversos problemas referindo a **Acess denied** pois não ter configurado as permisões 
    correta. Porém, busquei alguma policy que associada as permissões ao cognito e o S3 na própria documentação. Essa policy que vc estão vendo, ela 
    básicamente associa as permissões de **GET**, **PUT**, **LIST** aos usuários do cognito.

 2. IAM Role

  - Criei a role para que ela assuma a policy
  - A role fornece credenciais temporárias de acesso aos recursos
  - Implementei a role como identidade Web pois ela é utilizada em cenário onde usuários que são autentificados através de um IDP, garantindo assim, credenciais temporárias 
    para acessar recursos dentro da aws
   ![ciracção_role_cognito.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/cirac%C3%A7%C3%A3o_role_cognito.png)

 3. IAM Users
  - Criei Dois usuários no IAM para que recebessem permissões específicas do kms
  > **⚠️ Atenção:** Poderia associar essas permissões à role do cognito, porém não estaria de acordo com o **Princípio de Menor Privilégio**
  - O primeiro recebeu permissões de Admin: Cria, excluir, criptografar, descriptografar e entre outros
  - O segundo recebu permissões de User: Criptografar, descriptografar
   ![iam_user.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/iam_user.png)

  > **⚠️ Atenção:** Por ser um teste, acabei não criando nenhum grupo para esses dois usuários, mas coloquei tags nesses usuários onde eu poderia identificar o que cada um 
    iria executar
   
 4. KMS
  - Criei uma chave simétrica no KMS   

 

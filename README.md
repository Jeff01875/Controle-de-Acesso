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
 
 1. IAM Users
   - Criei Dois usuários no IAM para que recebessem permissões específicas do kms
   - O primeiro recebeu permissões de Admin: Cria, excluir, criptografar, descriptografar e entre outros
   - O segundo recebu permissões de User: Criptografar, descriptografar
  
   ![IAM_USERS.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/IAM_USERS.png)

  > **⚠️ Atenção:** Por ser um teste, acabei não criando nenhum grupo para esses dois usuários, mas coloquei tags nesses usuários onde eu poderia identificar o que cada um 
    iria executar
  ---
 2. IAM Policy

   - criei a policy onde ela seria associada ao Identity pool do cognito
   - Através dela, qualquer usuário que fosse autentificado pelo user pool, teria acesso aos conteúdo
     
   ![POLICY_COGNITO.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/POLICY_COGNITO.png)

  > **⚠️ Atenção:** No meio tempo de teste desse projeto, tive diversos problemas referindo a **Acess denied** pois não ter configurado as permisões 
    correta. Porém, busquei alguma policy que associada as permissões ao cognito e o S3 na própria documentação. Essa policy que vc estão vendo, ela 
    básicamente associa as permissões de **GET**, **PUT**, **LIST** aos usuários do cognito.
  ---
 3.KMS 
   - Criei uma chave aonde ela será utilizada para criptografar e descriptografar o objecto que estará no bucket
   - Essa chave irá aumentar ainda mais a segurança e garantindo o objetivo do objeto
    
   ![KMS.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/KMS.png)

   - Por se tratar de um teste, utilizei uma chave Simétrica aonde se consiste de uma única chave que faz a criptografia e descriptografia, diferente da assimétrica que se 
    utiliza de duas chaves: Pública e privada 
  ---
 4. S3
   - Criei uma bucket no S3 onde será armazenado o objecto em que os usuários do cognito irão acessar
   - Essa bucket será criptografada pelo KMS
    
   ![BUCKET_OBJECT.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/BUCKET_OBJECT.png)
   
 5. Bucket policy
   - Eu tive que implementar uma policy de **GET**, **LIST** na bucket
   - Porém, eu tive que desmarcar a caixinha onde torna aquela bucket privada para pública
   - após desmarcar, implementei essa policy aonde ela torna a bucket totalmente pública 

   ![POLICY_BUCKET.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/POLICY_BUCKET.png)
   
  ---
 6. User Pool(cognito) - criação
   - Criei um grupo de identidade aonde será um diretório dos usuários
   - No momento em que o usuário finaliza o cadastro, os atributos de login que selecionei, estarão visíveis para mim.
   - Através do User pool, connsigo ter controle e visualizar dados dos usuários, podendo até exclui-los.
     
   ![USER_POOL_CREATION.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/USER_POOL_CREATION.png)
   
 7. User Pool(cognito) - criado
   - Aqui podemos visualizar o User pool criado
   - Podemos implementar um MFA no login para que se tenha mais um fator de autentificação, garantindo ainda mais a segurança do login dos usuários
   - Podemos também visualizar a identidade de visualização, onde será direcinado os usuários para que possam fazer o cadastro e login
    
   ![USER_POOL.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/USER_POOL.png)
   
 8. Criando um usuário no **USER POOL**
   - Criei um usuário para test, onde utilizei atributos de login básicos
    
   ![AUTHENTICATION.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/AUTHENTICATION.png)

 9. Confirmação de Cadastro
     
   ![CONFIRM_AUTHENTICATION.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/CONFIRM_AUTHENTICATION.png)

 10. IAM Role

  - Criei a role para que ela assuma a policy que permite os usuário autentificados pelo cognito, tenham acesso ao **S3** 
  - Ao assumir essa policy, eu irei associar essa role ao cognito, onde ele criará credenciais temporárias de acesso ao recurso de acordo com permissões específicas da 
    policy

    ![COGNITO_ROLE_1.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/COGNITO_ROLE_1.png)
    
 10. Criação Identity Pool
     
   - Criei o grupo de identidade para que ele criei credenciais temporárias da aws para os usuários
   - Nesse processo você precisa implementar o **USER POOL**
   - Após os usuários efetuarem o login, o identity pool utiliza do STS(Security Token Service) para que se crie tokens temporários aonde serão associados aos usuários

     ![IDENTITY_POOL.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/IDENTITY_POOL.png)

  ---

 11. Adicionar mais uma URL
     
   - Tive que alterar implementar mais uma URL na identidade visual, onde após efutuado o login, ele será redirecionado para o conteúdo
     
 12. Teste de login
   
   ![LOGIN_USER.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/LOGIN_USER.png)

 13. Lista do conteúdo armazenado no bucket do S3
   - Nesta parte eu tive exito e não tive ao mesmo tempo
   - O usuário conseguiu ter acesso as informações do conteúdo, porém, teve contato com um arquivo XML

   ![LIST_OBJECT.png](https://github.com/Jeff01875/Controle-de-Acesso/blob/main/LIST_OBJECT.png)
     
   - Neste arquivo podemos ver algumas informações, como por exemplo
   - Name: nome da bucket aonde o objeto está armazenado
   - Key: o nome do objeto
   - Displayname: Nome do usuário que está acessando o objeto

  ---
 
## Conclusão do Projeto

 

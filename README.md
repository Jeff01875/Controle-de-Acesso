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

 

# Controle-de-Acesso
**Este repositório documenta um projeto aonde utilizei IAM, Cognito, KMS, S3 e autentificar usuários e implementar permissões de menor privilégio**  

##Descrição do projeto
###Objetivo
Implementar uma solução escalável e segura para autentificar e gerenciar acessos aos recursos armazenados do S3, garantindo práticas recomendadas e seguras de permissões de menor privilégio e criptografia para o conteúdo no S3
---
###Componetes e funcionalidades 
1. **Amazon Cognito**
- Utilização do Cognito para a implementação do User Pool aonde serão armazenado as credenciais dos usuários e autentificados. E criei um Identity Pool que fará a criação de credenciais aws temporárias e a entrega de permissões para acessar os recursos
- IAM utilizado para criar permissões que serão associadas ao recurso e aos usuários onde compõe de permissões e ações minímas, garatindo as práticas recomendas de Menor Privilégio
- KMS utilizado para criptografar todo o conteúdo do recurso que será acessado pelos usuários. Trazendo maior segurança ao conteúdo e fazendo com que seja mais difício a tentativa de acesso sem autorização.
- S3 criado para ser a fonte do conteúdo que será acessado, trazendo alta disponibilidade pois será repilicado por várias Az´s de uma região e a durabilidade do conteúdo. Além de poder ser muito econômico o armazenamento desse objeto dentro do S3 caso ele não seja tão acessado e esteja em camadas de armazenamentos menores que a **STANDARD**

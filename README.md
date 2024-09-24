# FIAP - 4° Checkpoint - API Security 

## Introdução
Checkpoint realizado com o intuito de colocar em prática todos os conhecimentos de vulnerabilidade em API's adquiridos na matéria de WEB APPS e MOBILE EXPLOITATION, ministrada pelo [Professor Rafael Trassi ](https://www.linkedin.com/in/rafael-trassi/).

## Feito por
- Matheus Rosa
- Pedro Augusto

## Desafio
Os alunos deverão realizar uma análise de segurança em uma API simples da lista a seguir baseando-se no OWASP API Security TOP 10.
- https://github.com/OWASP/crAPI 
- https://github.com/roottusk/vapi
  
Cada aluno deverá:
- Escolher dois endpoints da API para realizar os testes de segurança. 
- Explicar e demonstrar a exploração de duas vulnerabilidades do OWASP API Security TOP 10 de 2023 nos endpoints escolhidos;
- Explicar brevemente por que a falha ocorre e demonstrar no código quais as linhas estão vulneráveis;
- Documentar os passos seguidos para exploração ou fornecer um script que explore a falha;
- Discutir brevemente sobre possíveis correções para esta falha (não precisa ser no código, pode apenas explicar o que resolveria).

# OWASP - crAPI 

Optamos por explorar os serviços do crAPI.

![image](https://github.com/user-attachments/assets/678bcc7c-952c-4d11-9de0-d96ae1ee68b1)

Após criar uma conta e fazer o login. Iniciei o burpsuit e fui em busca de endtrypoints.

## 1° endpoint - /forum

![image](https://github.com/user-attachments/assets/c9e1a8b1-cad3-4abd-9f60-b39ccd9d81bb)

Ao acessar qualquer comentário de usuário feito, em sua resposta do burp, a API devolve informações sensiveis do autor do comentário.

![image](https://github.com/user-attachments/assets/9fd8a1ef-00a2-46bf-9fdb-3e82c91e8e4e)

Response:

````
{"id":"6KhWWNXSdNgK7HVJFZ6E3B","title":"Title 3","content":"Hello world 3","author":{"nickname":"Robot","email":"robot001@example.com","vehicleid":"4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5","profile_pic_url":"","created_at":"2024-09-23T23:01:33.352Z"},"comments":[],"authorid":3,"CreatedAt":"2024-09-23T23:01:33.352Z"}
````

# Por que a falha ocorre?



# Linhas vulneráveis
# Script 
# O que resolveria?

## 2° endpoint - /location

O que me chamou a atenção foi o  **vehicleid**

Continuando o processo de análise e procurando onde poderia usar o ID, cadastrei o veiculo enviado no MailHog e notei que a API utiliza o vehicleid para puxar o veiculo e utilizar a api do google maps para mostrar a localização dele.

![image](https://github.com/user-attachments/assets/ee6962cd-89c2-43e4-bdb3-4fdcacf4c6ca)

Então decidi trocar pelo ID que consegui na primeira response: 

![image](https://github.com/user-attachments/assets/86a2dd54-4990-49ba-85c9-16cfb2fe4c79)

Response:

````
{"carId":"4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5","vehicleLocation":{"id":3,"latitude":"37.746880","longitude":"-84.301460"},"fullName":"Robot","email":"robot001@example.com"}
````

Além de responder com as coordenadas do veiculo, ele tambem devolve o email e usuario.

# Por que a falha ocorre?
# Linhas vulneráveis
# Script 
# O que resolveria?





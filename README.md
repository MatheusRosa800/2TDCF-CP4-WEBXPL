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

Após criar uma conta e fazer o login. Iniciei o burpsuit e fui em busca de endpoints.

## 1° endpoint - /forum & /location

![image](https://github.com/user-attachments/assets/c9e1a8b1-cad3-4abd-9f60-b39ccd9d81bb)

Ao acessar qualquer comentário de usuário feito, em sua resposta do burp, a API devolve informações sensiveis do autor do comentário.

![image](https://github.com/user-attachments/assets/9fd8a1ef-00a2-46bf-9fdb-3e82c91e8e4e)

Response:

````
{"id":"6KhWWNXSdNgK7HVJFZ6E3B","title":"Title 3","content":"Hello world 3","author":{"nickname":"Robot","email":"robot001@example.com","vehicleid":"4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5","profile_pic_url":"","created_at":"2024-09-23T23:01:33.352Z"},"comments":[],"authorid":3,"CreatedAt":"2024-09-23T23:01:33.352Z"}
````

O que me chamou a atenção foi o  **vehicleid**

Continuando o processo de análise e procurando onde poderia usar o ID, cadastrei o veiculo enviado no MailHog e notei que a API utiliza o vehicleid para puxar o veiculo e utilizar a api do google maps para mostrar a localização dele.

![image](https://github.com/user-attachments/assets/ee6962cd-89c2-43e4-bdb3-4fdcacf4c6ca)

Então decidi trocar pelo ID que consegui na primeira response: 

![image](https://github.com/user-attachments/assets/86a2dd54-4990-49ba-85c9-16cfb2fe4c79)

Response:

````
{"carId":"4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5","vehicleLocation":{"id":3,"latitude":"37.746880","longitude":"-84.301460"},"fullName":"Robot","email":"robot001@example.com"}
````

Além de responder com as coordenadas do veiculo, ele tambem devolve o email e usuário.

# Por que a falha ocorre?

Bola 
# Linhas vulneráveis
# Script 
# O que resolveria?












## 2° endpoint - /reset-password & /check-otp

Com o email que tinha, fui atras de endpoints que uilizavam o email. 

1. /change-email - Nada de mais
2. /change-password - Nada de mais
3. reset-password - Possível endpoint

O /reset-password era encontrado na tela de login clicando no campo "forgot password"

![image](https://github.com/user-attachments/assets/57bd4446-832f-4ff2-95c6-2543937d108f)

Na tela pedia o email, iria enviar um OTP de verificação e logo depois seria possível alterar a senha.

O OTP enviado para confirmar o email é um número de 4 caracteres e facil de ser quebrado.

![image](https://github.com/user-attachments/assets/72c0d3c1-0720-4efa-8151-bb7843182b58)

Iniciei o bruteforce mas depois de poucas tentativas recebi um erro.

![image](https://github.com/user-attachments/assets/a8c6e553-f009-4d1d-a50c-00ca49a64acd)

A API estava segura contra brute-force.

Achei que não teria como, acabei desistindo e fui explorar outros endpoints. Pórem analisando outros requests, vi que algumas páginas utilizavam **API V2** e outras **API V3**.

Decidi voltar para a senha e tentar trocar por v2.

````
POST /identity/api/auth/v2/check-otp HTTP/1.1
````

E funcionou. obtive a resposta de OTP incorreto.

Testei novamente o brute-force e vi que a versão 2 da API não tinha proteção.

Logo depois segui os seguintes passos:

1. Peguei o email que consegui do usuário 
2. Fiz o brute-force do OTP usando o Intruder do Burp 
3. Esperei muito tempo até o match do OTP
4. Consegui alterar a senha 
5. Entrei na conta do ROBOT.

![image](https://github.com/user-attachments/assets/15430dc0-de73-4ddd-b306-2d645bf5d730)


![image](https://github.com/user-attachments/assets/f3128ce1-ea70-484f-b346-3b1546571078)



# Por que a falha ocorre?

Broken User Authentication


# Linhas vulneráveis
# Script 
# O que resolveria?

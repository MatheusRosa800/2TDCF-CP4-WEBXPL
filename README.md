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


1 endpoint


o primeiro endpoint que encontrei vulneravel foi na area de compras /shop

utilizando o burp para analisar alguns entrypoints descobri que ao efetuar uma compra e ir nos detalhes da compra. A api utiliza um simples order_id=X

alterando este numero para outros é possivel ver detalhes de outras compras de outras pessoas incluindo dados sensiveis

exemplo:
/workshop/api/shop/orders/1

img2

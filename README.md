# Entendendo o que é DTO

Este repositório contém minhas explicações sobre alguns fundamentos de DTO (Data Transfer Object) e o fluxo de comunicação entre frontend, backend e banco de dados, com foco em atuação como QA.

---

## 1. O que é DTO?

DTO (Data Transfer Object) é o contrato da API que define o formato dos dados de entrada e saída.  
Ele garante padronização na comunicação entre sistemas.

---

## 2. Request DTO (entrada)

Dados enviados para a API via POST/PUT:

```json
{
  "name": "Erica",
  "email": "erica@email.com"
}
```
---
## 3. Response DTO (saída)

Dados retornados pela API:
```json
{
  "id": 1, 
  "name": "Erica",
  "email": "erica@email.com"
}
```
---

## 4. O que acontece e o que faço com esse dado?

Quando envio "name": "Erica", o backend não procura nada com esse nome no código.

Ele já sabe qual classe usar e apenas pega o objeto (no nosso exemplo, o formato usado é o JSON, mas poderia ser XML, por exemplo) e transforma em um objeto em memória dessa classe, preenchendo os campos com os valores recebidos.

Depois disso, se estiver tudo válido, ele salva no banco de dados.

----

## 5. E o que faço com esse dado?
Essas informações podem ser usadas para criar um cadastro, por exemplo.

Ao cadastrar um usuário e salvar, o backend entra em ação e retorna o response com os dados persistidos.

# Entendendo o que é DTO

Este repositório contém minhas explicações sobre alguns fundamentos de DTO (Data Transfer Object) e o fluxo de comunicação entre frontend, backend e banco de dados, com foco em atuação como QA.

---

## 1. O que é DTO?

DTO (Data Transfer Object) é uma representação do contrato da API utilizada que define o formato dos dados de entrada e saída.  
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
Observação: o campo id é gerado no backend e passa a fazer parte do objeto retornado.

---

## 4. O que acontece e o que faço com esse dado?

Quando envio "name": "Erica", o backend não procura nada com esse nome no código.

O sistema já possui uma estrutura definida e realiza o mapeamento (deserialização) do JSON para um objeto em memória, preenchendo seus atributos com os valores recebidos.

Após isso:
- Os dados são validados conforme regras de negócio
- Caso estejam corretos, são persistidos no banco de dados
- A API retorna um response seguindo o padrão definido

----

## 5. E o que faço com esse dado?
Essas informações podem ser usadas para criar um cadastro, por exemplo.

Ao cadastrar um usuário e salvar, o backend entra em ação, processa os dados e retorna o response com os dados persistidos.

----

## 6. DTO vs Entity
DTO: utilizado para transporte de dados na API (entrada e saída)

Entity: representa a estrutura da tabela no banco de dados

DTO não precisa ter a mesma estrutura dos dados armazenados no banco de dados.

----

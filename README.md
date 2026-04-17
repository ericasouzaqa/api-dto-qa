# Introdução
Este repositório contém minhas explicações sobre alguns fundamentos de DTO (Data Transfer Object) e o fluxo de comunicação entre frontend, backend e banco de dados, com foco em atuação como QA.

Os exemplos neste guia utilizam o formato JSON, que é o padrão de mercado para APIs REST. No entanto, o conceito de DTO se aplica a outros formatos de serialização, como XML, Protocol Buffers (gRPC) ou Avro, dependendo das definições do contrato de interface entre os sistemas.

---

## 1. O que é DTO?
DTO (Data Transfer Object) é um objeto que transporta dados entre processos. Na prática, ele é a representação do contrato da API, definindo exatamente quais campos podem entrar (Request) e quais podem sair (Response).

Ele serve como um filtro de segurança e performance, garantindo que o sistema troque apenas as informações necessárias.

---

## 2. Request DTO (Entrada)
Dados enviados pelo cliente Frontend/Tester) para a API, geralmente via POST, PUT ou PATCH:

```json
{
  "name": "Erica",
  "email": "erica@email.com"
}
```

---

## 3. Response DTO (Saída)
Dados retornados pela API após o processamento:

```json
{
  "id": 1, 
  "name": "Erica",
  "email": "erica@email.com"
}
```
Observação: O campo id geralmente é gerado no backend ou banco de dados e passa a fazer parte do objeto retornado no Response, confirmando que o recurso foi criado ou identificado.

---

## 4. O fluxo do dado: O que acontece por trás?
Quando você envia o JSON "name": "Erica", o sistema realiza um processo em etapas:

*Desserialização:* O backend recebe o texto (JSON) e o transforma em um objeto "vivo" na memória do servidor.

*Validação:* O sistema verifica se os dados seguem as regras de negócio (ex: se o e-mail tem formato válido). 

*Mapeamento:* Caso os dados estejam corretos, o backend converte (mapeia) o DTO em uma Entity para que ela possa ser processada pela camada de dados.

*Persistência:* A Entity é salva no banco de dados.

*Serialização:* O backend gera um DTO de resposta e o transforma de volta em JSON para o cliente.

---

## 5. E o que faço com esse dado?
Essas informações são usadas para executar ações de negócio. 

Por exemplo, ao cadastrar um usuário, o backend processa o Request, persiste as informações e retorna o Response. 

Como QA, comparamos se o que foi enviado no Request e o que está no banco reflete corretamente o que recebemos no Response.

---

## 6. DTO vs Entity
*DTO (Data Transfer Object):* Utilizado para o transporte de dados (entrada e saída). Foca no que o cliente/usuário precisa ver ou enviar.

*Entity:* Representa a estrutura real da tabela no banco de dados.

Por que separar os dois?
O DTO não precisa ter a mesma estrutura da Entity. 

Isso garante segurança, evitando a exposição de dados sensíveis (como senhas, logs de auditoria ou IDs internos) que existem na Entity, mas não devem chegar ao usuário final.

*Imagine o seguinte cenário real:* O Problema da Segurança (Privacidade)

Imagine que a sua tabela de usuários no banco de dados (Entity) tenha os campos:

id
nome
email
senha_hash (senha criptografada)
tentativas_de_login
endereco_ip_ultimo_acesso*

Se você não usar um DTO e retornar a Entity direto na API, o seu JSON de resposta seria assim:

```json
{
  "id": 1,
  "nome": "Erica",
  "email": "erica@email.com",
  "senha_hash": "$2a$12$Kj9...", 
  "tentativas_de_login": 0,
  "endereco_ip_ultimo_acesso": "192.168.0.1"
}
```

Percebe o risco? Você acabou de enviar a senha (mesmo criptografada) e dados internos de segurança para o navegador do usuário.

Com o DTO, você cria um filtro que só deixa passar o que é seguro:

```json
{
  "id": 1,
  "nome": "Erica",
  "email": "erica@email.com"
}
```

*O Problema da Estabilidade (O Contrato)*

Imagine que foi alterado o nome da coluna no banco de nome para nome_completo_do_usuario.

*Sem DTO:* O seu JSON de saída mudaria automaticamente para "nome_completo_do_usuario": "Erica". Isso quebraria o Frontend, porque estava sendo esperando apenas o "nome".

*Com DTO:* O banco muda, a Entity muda, mas o desenvolvedor do Backend faz o mapeamento para que o DTO continue enviando "nome". O "contrato" com o Frontend permanece intacto e nada quebra.

---

## Referência

* [Padrão DTO - Martin Fowler]

https://martinfowler.com/eaaCatalog/dataTransferObject.html

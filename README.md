# Case 02 - Fluxograma - Abertura Automática de Casos via WhatsApp

> Projeto 100% anonimizado para fins de portfólio | LGPD | Nenhum dado real de cliente ou empresa

## 📌 Contexto
Chat de atendimento automotivo onde cliente abre caso no CRM direto pelo WhatsApp, sem intervenção humana.

**Meu papel:** QA Analyst Jr + Process Analyst
- Desenhei fluxograma completo no Figma com todas as tomadas de decisão
- Defini regras de obrigatoriedade por Motivo 1 e Motivo 2
- Validei integração BLIP + CRM

## 🔄 Fluxo que mapeei

**1. Escolha do Motivo do Contato 1**
- Peças, Problemas com Produto, Serviços da Concessionária, Garantia, Indisponibilidade

**2. Validação de Duplicidade (Regra de Ouro)**
- Sistema consulta por VIN/Chassi se já existe caso aberto com mesmo motivo
- SIM: Informa protocolo existente e encerra
- NÃO: Segue fluxo

**3. Memória de Concessionária**
- Consulta via API BLIP qual foi última concessionária do cliente
- Pergunta: Manter ou trocar?

**4. Coleta de Dados Obrigatórios**
- Nome, CPF, E-mail, WhatsApp, Concessionária

**5. Ramificação Motivo 2**
- Ex: Problema Técnico > Ar-condicionado, Freio, Motor, Multimídia
- Inputs: Veículo parado na oficina? Desde quando? Km atual? Primeira vez?

**6. Integração com CRM**
- Canal = WhatsApp
- Gera protocolo + plano de atividades
- Sucesso: Envia protocolo + pesquisa satisfação
- Erro: Mensagem amigável + transbordo humano + log

## ✅ Cenários de Teste que validei

| ID | Cenário | Resultado Esperado |
|---|---|---|
| CT01 | Cliente com VIN já com caso aberto mesmo motivo | Bloqueia duplicidade e informa protocolo |
| CT02 | Cliente com histórico de concessionária | Sugere última concessionária |
| CT03 | Fluxo Garantia com veículo parado | Campos obrigatórios: Km, data entrada oficina |
| CT04 | Falha na integração CRM | Mensagem de erro + transbordo + log |
| CT05 | Cliente novo sem histórico | Pula etapa de memória e pede concessionária |

## 🛠️ Ferramentas
Figma, Asana, Confluence, chat whatsapp, Salesforce/CRM

## 🖼️ Fluxograma
Imagem do fluxograma redesenhado no Figma (versão anonimizada)
`/https://www.figma.com/proto/iTayrNitft04k918SLgXum/Sem-t%C3%ADtulo?node-id=3-4&t=9OWYiJfVkiUfzawI-1`


---
## 🚀 Nível 1 - Automação de Testes de API (Postman)

Implementei a automação da API de abertura de caso para validar o fluxo mapeado.

**O que foi feito:**
- Criei uma requisição `POST https://jsonplaceholder.typicode.com/posts` simulando o endpoint de abertura de caso via WhatsApp.
- Enviei no `Body` dados anonimizados: `vin`, `canal`, `motivo` e `concessionaria`.

**Automação (Postman Scripts - After Response):**
```javascript
pm.test("CT01 - Abertura de caso deve ser 201", function () {
    pm.response.to.have.status(201);
});
pm.test("CT02 - Deve gerar protocolo com ID", function () {
    pm.expect(pm.response.json()).to.have.property('id');
});
pm.test("CT03 - Deve salvar VIN enviado", function () {
    pm.expect(pm.response.json()).to.have.property('vin');
});


---
**Autora:** iCarlk | QA & Process Analyst

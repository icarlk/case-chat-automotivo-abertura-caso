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


## 🚀 Nível 1 - Automação de Testes de API (Postman)

Estudo de automação da API de abertura de caso para validar o fluxo mapeado. (Exercicío)

**O que foi feito:**
- Criei uma requisição POST (https://jsonplaceholder.typicode.com/posts) simulando o endpoint de abertura de caso via WhatsApp.
- Enviei no Body dados anonimizados: vin, canal, motivo e concessionaria.

**Automação (Postman Scripts - After Response):**
    pm.test("CT01...
![Evidencia - 3/3 PASSED](https://github.com/user-attachments/assets/f95b79d6-6ad3-4e15-922f-cdd5a52bac5b)

**Resultado:** ✅ 3/3 PASSED


## 🧪 Nível 2 - Em aprendizado: Testes Negativos e Regra de Ouro

> **Contexto de estudo:** Este é um exercício prático para evoluir de testes de caminho feliz para testes negativos, aplicando a Regra de Ouro que mapeei no fluxo (não duplicar casos para o mesmo VIN).

**O que eu aprendi neste nível:**
- **Conceito de Happy vs Unhappy Path:** Não basta testar quando dá certo (201), preciso testar quando o usuário erra.
- **Por que 409 Conflict:** Entendi que é o código usado quando o sistema tenta criar algo que já existe. No caso, seria barrar um segundo caso aberto para o mesmo chassi.
- **Por que 400 Bad Request:** Quando o body vem sem dado obrigatório (sem VIN), a API deve retornar 400.
- **Case-sensitive:** Descobri na prática que `id` é diferente de `ID` - um detalhe que quebra integração.

**Automação realizada no Postman (After Response):**
- CT01 a CT03: Validação de criação (201 + ID + VIN) - Nível 1
- CT04: Simulação da Regra de Ouro - quando VIN já existe, deveria retornar 409
- CT05: Validação de body sem VIN - deveria retornar 400

**Evidência - 5/5 PASSED (em ambiente de mock):**
![Evidencia N2 - 5/5 PASSED](https://github.com/user-attachments/assets/f3f98d85-fedd-4da5-b683-8ff74c0b4b19)


*Nota: Usei jsonplaceholder como API mock para treino. Numa API real, os cenários CT04 e CT05 validariam os status 409 e 400 reais. O objetivo aqui foi praticar a escrita dos cenários negativos.*

**Resultado:** ✅ Entendi a importância de cobrir cenários negativos e como documentar a Regra de Ouro em automação.
---
**Autora:** 1Car1k | QA & Process Analyst



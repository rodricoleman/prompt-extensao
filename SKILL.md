---
name: prompt-extensao
description: Pega o contexto do trabalho em andamento (conversa, memória, planos, arquivos alterados) e escreve um prompt pronto para colar no Claude in Chrome (a extensão do navegador), que executa sozinho a parte que precisa de navegador — cliques em painéis da Supabase, Vercel, Cloudflare, Google, etc. Use quando o Rodrigo pedir "prompt para a extensão", "manda pra extensão", "gera o prompt pro Chrome", ou /prompt-extensao.
---

# Prompt para a extensão (Claude in Chrome)

Objetivo: transformar o que está sendo feito agora num prompt **autossuficiente** que o Claude in Chrome consiga executar do início ao fim sem ter visto esta conversa.

## 1. Qual tarefa

**Padrão: a tarefa que está sendo falada agora na conversa** — o pedido mais recente do Rodrigo e o que ficou combinado sobre ele. Não varra o projeto inteiro atrás de outras pendências.

- Texto depois de `/prompt-extensao` → é essa a tarefa (ela manda sobre a conversa).
- Só olhe o projeto todo (memória, planos, outras pendências) se ele pedir isso explicitamente ("do projeto todo", "tudo que falta", "todas as pendências").
- Se a conversa não deixa claro qual é a tarefa, pergunte — não invente uma.

## 2. Juntar o contexto

A fonte principal é a conversa: o que foi decidido, o que já foi feito, o que falta clicar.

Memória (`MEMORY.md` e as notas dela), planos em `~/.claude/plans/` e arquivos dos projetos só entram para **completar detalhes dessa tarefa** que a conversa não trouxe — URLs, IDs de projeto, nomes de schema, valores de configuração. Leia o mínimo, sem pedir permissão.

Separe o que é tarefa de **navegador** (painéis web, formulários, contas logadas) do que é tarefa de terminal/código — a extensão só recebe a primeira. Se nada precisa de navegador, diga isso e pare.

Se faltar um dado que só o Rodrigo sabe e que muda o que a extensão faz, pergunte antes de escrever (uma pergunta, curta).

## 3. Escrever o prompt

Em português, direto, no formato abaixo. A extensão não sabe nada do projeto: cada nome, URL, ID e valor tem de estar escrito por extenso.

```
## Objetivo
<uma ou duas frases: o que deve estar verdade quando terminar>

## Contexto
<o mínimo para entender: qual sistema, qual projeto/conta, por que isso está sendo feito>

## Onde
- <URL exata de cada painel> (conta: <qual conta/organização/projeto, com ID>)

## Passos
1. <ação concreta: abrir X, ir em Menu > Submenu, campo "Y" = valor exato>
2. ...
(se um passo depende de algo na tela, diga o que procurar e o que fazer se não achar)

## Como conferir
- <o que deve aparecer na tela quando cada mudança estiver certa>

## Limites
- Não apague, não pague, não compre, não convide ninguém, não mude nada fora da lista acima.
- Antes de qualquer ação irreversível <listar quais, se houver>, pare e me pergunte.
- Se a tela estiver diferente do descrito ou algo der erro, pare e me diga o que viu (não improvise).
- Não copie chaves, senhas ou tokens para o chat.

## No fim, me responda
- O que foi feito, passo a passo (feito / não feito / diferente do esperado).
- Valores que aparecerem na tela e que eu peça para anotar: <quais>.
```

Regras:
- **Nunca** coloque segredos (senhas, service_role, tokens, chaves privadas) no prompt. Se o passo precisa de um, escreva "peça ao Rodrigo" ou indique onde ele está no próprio painel.
- Valores exatos, não aproximados ("600", não "uns 10 minutos").
- Um prompt por sistema quando forem vários painéis independentes; numere-os na ordem em que devem rodar.
- Inclua no "No fim, me responda" o que você precisa saber para continuar o trabalho aqui depois.

## 4. Entregar

1. Salve o prompt em `~/.claude/prompts-extensao/AAAA-MM-DD-<assunto-curto>.md` (crie a pasta se não existir).
2. Mostre o prompt inteiro na resposta, dentro de um bloco de código, para copiar e colar no painel lateral do Claude in Chrome.
3. Uma linha final: "Quando a extensão terminar, cole a resposta dela aqui que eu continuo." — e, quando ela colar, confira o resultado contra o "Como conferir" e atualize a memória do projeto com o que mudou.

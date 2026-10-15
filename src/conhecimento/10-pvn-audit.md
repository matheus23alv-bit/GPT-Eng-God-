---
name: pvn-audit
description: Audita código e entregas dos projetos do Matheus (HTML, CSS, JS, Node, Termux, Firebase, Supabase) sem alterar nada. Classifica achados em problema real, melhoria objetiva ou preferência opcional, com evidência, estado e resultado. Use sempre que ele pedir revisão, auditoria, análise de código, "verifica isso", "tem algum problema?", "está certo?", ao enviar código para avaliar, ou quando houver mais de 2 ajustes no mesmo arquivo ou projeto na sessão. Não use para corrigir sem OK, fechar versão (use pvn-release) ou criar prompts.
---

# pvn-audit · v0.2.0

Status: ATIVO · módulo de conhecimento do GPT ENG GOD (aprovado em 2026-10-15).

## Missão

Diagnóstico confiável e curto: o que quebra,
o que melhora e o que é gosto. Nada é
chamado de funcional sem verificação.

## Regras invioláveis

1. Auditar não autoriza alterar. Zero edição
   durante a auditoria.
2. Correção só após resumo em uma frase e OK.
3. Julgamento próprio. Não concordar nem
   criticar por padrão. Se está bom, dizer
   que está bom.
4. Todo achado tem evidência: `arquivo:linha`
   ou trecho citado.
5. Marcar cada achado como FATO, INFERÊNCIA
   ou HIPÓTESE.
6. Não inventar testes, resultados, hashes
   ou links.
7. Sem refinamento estético sem pedido ou
   ganho claro.
8. Não impor estética nova a projeto com
   identidade aprovada.
9. Um projeto por sessão. Não misturar.

## Fase 0 · Escopo e baseline

➜ Escopo = o que ele enviou ou apontou. Não
  ampliar sem pedido.
➜ Identificar versão (`src/VERSION`) e o que
  deve ser preservado.
➜ Contexto padrão: localhost, Android e
  Termux. Não presumir produção.
➜ Sem acesso ao código: pedir o arquivo.
  Resultado NÃO EXECUTADO até recebê-lo.

## Fase 1 · Varredura

Aplicar só os itens pertinentes ao escopo.

Lógica e JS:

- erro silencioso (`catch` vazio, promise
  sem `.catch`)
- `async` sem `await`, condição de corrida
- listener ou `setInterval` duplicado
- estado global mutável compartilhado
- `JSON.parse` ou `localStorage` sem
  `try/catch`
- `==` em comparação crítica

Interface mobile:

- `viewport` ausente ou zoom bloqueado
- rolagem horizontal da página
- fonte pequena demais (referência 16px)
- alvo de toque pequeno (referência 44px)
- contraste fraco no tema claro
- página vertical longa demais

Dados e segurança:

- chave ou segredo no front-end
- regras Firebase abertas ou RLS do
  Supabase desligada
- entrada do usuário sem validação
- `innerHTML` com dado externo (XSS)

Servidor local e Termux:

- bind em `0.0.0.0` sem necessidade
  (exposição na rede local)
- porta fixa sem checar se está em uso
- caminho absoluto frágil
- `runtime/` dentro de `src/`
- dependência usada e não declarada

Estrutura e versão:

- versão divergente de `src/VERSION`
- duplicado, backup ou temporário
- código morto ou legado
- arquivo fora do padrão `src/` e
  `runtime/`

Regressão:

- comportamento aprovado que mudou

## Fase 2 · Classificação

➜ PROBLEMA REAL: quebra, perda de dado,
  risco de segurança ou regressão.
➜ MELHORIA OBJETIVA: ganho verificável em
  robustez, desempenho ou manutenção.
➜ PREFERÊNCIA OPCIONAL: gosto, sem ganho
  claro.

Desempate: na dúvida, rebaixar uma classe.

Ordenar por impacto dentro de cada classe.

## Fase 3 · Estado e resultado

Estado por achado ou correção:

➜ DOCUMENTADO: descrito, não aplicado
➜ IMPLEMENTADO: aplicado, não verificado
➜ TESTADO: verificado nesta sessão
➜ COMPROVADO: confirmado por ele no
  ambiente dele

Resultado geral:

➜ APROVADO: nenhum problema real
➜ PARCIAL: problema não crítico ou ponto
  não verificável aqui
➜ FALHOU: quebra, perda de dado ou risco
  de segurança
➜ NÃO EXECUTADO: sem acesso ao código

## Fase 4 · Resposta

Limites de leitura (curto por padrão):

➜ PROBLEMA REAL: todos
➜ MELHORIA OBJETIVA: até 5
➜ PREFERÊNCIA OPCIONAL: até 3
➜ Excedente: informar a contagem e oferecer
  expandir.

Ordem:

1. Conclusão: `Resultado: <R> · <projeto>
   v<X.Y.Z>`
2. Problema real.
3. Melhoria objetiva.
4. Preferência opcional.
5. Verificado aqui ➜ ...
   Depende do ambiente dele ➜ ...
6. Como testar: "Passo X" ➜ descrição curta
   ➜ bloco com o comando.
7. Uma linha: corrigir só com OK.

Modelo de achado:

    1. `app.js:42` ➜ descrição curta.
       FATO. Correção: <uma frase>.

Usar `━━━━━━━PVN━━━━━━━` só entre blocos
principais, se houver mais de um. Omitir
classes sem achados.

## Rodapé obrigatório

Contar ajustes por arquivo ou projeto na
sessão. Ao passar de 2, informar a versão
atual e fechar com bloco separado:

    🔎 Pontos de atenção
    Erros: ...
    Bugs: ...
    Código frágil: ...
    Melhorias: ...
    Visual: ...

Item vazio: "nenhum identificado".

## Correção (só após OK)

➜ Arquivo existente: substituição integral,
  código completo, sem "...".
➜ Marcador de versão no arquivo.
➜ Corrigir regressão antes de concluir.
➜ Reauditar o trecho alterado e atualizar o
  estado.

## Entrega de correções (ChatGPT e Codex)

➜ HTML como arquivo `.html` para download.
  Canvas só se ele pedir.
➜ Demais arquivos como link de download,
  nome versionado.
➜ Codex: editar no repositório só após OK.

## Integração

➜ Fechamento de versão é da `pvn-release`.
  Sugerir em uma linha, sem executar.
➜ Não avançar para a próxima fase.

## Erros que esta skill evita

- preferência listada como problema
- achado sem linha ou evidência
- "funciona" sem execução
- auditar e já corrigir
- resposta longa demais para o celular

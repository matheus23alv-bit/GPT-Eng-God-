---
name: pvn-release
description: Fecha versão de projetos PVN e pessoais do Matheus com número único, documentação sincronizada, pacote de changelog e limpeza controlada. Use sempre que ele pedir para fechar, versionar, lançar, empacotar ou subir versão, atualizar VERSION, Changelog.md ou Leia-me.txt, gerar pasta Versões/, ou limpar arquivos substituídos de um projeto, mesmo sem dizer "release". Não use para revisar qualidade de código (use pvn-audit), para deploy externo ou para criar funcionalidade nova.
---

# pvn-release · v0.2.0

Status: ATIVO · módulo de conhecimento do GPT ENG GOD (aprovado em 2026-10-15).

## Missão

Fechar uma versão com:

➜ um único número em todos os pontos
➜ histórico preservado
➜ pacote de versão só com changelog
➜ nenhuma sobra de arquivo substituído

## Regras invioláveis

1. PT-BR, direto e formal. Conclusão primeiro.
2. Um projeto e uma versão por sessão.
3. Não avançar sozinho para outra fase.
4. Não criar escopo, versão ou função não
   pedidos.
5. `src/VERSION` é a única fonte da versão.
6. Nunca alterar entradas antigas do
   changelog.
7. Nunca copiar o projeto para `Versões/`.
8. Sem deploy externo, salvo pedido
   explícito.
9. Não declarar teste que não foi executado.
10. Distinguir fato, inferência e hipótese.

## Portão de autorização

Um único OK cobre o plano inteiro.

Antes de editar, enviar o plano (Fase 2) e
aguardar OK. Depois do OK, executar sem
reconfirmar.

Remoções entram no plano, uma por linha, com
motivo. Remoção descoberta depois do OK
exige novo OK específico.

Mudança de decisão já aprovada exige
autorização específica.

## Fase 0 · Diagnóstico (só leitura)

Coletar:

- projeto e caminho raiz
- versão atual em `src/VERSION`
- versões em manifest, interface e docs
- mudanças desde a última versão: seção
  "Não lançado" do changelog, `git log` ou
  relato dele
- candidatos a remoção: substituídos,
  backup, temporário, legado

Exceções:

➜ Sem `src/VERSION`: avisar e propor criar.
➜ Versões divergentes: listar todas.
  `src/VERSION` prevalece, salvo decisão
  dele.
➜ Mais de um projeto no pedido: perguntar
  qual primeiro.
➜ Sem lista de mudanças: perguntar. Não
  inventar.
➜ Estrutura fora do padrão `src/`: avisar e
  seguir a estrutura existente. Não
  reorganizar sem pedido.

## Fase 1 · Número da versão

Se ele informou, usar. Se não, propor:

➜ PATCH `x.y.Z`: correção sem mudar o uso
➜ MINOR `x.Y.0`: função nova compatível
➜ MAJOR `X.0.0`: quebra de uso ou de dados

Ele decide. Nunca reutilizar número já
lançado.

## Fase 2 · Plano

Modelo:

    Fechar <projeto> v<X.Y.Z>
    (atual v<A.B.C>).
    ➜ Atualizar: VERSION, manifest,
      interface, Changelog.md, Leia-me.txt
    ➜ Criar: Versões/<projeto>_v<X.Y.Z>/
    ➜ Remover: <arquivo> (motivo)
    Confirma com OK?

Omitir linhas que não se aplicam.

## Fase 3 · Execução

Ordem fixa:

1. `src/VERSION`
2. manifest (`package.json`,
   `manifest.json`)
3. versão exibida na interface (rodapé,
   splash, constante como `APP_VERSION`)
4. `src/Changelog.md`
5. `src/Leia-me.txt`
6. `Versões/<projeto>_v<X.Y.Z>/`
7. remoções aprovadas

Arquivo existente: substituição integral,
conteúdo completo, sem "..." nem trecho
solto.

Entrada nova no topo do changelog:

    ## [X.Y.Z] - AAAA-MM-DD
    ### Adicionado
    ### Alterado
    ### Corrigido
    ### Removido

Omitir seções vazias. Usar a data real da
sessão.

Pacote de versão: um único arquivo
`Versões/<projeto>_v<X.Y.Z>/CHANGELOG_v<X.Y.Z>.md`
com versão, data, resumo, mudanças,
arquivos alterados, removidos, riscos e
como testar. Nada além disso.

## Fase 4 · Verificação

Executar quando houver acesso. Sem acesso,
entregar os comandos para ele rodar.

➜ V1 número igual em todos os pontos
➜ V2 entradas antigas do changelog intactas
➜ V3 pacote sem cópia do projeto
➜ V4 sem duplicado, backup ou temporário
➜ V5 arquivo removido sem referência no
  código

Termux (bash):

    cat src/VERSION
    grep -rnF "X.Y.Z" src/
    grep -rnE "[0-9]+\.[0-9]+\.[0-9]+" \
      src/ | grep -v Changelog.md
    find . -iname "*.bak" -o -iname "*~" \
      -o -iname "*copia*" -o -iname "*old*"
    grep -rn "<arquivo-removido>" src/
    git diff src/Changelog.md

Windows (PowerShell):

    Get-Content src\VERSION
    Get-ChildItem src -Recurse -File |
      Select-String -SimpleMatch "X.Y.Z"
    Get-ChildItem -Recurse -File |
      Where-Object Name -match "bak|old|copia"

O segundo `grep` lista todo número no
formato de versão. Revisar só os que deveriam
ser a versão do projeto.

## Fase 5 · Critério de resultado

Estado:

➜ DOCUMENTADO: descrito, não aplicado
➜ IMPLEMENTADO: aplicado, não verificado
➜ TESTADO: verificado nesta sessão
➜ COMPROVADO: confirmado por ele no
  ambiente dele

Resultado:

➜ APROVADO: V1 a V5 passaram
➜ PARCIAL: algum V não executado, ou falha
  não crítica
➜ FALHOU: V1 ou V3 falhou
➜ NÃO EXECUTADO: nada foi verificado

## Fase 6 · Resposta

Ordem:

1. Conclusão: `v<X.Y.Z> fechada · <projeto>`
2. Estado e resultado.
3. Caminhos completos criados ou alterados.
4. Removido ➜ motivo. Mantido ➜ motivo.
5. Riscos.
6. O que foi verificado aqui e o que depende
   do ambiente dele.
7. Como testar: "Passo X" ➜ descrição curta
   ➜ bloco com o comando.

Formato: frases curtas, uma ideia por
parágrafo, ➜ para relações curtas, ⓘ para
observações. Links fora de bloco de código.

## Integração

➜ Sem auditoria nesta sessão: sugerir
  `pvn-audit` em uma linha, sem executar.
➜ Não iniciar a próxima versão.

## Destino e entrega (ChatGPT e Codex)

➜ Com acesso a `G:\Meu Drive\00_Projetos`
  (conector ou Codex local): ler
  `00_LEIA-ME.txt`, preservar a
  organização, salvar no local adequado e
  informar o caminho completo.
➜ ChatGPT sem acesso à pasta: avisar antes e
  entregar link de download do arquivo
  gerado, nome versionado. ZIP só com
  vários arquivos.
➜ Codex: gravar direto no repositório
  indicado.
➜ HTML como arquivo `.html` para download.
  Canvas só se ele pedir.

## Erros que esta skill evita

- atualizar VERSION e esquecer a interface
- reescrever entrada antiga do changelog
- copiar `src/` para `Versões/`
- remover arquivo ainda referenciado
- chamar de TESTADO sem executar
- salvar fora de `00_Projetos` sem aviso

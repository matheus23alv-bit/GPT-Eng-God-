# AGENTS.md · GPT-Eng-God

Guia para agentes (Codex, Claude Code) que
trabalham neste repositório. Vale também
para humanos.

## O que é

Repositório do GPT personalizado ENG GOD 12:
um arquivo de instruções e módulos de
conhecimento. Não há código executável,
build ou testes automatizados.

## Estrutura

    src/VERSION                 única fonte da versão
    src/Changelog.md            histórico (nunca reescrever)
    src/Leia-me.txt             instalação e testes manuais
    src/instrucoes/ENG-GOD-instrucoes.md
                                campo "Instruções" do GPT
    src/conhecimento/NN-*.md    módulos de conhecimento

Numeração dos módulos:

    00-09  roteamento e orquestração
    10-19  skills PVN (audit, release)
    20-29  perfil e regras do operador

## Regras

- PT-BR em todo conteúdo e nas respostas.
  Comandos, nomes de arquivo e código no
  original.
- `src/instrucoes/ENG-GOD-instrucoes.md`
  deve ficar abaixo de 8000 caracteres.
  Conferir com `wc -m` após editar.
- `02-eng-god-12.0.1.md` é texto canônico do
  método. Não editar o conteúdo; uma nova
  versão do método entra como novo arquivo
  e o antigo sai (sem duplicados).
- Módulos 10 e 11 vêm do pacote
  pvn-skills. Alterar só com pedido.
- Sem arquivos duplicados, backup, legado
  ou temporário dentro de `src/`.
- Versão: alterar `src/VERSION` e registrar
  em `src/Changelog.md` no topo, sem tocar
  em entradas antigas. Mudança não lançada
  vai na seção "Não lançado".
- Toda entrega atualiza `src/Leia-me.txt`
  se a estrutura ou a instalação mudar.

## Fluxo de trabalho

1. Ler `src/Leia-me.txt` e este arquivo.
2. Identificar o baseline (`git log`,
   `src/VERSION`).
3. Antes de criar ou editar arquivo:
   resumir o pedido em uma frase e aguardar
   OK do operador.
4. Mudanças incrementais; arquivo existente
   é substituído por inteiro.
5. Commit com mensagem descritiva em PT-BR.
   Não incluir identificador de modelo.
6. Push só para a branch indicada na
   sessão.

## Verificação mínima antes de concluir

    wc -m src/instrucoes/ENG-GOD-instrucoes.md
    cat src/VERSION
    ls src/conhecimento/
    git status --short

## Fora de escopo

- Deploy ou publicação do GPT: é manual,
  feita pelo operador.
- Criar módulos novos sem pedido.
- Alterar preferências do operador
  (módulo 20) por iniciativa própria.

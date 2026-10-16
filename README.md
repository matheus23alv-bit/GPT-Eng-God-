# GPT-Eng-God-

ENG GOD 12 como GPT personalizado: especialista em engenharia de prompts, orquestração de IA, OCR, análise documental, organização inteligente de arquivos, arquitetura de software, auditoria técnica e automações.

Versão: ver `src/VERSION`.

## Estrutura

```
src/
├── VERSION
├── Changelog.md
├── Leia-me.txt
├── instrucoes/
│   └── ENG-GOD-instrucoes.md   (campo "Instruções" do GPT)
└── conhecimento/               (arquivos de "Conhecimento" do GPT)
    ├── 00-roteador.md
    ├── 01-orquestracao.md
    ├── 02-eng-god-12.0.1.md
    ├── 10-pvn-audit.md
    ├── 11-pvn-release.md
    └── 20-perfil-operador.md
```

## Como funciona

As instruções trazem o roteador: cada mensagem é classificada pelo objeto pedido e ativa um módulo de conhecimento.

| Rota | Pedido | Módulo |
|---|---|---|
| R0 | ativação sem tarefa | 02 (cartão e menu) |
| R1 | criar ou melhorar prompt | 02 ENG GOD |
| R2 | resultado direto sobre material | 02 (exceção) |
| R3 | auditoria de código | 10 pvn-audit |
| R4 | fechar versão | 11 pvn-release |
| R5 | várias fases ou pipeline de IA | 01 orquestração |
| R6 | demais tarefas técnicas | 02 como TASK |

O módulo 20 governa comunicação, escopo, qualidade e entrega em todas as rotas.

## Instalação

Passos em `src/Leia-me.txt`.

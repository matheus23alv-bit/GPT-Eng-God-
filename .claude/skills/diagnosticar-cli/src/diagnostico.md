# Referências por ambiente

## Seleção de testes

Selecionar somente comandos pertinentes. Substituir `<cli>` por nome confirmado, sem interpolar dados não confiáveis. Não misturar shells. Evitar saídas com credenciais; aliases e funções também podem conter segredos.

PowerShell:
```powershell
$PSVersionTable.PSVersion
Get-Location
Get-Command node,npm -All | Select-Object CommandType,Name,Source
node -p "JSON.stringify({platform:process.platform,arch:process.arch,execPath:process.execPath,version:process.version})"
npm config get prefix
```
Usar `where.exe <cli>` para arquivos no PATH, lembrando que não revela funções/aliases do PowerShell. Inspecionar Definition apenas se necessário e com saída saneada.

CMD:
```bat
ver
cd
where node
where npm
node -p "JSON.stringify({platform:process.platform,arch:process.arch,execPath:process.execPath,version:process.version})"
npm config get prefix
```

Bash em Linux/WSL/Termux/proot:
```bash
pwd
uname -srm
type -a node npm
node -p 'JSON.stringify({platform:process.platform,arch:process.arch,execPath:process.execPath,version:process.version})'
npm config get prefix
```
`type -a` pode revelar corpos de funções; pedir saída saneada. Em WSL, consultar `/proc/sys/kernel/osrelease` se a camada estiver incerta. No host Termux, verificar `printf '%s\n' "$PREFIX"` e `command -v proot-distro` quando necessário; não presumir esses valores no Ubuntu. Preferir origem do executável e prefixo npm ao PATH integral.

## Windows e WSL

- Módulo em `/mnt/c/Users/.../AppData/Roaming/npm` com processo Linux indica fortemente mistura de instalações, mas não confirma sozinho qual Node foi chamado. Conferir plataforma, execPath, resolução e prefixo.
- Comparar CLI instalada com plataforma e arquitetura reais. Não compartilhar node_modules com dependências nativas entre Windows e Linux nem presumir que instalação no Windows resolve WSL.
- Antes de alterar PATH, investigar alias, função, shim, link, cache de resolução e gerenciador de versões. Preferir seleção do runtime na sessão. `hash -r` é pertinente ao Bash, não universal.
- “Acesso negado” pode ser ACL, arquivo bloqueado, diretório protegido, política de execução ou daemon. Não elevar tudo nem mudar política global sem identificar o alvo.

## Termux e proot-distro

- Distinguir caminhos Android, host Termux e Ubuntu convidado; confirmar usuário, bind mount e diretório visível antes de `cd`. Armazenamento compartilhado pode limitar execução, permissões e links. Sugerir migração para armazenamento interno somente se sustentada por testes.
- Em `proot-distro login ubuntu -- bash "cd Bidel-Agent-Personal"`, Bash recebe um nome de arquivo, não um comando. `bash -c` executa string; `bash -lc` também carrega inicialização de login e pode alterar PATH.
- Para entrar e permanecer no projeto, preferir `--work-dir` se confirmado no `proot-distro login --help` instalado. Alternativa com caminho confirmado: `proot-distro login ubuntu -- bash -c 'cd /caminho/confirmado && exec bash -i'`. Substituir caminho e confirmar binds/usuário; não executar automaticamente nem presumir `/root`.
- Investigar suporte a kernel, daemon e systemd quando necessário. ARM64 não garante compatibilidade de libc: Android/Termux e Ubuntu/glibc são ambientes distintos.

## Node/npm e dependências

- Separar runtime, gerenciador, pacote e binário nativo. Confirmar Node, arquitetura, caminho npm, prefixo e origem CLI. Usar `npm ls -g --depth=0 <pacote>` ou equivalente local quando pertinente.
- Missing optional dependency pode decorrer de plataforma misturada, instalação incompleta, opcionais omitidos, lockfile, arquitetura ou publicação sem binário compatível. Confirmar antes de reinstalar. Consultar `npm config get omit` ou `npm config get optional` somente quando pertinente e suportado.
- Inspecionar somente chaves necessárias e saneadas do npm; não imprimir `.npmrc`. Separar DNS/proxy/TLS, pacote ausente e autenticação de registry. Não desativar TLS como correção.
- Respeitar packageManager, Node declarado e lockfile do projeto. Não misturar gerenciadores nem apagar lockfiles indiscriminadamente. Instalações podem executar scripts e precisam de autorização. Antes de reinstalação global, confirmar runtime e prefixo.

## Autenticação e serviços

- Separar credenciais CLI, registry, Git e serviço. Identificar conta pretendida sem coletar segredo; usar status oficial disponível na versão instalada.
- Token que não pode ser renovado após logout/troca de conta sugere sessão invalidada. Não atribuir a PATH sem evidência. Conferir login oficial e explicar alcance do logout antes de executar.
- Diferenciar expiração, permissão insuficiente, indisponibilidade, proxy/rede e relógio incorreto. 401/403 isolado não justifica reinstalação.
- Separar cliente e daemon. Conferir na versão instalada flags sugeridas pelo erro. Funcionamento sem daemon é contorno e evidência para investigar o serviço, não confirmação de causa resolvida.

## Validação

| Falha | Evidência decisiva | Critério de sucesso |
|---|---|---|
| PATH/runtime | Origem e plataforma | CLI correta também em nova sessão |
| Dependência nativa | Plataforma, arquitetura e pacote | Binário carrega e operação funciona |
| Diretório/proot | Caminho no convidado e semântica shell | Shell permanece no diretório certo |
| Autenticação | Conta e resposta do serviço | Operação autenticada sem efeito externo |
| Permissão/daemon | Alvo e comparação entre modos | Operação permitida no modo pretendido |

Aplicar somente a linha pertinente. Distinguir confirmação relatada pelo usuário de observação direta. Se houver alteração persistente, validar também após reabrir o terminal.

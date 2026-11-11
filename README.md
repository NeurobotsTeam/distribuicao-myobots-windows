# Myobots Windows — Distribuição

Repositório **público** de distribuição do **Myobots para Windows** (Neurobots).

Aqui ficam só os **instaladores** e os arquivos do **auto update**. O código-fonte do app fica no repositório privado `NeurobotsTeam/myobots-windows`.

## Download

Baixe o instalador mais recente em **[Releases](https://github.com/NeurobotsTeam/distribuicao-myobots-windows/releases/latest)**: o arquivo `Myobots-Setup-X.Y.Z.exe`.

- Windows 10/11, 64 bits.
- Depois de instalado, o app se atualiza sozinho.

## Como funciona o auto update

O Myobots usa `electron-updater`, que lê as releases deste repositório:

1. Ao abrir, o app checa a release mais recente e lê o `latest.yml`.
2. Se houver versão nova, ela é baixada em segundo plano (o download fica pausado durante as sessões de jogo).
3. Quando o download termina, aparece **"Nova versão do Myobots disponível!"** com **Instalar Agora** / **Depois**.
4. **Instalar Agora** fecha o app, roda o instalador e abre a versão nova.

Cada release precisa ter estes três arquivos (gerados automaticamente):

| Arquivo | Para que serve |
|---|---|
| `Myobots-Setup-X.Y.Z.exe` | Instalador NSIS |
| `Myobots-Setup-X.Y.Z.exe.blockmap` | Download diferencial |
| `latest.yml` | Versão, hash e tamanho; é o que o app consulta |

> ⚠️ Não apague nem renomeie esses arquivos nas releases publicadas, senão o auto update dos clientes quebra.

## Como publicar uma nova versão (equipe)

As releases são criadas pelo script `deploy_myobots.py` do repositório **Automação de Tarefas**. Não suba os arquivos à mão.

```powershell
# precisa de GH_TOKEN (PAT com acesso a este repo) nas variáveis de ambiente
python deploy_myobots.py                         # interativo
python deploy_myobots.py --version 2.2.2 --publish -y
```

O script:
1. Atualiza `version` no `package.json` do Myobots.
2. Roda `npm run deploy:win:gh` (electron-builder `--publish always`).
3. Cria a release **draft** `vX.Y.Z` aqui, com o instalador, o `.blockmap` e o `latest.yml`.

Depois disso:
- Revise o draft em [Releases](https://github.com/NeurobotsTeam/distribuicao-myobots-windows/releases) e clique em **Publish release**. Os clientes só recebem o update depois que a release é publicada.
- A versão precisa ser **maior** que a anterior (semver `X.Y.Z`) para os clientes receberem o update.

## Observações

- Este repositório precisa continuar **público**, porque o app instalado não leva token do GitHub.
- O instalador não é assinado digitalmente, então o Windows SmartScreen pode mostrar um aviso na primeira instalação ("Mais informações" → "Executar assim mesmo").
- Logs do updater no cliente: `%APPDATA%\Myobots\logs\main.log`.

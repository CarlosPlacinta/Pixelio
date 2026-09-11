# Pixelio

Pixelio evaluates Habbo inventories, rooms, and unlocked wardrobe items.
Available in Brazilian Portuguese and English for Windows 10/11 x64.

**Latest version: [1.2.0](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.0)** · **[Full changelog](CHANGELOG.md)**

The changelog records the app's development from the original inventory scanner,
including features, visual refinements, fixes, experiments, and releases.

## Upcoming version 1.2.8 — not yet downloadable

The next release includes room-capture and scan-speed fixes, persistent price
estimates, NFT and BC/CA tags and filters, Windows installer support, and update
notifications while Pixelio is open.

The application changes and isolated installer checks have been tested locally.
The full Windows package is **blocked**: Windows flagged the existing furniture
renderer, and that executable is now missing from the build environment. This
release will not be published to the updater until the dependency issue is
resolved and the complete package passes its installation checks.

**1.2.0 remains the latest public download.** The new features described in the
[changelog](CHANGELOG.md) are not yet available through GitHub updates.

## Download

Download **Pixelio.exe** from the [latest release](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
Alternatively, extract **Pixelio-Windows-x64.zip** and open Pixelio.exe. The ZIP also
includes the changelog, instructions, and third-party notices.

1. Open Pixelio and click **Connect account**.
2. If **Set up connection** appears, approve the Windows setup prompt once.
3. Enter **Habbo Classic (AIR)**. When Pixelio identifies the account, choose an
   inventory, room, or wardrobe scan.

The G-Earth connection engine and Java runtime are included. No separate G-Earth,
Java, or Python installation is needed. The one-time connection setup installs a
small helper on the PC; normal subsequent connections do not request permission
for javaw.exe. Remove the helper under **Connections > Remove connection setup**.

Opening Pixelio shows saved results without starting a scan. Closing or updating
Pixelio ends its built-in Habbo connection; reconnect to Habbo afterward. External
G-Earth connections remain available under Connections.

## Updates and saved data

From version 1.1.0, Pixelio checks for updates when it opens. Choose **Update now**
or **Later** when a new version is available. Saved scans and settings are kept.
Finish or stop active scans before updating. Versions older than 1.1.0 need the
latest executable once to enable future update prompts.

Each Windows user's data stays in `%LOCALAPPDATA%\HabboInventoryScanner`.
The release contains no personal accounts, scans, or settings.

## Português

O Pixelio avalia inventários, quartos e visuais desbloqueados do Habbo.
Disponível em PT-BR e inglês para Windows 10/11 x64.

**Versão atual: [1.2.0](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.0)** · **[Histórico completo de mudanças](CHANGELOG.md)**

### Próxima versão 1.2.8 — ainda indisponível para download

A próxima versão reúne correções no scan de quartos e na velocidade das consultas,
estimativas com preços salvos, tags e filtros NFT e BC/CA, suporte a instalador
Windows e avisos de atualização enquanto o Pixelio estiver aberto.

As mudanças e os testes isolados do instalador passaram localmente. O pacote
completo está **bloqueado**: o Windows sinalizou o renderizador de imagens de mobis,
e esse executável está ausente no ambiente de compilação. A versão só será
publicada no atualizador após resolver essa dependência e validar o pacote completo.

**A versão 1.2.0 continua sendo o download público mais recente.** As novidades do
[changelog](CHANGELOG.md) ainda não estão disponíveis pelo atualizador.

Baixe **Pixelio.exe** na [versão mais recente](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
Também é possível extrair **Pixelio-Windows-x64.zip** e abrir Pixelio.exe. O ZIP
inclui o histórico de mudanças, as instruções e os avisos de terceiros.

1. Abra o Pixelio e clique em **Conectar conta**.
2. Se aparecer **Configurar conexão**, confirme a solicitação do Windows uma vez.
3. Entre no **Habbo Classic (AIR)**. Quando a conta for identificada, escolha o
   scan do inventário, quarto ou visuais.

O mecanismo de conexão do G-Earth e o Java estão incluídos. Não é necessário
instalar G-Earth, Java ou Python separadamente. A configuração inicial instala
um pequeno componente neste PC; as conexões seguintes não pedem permissão para
javaw.exe. Remova-o em **Conexões > Remover configuração**.

Ao abrir, o Pixelio mostra os resultados salvos sem iniciar um scan. Fechar ou
atualizar o aplicativo encerra a conexão integrada; reconecte ao Habbo depois.
Conexões externas do G-Earth continuam disponíveis em Conexões.

A partir da versão 1.1.0, o Pixelio verifica atualizações ao abrir. Escolha
**Atualizar agora** ou **Mais tarde**. Conclua ou pare os scans antes de atualizar.
Seus scans e configurações são mantidos. Versões anteriores à 1.1.0 precisam do
executável mais recente uma vez para ativar as próximas atualizações.

Os dados de cada usuário ficam em `%LOCALAPPDATA%\HabboInventoryScanner`.
Esta distribuição não inclui contas, scans ou configurações pessoais.

Pixelio is an independent application. Habbo artwork belongs to its owners.
The embedded connection engine is based on G-Earth. See THIRD_PARTY_NOTICES in
the ZIP for engine, connection helper, runtime, artwork, and renderer notices.

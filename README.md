# Pixelio

Pixelio evaluates Habbo inventories, rooms and unlocked wardrobe items.
Available in Brazilian Portuguese and English for Windows 10/11 x64.

**Latest version: [1.2.11](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.11)** · **[Full changelog](CHANGELOG.md)**

## Install

Download **Pixelio-Setup-1.2.11-x64.exe** from the
[latest release](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
The installer adds Pixelio to **Start** and **Installed apps**, and offers an
optional desktop shortcut. Right-click Pixelio in Start to pin it to Start or
the taskbar. No separate Python, Go, Java or G-Earth installation is needed.

The portable **Pixelio.exe** and **Pixelio-Windows-x64.zip** remain available.
Each release includes checksums and third-party notices in the portable ZIP.

1. Open Pixelio and click **Connect account**.
2. If **Set up connection** appears, approve the Windows setup prompt once.
3. Enter **Habbo Classic (AIR)** and choose an inventory, room or wardrobe scan.

Opening Pixelio shows saved results without automatically starting a scan.
Closing or updating Pixelio ends its built-in Habbo connection; reconnect afterward.
External G-Earth connections remain available under Connections.

## What's new in 1.2.11

- Remove the top furniture/account subtitle and the extra saved-scan, partial-scan, before-fees and generic connection captions.
- Move furniture quantity and type count into the Items included card, and shorten the average-value card title to Average / Média.

## Included features

- Windows installer and Start/taskbar integration.
- Update checks at launch and every ten minutes while open.
- NFT and BC/CA tags, category filters, and clearer marketplace availability.
- Persistent price estimates and image caching; fresh prices replace estimates
  as each scan progresses.
- Complete furniture capture in large rooms, improved scan scheduling and
  connection-setup recovery.
- Rebuilt and verified furniture renderer, with bounded decompression and
  malformed-artwork handling. See the changelog for the earlier Windows detection
  and the verification performed on its replacement.

## Updates and saved data

Choose **Update now** or **Later** when a newer release is available.
Later dismisses that version for the current session. Newer releases can still
notify you. Finish or stop active scans before installing an update.

Existing versions with the updater can receive 1.2.11 when they next open.
To switch from a portable copy to the installed app, close Pixelio, run Setup,
then open the new Start menu entry. Your existing profile is reused.

Data stays in `%LOCALAPPDATA%\HabboInventoryScanner`. Updating and uninstalling
preserve saved scans, settings and cached artwork. The separately authorized
connection helper can be removed through **Connections > Remove connection setup**
before uninstalling, if it is no longer needed.

## Português

O Pixelio avalia inventários, quartos e visuais desbloqueados do Habbo.
Disponível em PT-BR e inglês para Windows 10/11 x64.

**Versão atual: [1.2.11](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.11)** · **[Histórico completo](CHANGELOG.md)**

Baixe **Pixelio-Setup-1.2.11-x64.exe** na
[versão mais recente](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
O instalador adiciona o Pixelio ao **Iniciar** e aos **Aplicativos instalados**,
com atalho opcional na área de trabalho. Clique com o botão direito no Pixelio
no Iniciar para fixá-lo no Iniciar ou na barra de tarefas.
O executável e o ZIP portáteis continuam disponíveis.

Abra o Pixelio, clique em **Conectar conta**, conclua **Configurar conexão** se
necessário e entre no **Habbo Classic (AIR)**. Não é necessário instalar
G-Earth, Java, Go ou Python separadamente.

A versão 1.2.11 remove as legendas adicionais do painel, coloca a contagem “x mobis · x tipos de mobis” no cartão Mobis incluídos e simplifica o título Valor médio para Média.

O Pixelio inclui instalador Windows, avisos de atualização ao abrir e a cada
dez minutos, tags e filtros NFT/CA, estimativas com preços salvos, correções no
scan de quartos e um renderizador de imagens recompilado e verificado.

Escolha **Atualizar agora** ou **Mais tarde**. Conclua ou pare scans antes de
instalar. Atualizações preservam seus dados; o app reinicia e será necessário
reconectar ao Habbo. Mais tarde dispensa essa versão durante a sessão.

Para trocar a versão portátil pelo aplicativo instalado, feche o Pixelio,
execute o instalador e abra a nova entrada no Iniciar. O perfil existente é
reutilizado em `%LOCALAPPDATA%\HabboInventoryScanner`.
Desinstalar preserva scans, configurações e imagens salvas. Se não precisar mais
do componente de conexão, remova-o em **Conexões > Remover configuração** antes
de desinstalar.

Pixelio is independent. Habbo artwork belongs to its owners. The embedded engine
is based on G-Earth. Runtime and dependency notices are included in the portable ZIP.
This repository distributes application releases and documentation, not personal
accounts, scans or settings.




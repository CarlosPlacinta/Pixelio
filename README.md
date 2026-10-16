# Pixelio

Pixelio evaluates Habbo inventories, rooms and unlocked wardrobe items.
Available in Brazilian Portuguese and English for Windows 10/11 x64.

**Latest version: [1.3.122](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.3.122)** · **[Full changelog](CHANGELOG.md)**

## Install

Download **Pixelio-Setup-1.3.122-x64.exe** from the
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

## What's new since 1.3.26

- **Room copy:** a Room copy tab saves a room's layout and pastes it into another room, using
  your inventory first and C.A. (Builders Club) furni when enabled. It shows a preview and a
  review before anything is placed, and lets you pause, stop, adjust the speed and recover.
  No purchases are made.
- **Wired catalogue:** every Wired in Habbo's furni list, sorted by kind, with new Wireds marked
  as they appear. Select one to see its picture and play its animation. Wired effects with a
  precise delay now load in the Wired tab.
- **Wired timing tools:** Measure arrivals and Calibrar chegada compare your avatar's arrival
  with a furni's movement, and learn from your normal clicks and steps.
  See the [Wired and Watch & Walk guide](PIXELIO_WIRED.md).
- **Looks:** Try-On and My clothing redesigned after Habbo's avatar editor, with Habbo's own
  icons, a sliding wardrobe, every look on Habbo's mannequin, and clothing effects that play at
  Habbo's speed.
- **Inventory and Room:** a shared redesign with whole rows, smaller icons, smooth scrolling and
  a type for every furni (LTD, NFT, C.A., credit furni with its exchange value, rare, HC, Wired
  and more).
- **Price history:** Pixelio keeps each furni's daily marketplace prices and shows an all-time
  average with a chart.
- **Search** ignores accents and word order everywhere.
- **Connection:** your account is detected on its own, and scanning works with Habbo's October
  client.
- **Bug reports:** if something goes wrong, Pixelio saves a report with a short code you can share.
- Smoother animations throughout: tabs, dropdowns, scrolling and the scan button.

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

Existing versions with the updater can receive 1.3.122 when they next open.
To switch from a portable copy to the installed app, close Pixelio, run Setup,
then open the new Start menu entry. Your existing profile is reused.

Data stays in `%LOCALAPPDATA%\HabboInventoryScanner`. Updating and uninstalling
preserve saved scans, settings and cached artwork. The separately authorized
connection helper can be removed through **Connections > Remove connection setup**
before uninstalling, if it is no longer needed.

## Português

O Pixelio avalia inventários, quartos e visuais desbloqueados do Habbo.
Disponível em PT-BR e inglês para Windows 10/11 x64.

**Versão atual: [1.3.122](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.3.122)** · **[Histórico completo](CHANGELOG.md)**

Baixe **Pixelio-Setup-1.3.122-x64.exe** na
[versão mais recente](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
O instalador adiciona o Pixelio ao **Iniciar** e aos **Aplicativos instalados**,
com atalho opcional na área de trabalho. Clique com o botão direito no Pixelio
no Iniciar para fixá-lo no Iniciar ou na barra de tarefas.
O executável e o ZIP portáteis continuam disponíveis.

Abra o Pixelio, clique em **Conectar conta**, conclua **Configurar conexão** se
necessário e entre no **Habbo Classic (AIR)**. Não é necessário instalar
G-Earth, Java, Go ou Python separadamente.

Novidades desde a 1.3.26:

- **Copiar quarto:** a aba Copiar quarto salva o layout de um quarto e o monta em outro,
  usando primeiro o seu inventário e, se ativado, mobis do C.A. Mostra uma prévia e uma revisão
  antes de colocar qualquer mobi, e permite pausar, parar, ajustar a velocidade e recuperar.
  Nenhuma compra é feita.
- **Catálogo de Wireds:** todos os Wireds da lista de mobis do Habbo, por tipo, com os novos
  marcados quando aparecem. Selecione um para ver a imagem e tocar a animação. Efeitos Wired com
  atraso preciso agora carregam na aba Wired.
- **Ferramentas de tempo Wired:** Medir chegada e Calibrar chegada comparam a chegada do seu
  avatar com o movimento de um mobi.
- **Visuais:** Experimentar e Minhas roupas redesenhados como o editor de avatar do Habbo, com os
  ícones do Habbo, guarda-roupa deslizante, manequim do Habbo e efeitos de roupa no ritmo do Habbo.
- **Inventário e Quarto:** novo layout com linhas inteiras, ícones menores, rolagem suave e o tipo
  de cada mobi (LTD, NFT, C.A., câmbio com o valor em créditos, raro, HC, Wired e outros).
- **Histórico de preços:** o Pixelio guarda os preços diários do mercado de cada mobi e mostra a
  média de todo o período com um gráfico.
- **A busca** ignora acentos e a ordem das palavras.
- **Conexão:** a conta é detectada sozinha, e o scan funciona com o cliente de outubro do Habbo.
- **Relatórios de erro:** se algo der errado, o Pixelio salva um relatório com um código curto.

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

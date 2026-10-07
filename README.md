# Farol

Uma ilha na borda da tela que mostra as suas sessões do Claude Code. Cada sessão é um barco, e o mascote
Lumi (o farol) acende quando uma delas termina, trava ou espera por você. Dali mesmo dá para aprovar permissões,
responder perguntas e voltar para a sessão.

## Para quem serve

- **Fedora 44 com KDE Plasma 6 no Wayland:** versão 1.0.1.
- **Windows 10 e 11 (64 bits):** versão 1.1.0, uma **prévia**. Veja [Windows (prévia)](#windows-prévia) para o que já
  funciona e o que ainda falta.
- Não funciona no GNOME nem no macOS.
- Claude Code instalado.

## Baixar

| Arquivo | Para | Versão | SHA-256 |
|---|---|---|---|
| [`farol-1.0.1-1.x86_64.rpm`](https://github.com/schneiderjaoo/farol-release/raw/main/farol-1.0.1-1.x86_64.rpm) | Fedora 44 KDE | 1.0.1 | `78b6e60979cfe9f296981f1bae7dcd7ece72df09e24bcb300d75a2c3ffee77b9` |
| [`Farol-1.1.0-windows-x64.zip`](https://github.com/schneiderjaoo/farol-release/raw/main/Farol-1.1.0-windows-x64.zip) | Windows 10 e 11 | 1.1.0 (prévia) | `fb104abc34c121b242879f3acf209f638464511a58ff84e5d1acc0140f0db6dd` |

Ainda não há pacote `.deb`.

## Windows (prévia)

É a primeira versão do Farol para Windows. O principal funciona: ver as sessões, aprovar permissões e responder
perguntas pela ilha. O resto está na lista do que ainda falta, mais abaixo.

### Antes de baixar

- **Python 3** instalado: é ele que roda o hook que liga o Claude Code ao Farol. Se não tiver, instale pelo
  [python.org](https://www.python.org/downloads/windows/) ou com `winget install Python.Python.3.12`. O "python" que
  já vem no Windows é só um atalho para a loja e não serve.
- **O `farol.exe` não é assinado.** Na primeira vez o Windows mostra "O Windows protegeu o computador": clique em
  **Mais informações → Executar assim mesmo**. Num computador com o **Smart App Control** ligado (Segurança do Windows
  → Controle de aplicativos e do navegador), o Windows bloqueia o Farol e não oferece essa opção: ali esta prévia não
  roda.

### Instalar

1. Baixe o `Farol-1.1.0-windows-x64.zip`. Para conferir o arquivo, no PowerShell:

   ```powershell
   Get-FileHash .\Farol-1.1.0-windows-x64.zip -Algorithm SHA256
   ```

2. Extraia o zip inteiro numa pasta sua, por exemplo `C:\Users\<você>\Farol`. Não rode de dentro do zip.
3. Dê dois cliques em `farol.exe`. A ilha fica no topo da tela e o ícone do farol na área de notificação, perto do
   relógio (pode estar atrás da setinha `^`).

Não precisa ser administrador e nada é instalado fora dessa pasta.

### Ligar ao Claude Code

1. Clique com o botão direito no ícone do farol e escolha **Claude Code hooks → Install…**.
2. O Farol mostra o que vai mudar em `C:\Users\<você>\.claude\settings.json`, faz um backup ao lado e só grava
   depois que você confirma.
3. Abra uma sessão nova do Claude Code: ela aparece como um barco na ilha. As sessões que já estavam abertas só
   aparecem depois de reabertas.

### O que já funciona

- A ilha pequena com o Lumi e um barco por sessão, e a ilha aberta com a sessão em foco, os últimos passos, o uso do
  contexto e a lista de sessões.
- Pedidos de permissão (Permitir, Negar, Sempre, Decidir no editor) e perguntas do Claude, respondidos pela ilha.
- A mensagem final de cada turno e o diff das edições.
- As abas Perguntar (com o serviço "Claude Code", que usa a conta em que o seu Claude Code está conectado), Skills e
  plugins, e Ajustes (tema, posição na tela, lembretes).

### O que ainda não funciona

- **Ir à sessão** e **Nova sessão**: os botões existem, mas ainda não trazem a janela do editor para a frente.
- **Atalhos de teclado globais** (Ajustes → Atalhos): toda combinação é recusada.
- **Notificações quando você está ausente.**
- **Chave de API** em Ajustes → IA: não é salva, então Anthropic, ChatGPT e Gemini não respondem na aba Perguntar.
- **Sons:** não acompanham esta versão.
- O facho de luz do Lumi pode aparecer cortado na ilha pequena.
- Só foi testado com o Claude Code no **VS Code**, no Windows 11.

### Abrir junto com o Windows

Aperte `Win+R`, digite `shell:startup` e, na pasta que abrir, crie um atalho para o `farol.exe`.

### Tirar

1. Pelo ícone do farol: **Claude Code hooks → Uninstall…**, e depois **Quit**.
2. Apague a pasta onde você extraiu o zip e a pasta `%LOCALAPPDATA%\Farol`, onde ficam os ajustes e o hook.

## Fedora

### O que mudou na 1.0.1

**Correções de segurança: quem instalou a 1.0 deve atualizar.** O pacote da 1.0 saiu desta página.

**1. Um texto vindo de fora podia fazer o Farol acessar a internet.** Bastava o texto trazer uma imagem: a resposta de
um serviço na aba Perguntar, ou o comando de um pedido de permissão mostrado na ilha. O Farol buscava o endereço da
imagem sem nenhum clique seu. Como esse endereço pode levar dados junto, um arquivo com instruções escondidas para o
modelo podia usar esse caminho para mandar para fora trechos da conversa. Na 1.0.1:

- Todo texto que vem de fora (de um modelo, de uma sessão, de um nome de arquivo, de um marketplace) é mostrado como
  texto puro, nunca como marcação.
- A resposta da aba Perguntar continua formatada (títulos, listas, tabelas, código), mas quem monta a formatação é o
  próprio Farol. Uma imagem vira o texto `[imagem: …]` e nada é buscado.
- Um link só abre com o seu clique e só quando é `http` ou `https`. Enquanto o ponteiro está sobre ele, a linha embaixo
  da conversa mostra o site para onde ele leva, seja qual for o texto do link.
- A interface da ilha não consegue mais buscar nada na rede. Da aba Perguntar só sai a sua pergunta.
- O texto das notificações da área de trabalho vai como texto puro.

**2. "Sempre" valia para mais do que você aprovou.** Num pedido de permissão com um comando de 400 caracteres ou mais,
o botão **Sempre** guardava só o começo do comando. Depois disso, outro comando com o mesmo começo era aprovado sem
aparecer na ilha. Agora "Sempre" vale para o comando inteiro. As regras que a 1.0 guardou para comandos longos deixam de
valer: o Farol volta a perguntar.

**3. O cartão de permissão podia esconder parte do comando.** O cartão tem duas linhas, e linhas em branco ou
caracteres invisíveis empurravam o resto do comando para fora dele. Agora uma quebra de linha aparece como `↵`, os
caracteres invisíveis são ignorados, e quando o cartão não mostra tudo ele avisa, por exemplo: "Cortado: 211 caracteres
em 4 linhas. Veja inteiro no editor." **Permitir** continua valendo para o comando inteiro.

Também nesta versão:

- **Perguntar sem chave de API:** o serviço "Claude Code" responde com a conta em que o seu Claude Code já está
  conectado. É o serviço padrão; Anthropic, ChatGPT e Gemini continuam disponíveis com chave de API.
- **Copiar:** cada resposta tem um botão que copia o texto dela, em Markdown.
- Uma resposta que começa com uma linha `---` não perde mais o primeiro bloco.

### Instalar ou atualizar

Baixe o arquivo e, na pasta onde ele ficou:

```sh
sudo dnf install ./farol-1.0.1-1.x86_64.rpm
```

Ou direto pelo link:

```sh
sudo dnf install https://github.com/schneiderjaoo/farol-release/raw/main/farol-1.0.1-1.x86_64.rpm
```

Para conferir o arquivo antes de instalar, compare o resultado com o SHA-256 da tabela:

```sh
sha256sum farol-1.0.1-1.x86_64.rpm
```

Se a 1.0 estava aberta, feche-a pela bandeja (**Quit**) e abra o Farol de novo: até lá quem roda é a versão antiga.

### Primeiro uso

1. Abra o Farol pelo menu de aplicativos. A ilha fica no topo da tela e o ícone na bandeja.
2. Clique com o botão direito no ícone da bandeja e escolha **Claude Code hooks → Install…**. O Farol mostra o que vai
   mudar em `~/.claude/settings.json`, faz um backup e só grava depois que você confirma.
3. Abra uma sessão nova do Claude Code. Ela aparece como um barco na ilha.

Quem atualiza da 1.0 não precisa instalar os hooks de novo.

## O que há nesta versão

- **Ilha pequena:** o Lumi e um barco por sessão aberta, numa cena de noite no tema escuro e de dia no tema claro. A
  flâmula de cada barco mostra o estado da sessão, e uma etiqueta avisa quando alguma espera por você ou parou com erro.
- **Sessões:** a sessão em foco com os últimos passos e o uso do contexto, a lista de todas as sessões e o botão para
  voltar à janela do editor.
- **Permissões e perguntas:** respondidas pela ilha, sempre com um clique ou Enter seus.
- **Nova sessão:** lista os repositórios da sua pasta de trabalho e abre um deles no editor com uma sessão nova.
- **Perguntar:** solte arquivos (texto, código, imagem ou PDF) na ilha e pergunte sobre eles ao serviço que você
  escolher: o Claude Code da sua conta, sem chave, ou Anthropic, ChatGPT e Gemini com a sua chave de API.
- **Skills e plugins:** mantém as skills de uma pasta (por exemplo o repositório de skills da sua empresa) ligadas ao
  Claude Code e atualizadas pelo git, e gerencia os plugins do Claude Code.
- **Ajustes:** tema, posição na tela, atalhos de teclado globais, lembretes e notificações.

## Bom saber

- **Sons:** os arquivos de som não acompanham esta versão; o Farol funciona em silêncio.
- **Chaves de API:** ficam no cofre de senhas do sistema (KWallet ou GNOME Keyring), nunca num arquivo do Farol.
- **Rede:** o Farol só fala com a internet quando você faz uma pergunta na aba Perguntar, atualiza a pasta de skills ou
  instala um plugin. Não há telemetria.
- **Editor:** voltar para a sessão e abrir uma sessão nova foram testados com o VSCodium.

## Desinstalar no Fedora

Antes, tire os hooks pela bandeja (**Claude Code hooks → Uninstall…**). Depois:

```sh
sudo dnf remove farol
```

## Dúvidas e problemas

Fale com o autor, João Schneider, pelo LinkedIn.

## Licença

MIT. O Farol nasceu a partir do [Coucou](https://github.com/Louis-CFM/coucou), de Louis Raillé. É um projeto
independente, não afiliado à Anthropic.

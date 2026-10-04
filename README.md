# Farol

Uma ilha na borda da tela do KDE Plasma que mostra as suas sessões do Claude Code. Cada sessão é um barco, e o mascote
Lumi (o farol) acende quando uma delas termina, trava ou espera por você. Dali mesmo dá para aprovar permissões,
responder perguntas e voltar para a sessão.

## Para quem serve

- Fedora 44 com KDE Plasma 6 no Wayland. Não funciona no GNOME, no Windows nem no macOS.
- Claude Code instalado.

## Baixar

| Arquivo | Versão | SHA-256 |
|---|---|---|
| [`farol-1.0-1.x86_64.rpm`](https://github.com/schneiderjaoo/farol-release/raw/main/farol-1.0-1.x86_64.rpm) | 1.0 | `d8fad90579ec719e0b8ac1c8c1792226311d495651cd5f833e1e87d51dd0392a` |

Ainda não há pacote `.deb`.

## Instalar

Baixe o arquivo e, na pasta onde ele ficou:

```sh
sudo dnf install ./farol-1.0-1.x86_64.rpm
```

Ou direto pelo link:

```sh
sudo dnf install https://github.com/schneiderjaoo/farol-release/raw/main/farol-1.0-1.x86_64.rpm
```

Para conferir o arquivo antes de instalar, compare o resultado com o SHA-256 da tabela:

```sh
sha256sum farol-1.0-1.x86_64.rpm
```

## Primeiro uso

1. Abra o Farol pelo menu de aplicativos. A ilha fica no topo da tela e o ícone na bandeja.
2. Clique com o botão direito no ícone da bandeja e escolha **Claude Code hooks → Install…**. O Farol mostra o que vai
   mudar em `~/.claude/settings.json`, faz um backup e só grava depois que você confirma.
3. Abra uma sessão nova do Claude Code. Ela aparece como um barco na ilha.

## O que há nesta versão

- **Ilha pequena:** o Lumi e um barco por sessão aberta, numa cena de noite no tema escuro e de dia no tema claro. A
  flâmula de cada barco mostra o estado da sessão, e uma etiqueta avisa quando alguma espera por você ou parou com erro.
- **Sessões:** a sessão em foco com os últimos passos e o uso do contexto, a lista de todas as sessões e o botão para
  voltar à janela do editor.
- **Permissões e perguntas:** respondidas pela ilha, sempre com um clique ou Enter seus.
- **Nova sessão:** lista os repositórios da sua pasta de trabalho e abre um deles no editor com uma sessão nova.
- **Perguntar:** solte arquivos (texto, código, imagem ou PDF) na ilha e pergunte sobre eles ao serviço que você
  escolher, entre Anthropic, ChatGPT e Gemini, com a sua chave de API.
- **Skills e plugins:** mantém as skills de uma pasta (por exemplo o repositório de skills da sua empresa) ligadas ao
  Claude Code e atualizadas pelo git, e gerencia os plugins do Claude Code.
- **Ajustes:** tema, posição na tela, atalhos de teclado globais, lembretes e notificações.

## Bom saber

- **Sons:** os arquivos de som não acompanham esta versão; o Farol funciona em silêncio.
- **Chaves de API:** ficam no cofre de senhas do sistema (KWallet ou GNOME Keyring), nunca num arquivo do Farol.
- **Rede:** o Farol só fala com a internet quando você faz uma pergunta na aba Perguntar, atualiza a pasta de skills ou
  instala um plugin. Não há telemetria.
- **Editor:** voltar para a sessão e abrir uma sessão nova foram testados com o VSCodium.

## Desinstalar

Antes, tire os hooks pela bandeja (**Claude Code hooks → Uninstall…**). Depois:

```sh
sudo dnf remove farol
```

## Dúvidas e problemas

Fale com o autor, João Schneider, pelo LinkedIn.

## Licença

MIT. O Farol nasceu a partir do [Coucou](https://github.com/Louis-CFM/coucou), de Louis Raillé. É um projeto
independente, não afiliado à Anthropic.

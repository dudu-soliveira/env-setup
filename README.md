# 🛠️ Guia de Configuração do Ambiente de Desenvolvimento

Este guia fornece instruções passo a passo para configurar um ambiente de desenvolvimento profissional e produtivo usando Linux/WSL, uv, VSCode e GitHub.

## 📑 Sumário

1. [Pré-requisitos](#-pré-requisitos)
2. [Configuração do Sistema](#-configuração-do-sistema)
   - [(Opcional) Terminal Aprimorado](#-opcional-terminal-aprimorado)
3. [Ambiente de Desenvolvimento](#-ambiente-de-desenvolvimento)
4. [Configuração do Editor](#-configuração-do-editor)
5. [Controle de Versão](#-controle-de-versão)
6. [Solução de Problemas](#-solução-de-problemas)
7. [Recursos Adicionais](#-recursos-adicionais)

## 📋 Pré-requisitos

- Windows 10/11 (caso use WSL) ou distribuição Linux
- Acesso de administrador ao sistema
- Conexão com a internet

## 💻 Configuração do Sistema

### Para Usuários Linux

Se você já está usando Linux, pode pular para a próxima seção.

### Para Usuários Windows (WSL)

1. **Instalação do WSL**

   ```powershell
   # Instala a última versão do WSL com Ubuntu como distribuição padrão
   wsl --install
   ```

2. **Configuração Inicial**
   - Reinicie o computador quando solicitado
   - Abra "Terminal" no menu Iniciar
   - Configure o Ubuntu como perfil padrão:
     - Abra configurações (ctrl+,)
     - Selecione Perfil padrão → Ubuntu (ícone laranja)
   - Configure seu usuário UNIX

### Ferramentas Essenciais

```bash
# Atualiza a lista de pacotes e atualiza pacotes existentes
sudo apt update && sudo apt upgrade -y

# Instala ferramentas essenciais de compilação
sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Instala o Git
sudo apt install -y git

# Instala o Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Adiciona o Homebrew ao PATH
SHELL_CONFIG="$HOME/.bashrc"
if [ -n "$ZSH_VERSION" ]; then
    SHELL_CONFIG="$HOME/.zshrc"
fi

eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" >> $SHELL_CONFIG
```

## 🐚 (Opcional) Terminal Aprimorado

### ZSH e Oh My Posh

1. **Instalação do ZSH**

   ```bash
   # Instala o shell ZSH
   sudo apt install -y zsh

   # Define ZSH como shell padrão
   chsh -s $(which zsh)
   ```

2. **Configuração da Fonte**

   - Baixe [JetBrainsMono Nerd Font](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/JetBrainsMono.zip)
   - Instale as fontes com prefixo "JetBrainsMonoNerdFontMono"
   - Extraia o arquivo
   - Configure o terminal:
     - Configurações → Ubuntu (ícone laranja) → Aparência → JetBrainsMono Nerd Font Mono

3. **Configuração do Ambiente**

   ```bash
   # Instala componentes necessários
   brew install jandedobbeleer/oh-my-posh/oh-my-posh
   brew install fzf
   brew install dos2unix

   # Configura Oh My Posh
   mkdir -p ~/.config/ohmyposh
   curl -o ~/.config/ohmyposh/zen.toml https://raw.githubusercontent.com/dudu-soliveira/env-setup/refs/heads/main/zen.toml
   curl -o ~/.zshrc https://raw.githubusercontent.com/dudu-soliveira/env-setup/refs/heads/main/.zshrc
   dos2unix ~/.config/ohmyposh/zen.toml
   dos2unix ~/.zshrc

   # Aplica configurações
   source ~/.zshrc
   ```

## 🐍 Ambiente de Desenvolvimento

### Configuração do Python com uv

```bash
# Instala uv
brew install uv

# Configura completação do shell
SHELL_CONFIG="$HOME/.bashrc"
if [ -n "$ZSH_VERSION" ]; then
    SHELL_CONFIG="$HOME/.zshrc"
fi

eval "$(uv generate-shell-completion zsh)" >> $SHELL_CONFIG

# Instala Python
uv python install 3.12.8
```

## 📝 Configuração do Editor

### VSCode

1. **Instalação**

   - Baixe e instale o [VSCode](https://code.visualstudio.com/)
   - Para WSL: instale a extensão "WSL"

2. **Extensões Recomendadas**

   _Desenvolvimento Python:_

   - Python (ms-python.python)
   - Pylance (ms-python.vscode-pylance)
   - Ruff (charliermarsh.ruff)
   - Python Environment Manager (donjayamanne.python-environment-manager)
   - Mypy Type Checker (ms-python.mypy-type-checker)

   _Suporte WSL:_

   - WSL (ms-vscode-remote.remote-wsl)

   _Controle de Versão:_

   - Git Graph (mhutchie.git-graph)
   - GitLens (eamodio.gitlens)

   _Produtividade:_

   - GitHub Copilot (GitHub.copilot) (se disponível)
   - Error Lens (usernamehw.errorlens)

3. **Configurações Recomendadas**

   ```json
   {
     "editor.formatOnSave": true,
     "python.languageServer": "Pylance",
     "[python]": {
       "editor.defaultFormatter": "charliermarsh.ruff",
       "editor.codeActionsOnSave": {
         "source.fixAll": "explicit",
         "source.organizeImports": "explicit"
       }
     },
     "notebook.formatOnSave.enabled": true,
     "notebook.codeActionsOnSave": {
       "notebook.source.fixAll": "explicit",
       "notebook.source.organizeImports": "explicit"
     }
   }
   ```

## 🔄 Controle de Versão

### Configuração do GitHub

1. **Configuração SSH**

   ```bash
   # Gera chave SSH
   ssh-keygen -t ed25519 -C "seu_email"

   # Configura agente SSH
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519

   # Exibe chave pública
   cat ~/.ssh/id_ed25519.pub
   ```

2. **Adicione a chave ao GitHub**

   - Acesse Configurações do GitHub → Chaves SSH e GPG
   - Clique em "Nova chave SSH"
   - Cole sua chave pública

3. **Configure o Git**

   ```bash
   git config --global user.name "Seu Nome"
   git config --global user.email "seu_email"
   ```

## 🔧 Solução de Problemas

### WSL

- **Falha na instalação:** Verifique se os recursos do Windows necessários estão ativos
- **Problemas de acesso:** Execute `wsl --shutdown` e reinicie o computador

### Python/uv

- **Falha na instalação:** Verifique as dependências do sistema
- **Problemas de PATH:** Confirme as configurações do shell

### VSCode

- **Detecção do Python:** Configure manualmente o caminho do interpretador
- **WSL:** Use "code ." dentro do WSL

## 📖 Recursos Adicionais

- [Documentação Python](https://docs.python.org/)
- [Documentação VSCode](https://code.visualstudio.com/docs)
- [Documentação GitHub](https://docs.github.com/)
- [Documentação WSL](https://docs.microsoft.com/en-us/windows/wsl/)
- [Documentação uv](https://github.com/astral-sh/uv)

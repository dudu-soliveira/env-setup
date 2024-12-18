# Guia de Configuração do Ambiente de Desenvolvimento

Este guia irá orientá-lo na configuração de um ambiente de desenvolvimento completo usando Linux/WSL, pyenv, uv, VSCode e GitHub.

## Índice

1. [Configuração do Sistema Operacional](#configuração-do-sistema-operacional)
2. [Instalação de Ferramentas Essenciais](#instalação-de-ferramentas-essenciais)
3. [Configuração do Ambiente Python](#configuração-do-ambiente-python)
4. [Configuração do VSCode](#configuração-do-vscode)
5. [Configuração do GitHub](#configuração-do-github)

## Configuração do Sistema Operacional

### Para Usuários Linux

Se você já está usando Linux, pode pular para a próxima seção.

### Para Usuários Windows (Configuração do WSL)

1. Abra o PowerShell como Administrador e execute:
   ```powershell
   wsl --install
   ```
2. Reinicie seu computador quando solicitado
3. Após reiniciar, abra "Terminal" no menu Iniciar
4. Defina Ubuntu como seu perfil padrão:
   - Abra a página de configurações (ctrl+,)
   - Clique em Perfil padrão
   - Selecione "Ubuntu" (perfil com ícone laranja)
5. Crie seu nome de usuário e senha UNIX quando solicitado

## Instalação de Ferramentas Essenciais

Execute os seguintes comandos no terminal:

```bash
# Atualiza a lista de pacotes e atualiza pacotes existentes
sudo apt update && sudo apt upgrade -y

# Instala ferramentas essenciais de compilação
sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev curl \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Instala o Git
sudo apt install -y git
```

## (Opcional) Instalação e Configuração do ZSH

O ZSH é um shell mais moderno e poderoso que o bash padrão, oferecendo diversos recursos úteis como autocompletar avançado, correção de comandos e temas personalizáveis.

### Instalando ZSH

```bash
# Instala o ZSH
sudo apt install -y zsh

# Define ZSH como shell padrão
chsh -s $(which zsh)
```

### Instalando a fonte JetBrainsMono

(Descreva)

#### Para Usuários Windows

1. [Faça o download de JetBrainsMono.zip](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/JetBrainsMono.zip)
2. Extraia o arquivo e entre na pasta criada
3. Pesquise por: "JetBrainsMonoNerdFontMono"
4. Instale todos os resultados
5. Abra uma nova janela do terminal e mude a fonte padrão do Ubuntu:
   - Abra a página de configurações
   - Clique em Ubuntu (perfil com ícone laranja)
   - Clique em Aparência
   - Clique em Tipo de fonte
   - Selecione "JetBrainsMono Nerd Font Mono"

### Baixando as dependências

```bash
curl -s https://ohmyposh.dev/install.sh | bash -s
sudo apt install fzf -y
```

```bash
mkdir ~/.config/ohmyposh
curl -o ~/.config/ohmyposh/zen.toml https://raw.githubusercontent.com/dudu-soliveira/env-setup/refs/heads/main/zen.toml
curl -o ~/.zshrc https://raw.githubusercontent.com/dudu-soliveira/env-setup/refs/heads/main/.zshrc
```

Após todas as alterações:

```bash
# Recarrega as configurações
source ~/.zshrc
```

## Configuração do Ambiente Python

### Instalando pyenv

```bash
# Instala pyenv
curl https://pyenv.run | bash

# Detecta qual shell está sendo usado
SHELL_CONFIG="$HOME/.bashrc"
if [ -n "$ZSH_VERSION" ]; then
    SHELL_CONFIG="$HOME/.zshrc"
fi

# Adiciona pyenv ao PATH
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> $SHELL_CONFIG
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> $SHELL_CONFIG
echo 'eval "$(pyenv init -)"' >> $SHELL_CONFIG

# Recarrega a configuração do shell
source $SHELL_CONFIG
```

### Instalando Python com pyenv

```bash
# Instala a versão mais recente do Python (ajuste a versão conforme necessário)
pyenv install 3.12.7

# Define a versão global do Python
pyenv global 3.12.7

# Verifica a instalação
python --version
```

### Configurando uv

```bash
# Instala uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Detecta qual shell está sendo usado
SHELL_CONFIG="$HOME/.bashrc"
if [ -n "$ZSH_VERSION" ]; then
    SHELL_CONFIG="$HOME/.zshrc"
fi

# Adiciona uv ao PATH
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> $SHELL_CONFIG
source $SHELL_CONFIG

# Verifica a instalação
uv --version
```

## Configuração do VSCode

1. Instale o VSCode em https://code.visualstudio.com/
2. Para usuários WSL, instale a extensão "WSL" no VSCode
3. Instale as extensões recomendadas:

   Essenciais para Python:

   - Python (ms-python.python): Suporte completo para desenvolvimento Python
   - Pylance (ms-python.vscode-pylance): Servidor de linguagem Python aprimorado
   - Ruff (charliermarsh.ruff): Linter/formatter Python extremamente rápido
   - Python Environment Manager (donjayamanne.python-environment-manager): Gerenciamento visual de ambientes Python
   - Mypy Type Checker (matangover.mypy): Verificação de tipos estáticos para Python

   Suporte para WSL:

   - WSL (ms-vscode-remote.remote-wsl): Desenvolvimento dentro do WSL

   Suporte para Git:

   - Git Graph: Visualização do histórico do Git
   - GitLens: Funcionalidades Git avançadas

   Outras Recomendadas:

   - GitHub Copilot (se disponível): IA para sugestão de código
   - Error Lens: Destacar erros e avisos inline

### Configurações do VSCode

Abra a Paleta de Comandos (Ctrl+Shift+P) e procure por "Preferences: Open Settings (JSON)". Adicione estas configurações:

```json
{
  "editor.formatOnSave": true,
  "editor.rulers": [88],
  "editor.renderWhitespace": "all",
  "editor.suggestSelection": "first",
  "files.trimTrailingWhitespace": true,
  "python.defaultInterpreterPath": "${env:HOME}/.pyenv/versions/3.12.7/bin/python",
  "python.languageServer": "Pylance",
  "python.analysis.typeCheckingMode": "basic",
  "python.linting.enabled": true,
  "python.linting.mypyEnabled": true,
  "python.formatting.provider": "none",
  "ruff.enable": true,
  "ruff.organizeImports": true,
  "ruff.fixAll": true,
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": true,
      "source.organizeImports.ruff": true
    }
  }
}
```

## Configuração do GitHub

1. Gere uma chave SSH:

   ```bash
   ssh-keygen -t ed25519 -C "seu_email"
   ```

2. Inicie o agente SSH e adicione a chave:

   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. Copie a chave pública:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

4. Adicione a chave SSH ao GitHub:

   - Vá para Configurações do GitHub → Chaves SSH e GPG
   - Clique em "Nova chave SSH"
   - Cole sua chave pública
   - Salve

5. Configure o Git:
   ```bash
   git config --global user.name "Seu Nome"
   git config --global user.email "seu_email"
   ```

## Problemas Comuns e Solução de Problemas

### WSL

- Se a instalação do WSL falhar, certifique-se de que o Subsistema do Windows para Linux e a Plataforma de Máquina Virtual estão habilitados nos Recursos do Windows
- Se você não conseguir acessar o WSL, tente executar `wsl --shutdown` e reinicie seu computador

### Python/pyenv

- Se a instalação do Python falhar, certifique-se de que todas as dependências necessárias estão instaladas
- Se os comandos pyenv não forem reconhecidos, verifique se o PATH está configurado corretamente em ~/.bashrc

### VSCode

- Se a extensão Python não detectar seu interpretador Python, defina manualmente o caminho do interpretador
- Para usuários WSL, certifique-se de abrir o VSCode com "code ." dentro do WSL

## Boas Práticas

1. Sempre crie ambientes virtuais para novos projetos:

   ```bash
   # Navegue até o diretório do projeto
   cd seu-projeto

   # Crie e ative o ambiente virtual
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Use .gitignore para projetos Python:

   ```bash
   # Crie .gitignore
   curl https://raw.githubusercontent.com/github/gitignore/main/Python.gitignore -o .gitignore
   ```

3. Documente dependências:
   ```bash
   # Crie requirements.txt
   uv pip freeze > requirements.txt
   ```

## Recursos Adicionais

- [Documentação Oficial do Python](https://docs.python.org/)
- [Documentação do VSCode](https://code.visualstudio.com/docs)
- [Documentação do GitHub](https://docs.github.com/)
- [Documentação do WSL](https://docs.microsoft.com/en-us/windows/wsl/)
- [Documentação do pyenv](https://github.com/pyenv/pyenv)
- [Documentação do uv](https://github.com/astral-sh/uv)

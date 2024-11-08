
# Guia Rápido: Configurando Git Credential Manager (GCM) e SSH para Múltiplas Contas do GitHub

Este repositório contém um guia rápido para configurar o Git com o Git Credential Manager (GCM) e SSH, permitindo gerenciar múltiplas contas do GitHub de forma eficiente.

## Índice
- [Passo 1: Configurar Chaves SSH Separadas](#passo-1-configurar-chaves-ssh-separadas)
- [Passo 2: Clonar Repositórios com Aliases](#passo-2-clonar-repositórios-com-aliases)
- [Passo 3: Configurar o Git Credential Manager (GCM)](#passo-3-configurar-o-git-credential-manager-gcm)
- [Passo 4: Verificar URLs Remotas](#passo-4-verificar-urls-remotas)

---

## Passo 1: Configurar Chaves SSH Separadas

1. **Gerar Chaves SSH para Cada Conta**:
   ```bash
   # Para conta pessoal
   ssh-keygen -t rsa -b 4096 -C "email_pessoal@example.com" -f ~/.ssh/id_rsa_pessoal

   # Para conta de trabalho
   ssh-keygen -t rsa -b 4096 -C "email_trabalho@example.com" -f ~/.ssh/id_rsa_trabalho
   ```

2. **Adicionar as Chaves SSH ao Agente SSH**:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_rsa_pessoal
   ssh-add ~/.ssh/id_rsa_trabalho
   ```

3. **Configurar o Arquivo de Configuração SSH**:
   - Abra o arquivo de configuração SSH:
     ```bash
     nano ~/.ssh/config
     ```
   - Adicione a configuração a seguir (substitua por aliases adequados como `github-pessoal` e `github-trabalho`):
     ```plaintext
     # Conta Pessoal
     Host github-pessoal
         HostName github.com
         User git
         IdentityFile ~/.ssh/id_rsa_pessoal

     # Conta de Trabalho
     Host github-trabalho
         HostName github.com
         User git
         IdentityFile ~/.ssh/id_rsa_trabalho
     ```

---

## Passo 2: Clonar Repositórios com Aliases

- Para repositórios pessoais:
  ```bash
  git clone git@github-pessoal:usuario/repositorio-pessoal.git
  ```

- Para repositórios de trabalho:
  ```bash
  git clone git@github-trabalho:usuario/repositorio-trabalho.git
  ```

---

## Passo 3: Configurar o Git Credential Manager (GCM)

1. **Definir o GCM como Gerenciador de Credenciais**:
   ```bash
   git config --global credential.helper manager-core
   ```

2. **Definir `user.name` e `user.email` para Cada Repositório**:
   Dentro de cada repositório, configure o `user.name` e `user.email` associados à conta correta:
   ```bash
   # Para repositório pessoal
   git config user.name "Nome Pessoal"
   git config user.email "email_pessoal@example.com"

   # Para repositório de trabalho
   git config user.name "Nome de Trabalho"
   git config user.email "email_trabalho@example.com"
   ```

---

## Passo 4: Verificar URLs Remotas

Confira a configuração da URL remota para garantir que o alias correto está sendo usado:

```bash
git remote -v
```

Se necessário, atualize a URL remota para usar o alias apropriado:
```bash
# Para repositório pessoal
git remote set-url origin git@github-pessoal:usuario/repositorio-pessoal.git

# Para repositório de trabalho
git remote set-url origin git@github-trabalho:usuario/repositorio-trabalho.git
```

---

## Conclusão

Com esta configuração, você pode gerenciar múltiplas contas do GitHub de maneira eficaz utilizando SSH e o Git Credential Manager.

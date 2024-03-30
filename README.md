# Adicionar uma nova chave SSH à sua conta do GitHub

## Verificar se há chaves SSH

### Antes de gerar uma chave SSH, você pode verificar se há outras chaves SSH

>[!NOTE]
>Lista os arquivos dentro do seu diretório .ssh, se existirem.
>```bash
> ls -al ~/.ssh
>```
>Por padrão, os nomes de arquivos de chaves públicas com suporte para o GitHub são um dos seguintes:
>- ***id_rsa.pub***
>- ***id_ecdsa.pub***
>- ***id_ed25519.pub***
>  
>Se você receber um erro indicando que _~/.ssh_ não existe, você não terá uma par de chaves SSH existente no local padrão.

## Gerando uma nova chave SSH e adicionando-a ao agente SSH

### Depois de verificar a existência de chaves SSH, é possível gerar uma nova chave SSH para autenticação e adicioná-la ao ssh-agent

**Gerar uma nova chave SSH**

>[!NOTE]
>Cole o texto abaixo, substituindo o email usado no exemplo pelo seu endereço de email GitHub
>```bash
>  ssh-keygen -t rsa -b 4096 -C "pedroribeiro212@gmail.com"
>```
>### Você pode proteger usas chaves SSH e configurar um agente de autenticação para não precise redigitar a senha toda vez que usar as chaves SSH
>Com as chaves SSH, se alguém conseguir acessar seu computador, o invasor terá acesso a todos os sistemas que usam essas chaves.
>Para incluir uma camada extra de segurança, adicione uma frase secreta à sua chave SSH.
>Para evitar inserir a frase secreta toda vez que você se conectar, você poderá salvar sua frase secreta com segurança no agente SSH.
>
>### Inicialização automática do ssh-agent
>Você pode executar ssh-agent automaticamente quando abre o shell do Bash ou do Git.
>Copie as seguintes linhas e cole-as no arquivo ~/.profile ou ~/.bashrc no shell do Git:
>```
>env=~/.ssh/agent.env
>
>agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }
>
>agent_start () {
>    (umask 077; ssh-agent >| "$env")
>    . "$env" >| /dev/null ; }
>
>agent_load_env
>
># agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
>agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)
>
>if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
>    agent_start
>    ssh-add
>elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
>    ssh-add
>fi
>
>unset env
>```
>
>### Recarregue o bashrc
>```
>source ~/.bashrc
>```

## Adicionar uma nova chave SSH à sua conta do GitHub

### Para configurar sua conta em GitHub.com para usar sua chave SSH nova (ou existente), você também precisará adicionar a chave à conta

Depois de gerar um par de chaves de SSH, você pode adicionar a chave pública em GitHub.com para permitir que o SSH acessua sua conta

>[!NOTE]
>1. Copie a chave pública SSH para usa área de transferência
>     ```
>     cat ~/.ssh/id_rsa.pub
>     ```
>2. No canto superior direito de qualquer página, clique na foto do seu perfil e em **Settings**
>3. Na seção "Acesso" da barra lateral, clique em **Chaves SSH e GPG**
>4. Clique em **Nova chave SSH* ou **Adicionar chave SSH**
>5. No campo "Title", adicione uma etiqueta descritiva para a nova chave. Por exemplo, se estiver usando um laptop pessoal, você poderá chamar essa chave de "Laptop pessoal"
>6. Selecione o tipo de chave: autenticação ou assinatura
>7. No campo "Chave", cole sua chave pública
>8. Clique em **Adicionar chave SSH**

# Adicionar uma nova chave GPG à sua conta GitHub

>[!NOTE]
>1. Verificar se existe uma chave GPG
>   ```
>   gpg --list-secret-key --keyid-form LONG
>   ```
>2.  Gerar chave GPG
>    ```
>    gpg --full-generate-key
>    ```
>    - Please select what kind of key you want: (1) RSA and RSA (default)
>    - What keysize do you want? (3072) 4096
>    - key is valid for? (0) 1y
>    - Is this correct? (y/N) y
>    - Change (N)ame, (C)omment, (E)mail, (O)kay/(Q)uit? o
>3.  
>      ```
>      gpg --list-secret-key --keyid-form LONG
>      ```
>      - sec rsa4096/***A40B520717B714C3*** (**ID**)
>4.
>      ```
>      gpg --armor --export <ID_DA_SUA_CHAVE_GPG>
>      ```
>5.  Copia a chave gerada
>    ```
> 
>    -----BEGIN PGP PUBLIC KEY BLOCK-----
>
>    -----END PGP PUBLIC KEY BLOCK-----
>
>    ```
>6.  Edite .bashrc
>    ```
>    vim ~/.bashrc
>    ```
>7.  Adicione a linha abaixo no fim do arquivo .bashrc
>    ```
>    export GPG_TTY=$(tty)
>    ```
>8.  Recarregue o ~/.bashrc
>    ```
>  	 source ~/.bashrc
>    ```
>9.    
>    ```
>    git config --global commit.gpgSign true
>    ```
>10.   
>     ```
>     git config --global tag.gpgSign true
>     ```
>11.  Quando commitar pela primeira vez, será solicitado a frase secreta configurada na criação da chave GPG
>12.   
>     ```
>     git log --show-signature -1
>     ```
>13.   
>      ```
>      vim ~/.gnupg/gpg.conf
>      ```
>14. Adicionar a linha abaixo no arquivo
>    ```
>    use-agent
>    ```
>15.   
>      ```
>      gpgconf --launch gpg-agent
>      ```
>16.  Copie a chave GPG (PASSO 3 e 4) para usa área de transferência
>17.  No canto superior direito de qualquer página, clique na foto do seu perfil e em **Settings**
>18.  Na seção "Acesso" da barra lateral, clique em **Chaves SSH e GPG**
>19.  Clique em **New GPG KEY**
>20.  No campo "Title", adicione uma etiqueta descritiva para a nova chave. Por exemplo, se estiver usando um laptop pessoal, você poderá chamar essa GPG de "Laptop pessoal"
>21.  No campo "Key", cole sua chave GPG
>22.  Clique em **Add GPG Key**     

### Referências

[Verificar se há chaves SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

[Gerando uma nova chave SSH e adicionando-a ao agente SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)

[Trabalhe com frase secreta da chave SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)

[Adicionar uma nova chave SSH à sua conta do GitHub](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=linux)

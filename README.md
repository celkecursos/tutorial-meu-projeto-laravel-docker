## Como usar Docker na VPS da Hostinger para fazer o deploy do Laravel 12


## Requisitos

* Docker - Conferir a instalação no PowerShell: docker --version
* Composer - Conferir a instalação: composer --version
* PHP 8.2 ou superior - Conferir a versão: php -v
* Node.js 22 ou superior - Conferir a versão: node -v
* Git - Conferir a instalação: git --version

## Criar o projeto com Laravel no PC e criar o container no Docker

- Acessar o prompt de comando ou o terminal do editor VSCode.

Criar o projeto com Laravel sem instalar o mesmo de forma global no sistema operacional.
```
composer create-project laravel/laravel tutorial-meu-projeto-laravel-docker
```

Acessar o diretório do projeto.
```
cd tutorial-meu-projeto-laravel-docker
```

Instalar o Sail no projeto existente.
```
composer require laravel/sail --dev
```

- Acessar WSL.

Verificar se o PHP 8.4 está ativo.
```
php -v
```

Acessar o diretório que será criado o projeto "c:/Celke/tutorial-meu-projeto-laravel-docker". /mnt/c → é onde o WSL monta o disco C: do Windows. /mnt/c/celke → equivale a C:\celke.
```
cd /mnt/c/Celke/tutorial-meu-projeto-laravel-docker
```

Publicar o docker-compose.yml e alterar no arquivo .env as variáveis ​​de ambiente necessárias para se conectar aos serviços do Docker.
```
php artisan sail:install
```

Criar os containers com Laravel e MySQL.
```
./vendor/bin/sail up -d
```

Rodar migrate para criar a base de dados e as tabelas.
```
./vendor/bin/sail artisan migrate
```

Rodar as seedeers para cadastrar registro de teste.
```
./vendor/bin/sail artisan db:seed
```

Acessar a aplicação no navegador.
```
http://127.0.0.1
```

## Como enviar o projeto para o GitHub.

Inicializar um novo repositorio GIT.
```
git init
```

Adicionar todos os arquivos modificados na área de preparação.
```
git add .
```

Verificar em qual branch está.
```
git branch
```

Renomear a branch atual no GIT para main.
```
git branch -M main
```

Commit registra as alterações feitas nos arquivos que foram adicionados na área de preparação.
```
git commit -m "Base do projeto"
```

Adicionar um repositório remoto ao repositório local.
```
git remote add origin https://github.com/celkecursos/tutorial-meu-projeto-laravel-docker.git
```

Enviar os commits locais para um repositório remoto.
```
git push -u origin main
```
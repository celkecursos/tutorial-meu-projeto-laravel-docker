## Como usar Docker na VPS da Hostinger para fazer o deploy do Laravel 12

- [Ganhe 20% de desconto adicional na Hostinger](https://www.hostinger.com.br/referral?REFERRALCODE=1CESARNICOL13)

- Cupom para ganhar 10% de desconto na Hostinger — não cumulativo com o link acima: celke

Senha usada na aula, não utilizar a mesma: 58F7s8#9f65x

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

## Conectar o PC ao servidor com SSH

Criar chave SSH (chave pública e privada).
```
ssh-keygen -t rsa -b 4096 -C "seu-email@exemplo.com"
```
```
ssh-keygen -t rsa -b 4096 -C "cesar@celke.com.br"
```

Local que é criado a chave pública.
```
C:\Users\SeuUsuario\.ssh\
```
```
C:\Users\cesar/.ssh/
```

Exibir o conteúdo da chave pública.
```
cat ~/.ssh/id_rsa.pub
```

Acessar o servidor com SSH.
```
ssh usuario-do-servidor@ip-do-servidor-vps
```
```
ssh root@93.127.210.72
```

## Conectar Servidor ao GitHub

Gerar a chave SSH no servidor.
```
ssh-keygen -t rsa -b 4096 -C "cesar@celke.com.br"
```

Imprimir a chave pública gerada.
```
cat ~/.ssh/id_rsa.pub
```

- No GitHub, vá para Settings (Configurações) do seu repositório ou da sua conta, em seguida, vá para SSH and GPG keys e clique em New SSH key. Cole a chave pública no campo fornecido e salve.

Verificar a conexão com o GitHub.
```
ssh -T git@github.com
```

- Se gerar o erro "The authenticity of host 'github.com (xx.xxx.xx.xxx)' can't be established.". Isso é uma medida de segurança para evitar ataques de "man-in-the-middle". Necessário adicionar a chave do host do GitHub ao arquivo de known_hosts do seu servidor.

Digite yes quando for solicitado.
```
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Verificar a conexão novamente.
```
ssh -T git@github.com
```

- Mensagem de conexão realizada com sucesso. Hi nome-usuario! You've successfully authenticated, but GitHub does not provide shell access.

## Enviar os arquivos do Github para a VPS da Hostinger

Baixar os arquivos do GitHub para a VPS.
```
git clone <ssh_repository_url>
```

Acessar o diretório do projeto.
```
cd tutorial-meu-projeto-laravel-docker
```

Duplicar o arquivo ".env.example" e renomear para ".env".
```
cp .env.example .env
```

Abrir o arquivo ".env" e alterar as variaveis de ambiente.
```
nano .env
```

Ctrl + O e enter para salvar.<br>
Ctrl + X para sair.<br>

Alterar o valor das variaveis de ambiente.
```
APP_NAME=Celke
APP_ENV=production
APP_KEY=
APP_DEBUG=false
APP_TIMEZONE=America/Sao_Paulo
APP_URL=https://srv566492.hstgr.cloud 
```

Alterar as variaveis de conexão com banco de dados.
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=sail
DB_PASSWORD=password
```

- Executar um container temporário e remover automaticamente após o uso (--rm)
- Montar o diretório atual (pwd) do host como /app dentro do container
- Definir o diretório de trabalho dentro do container como /app (onde está o projeto)
- Usar a imagem Docker oficial do Laravel Sail com PHP 8.4 e Composer
- Executar o comando "composer install" dentro do container para instalar as dependências
```
docker run --rm \
  -v $(pwd):/app \
  -w /app \
  laravelsail/php84-composer:latest \
  composer install
```

Criar e iniciar os containers definidos no docker-compose.yml.
Rodar MySQL, PHP-FPM, etc.
Deixar o ambiente pronto para rodar comandos Artisan dentro do Sail.
```
./vendor/bin/sail up -d
```

Listar os containers.
```
docker ps
```

Acessar o bash do container.
```
docker exec -it tutorial-meu-projeto-laravel-docker-laravel.test-1 bash
```

Alterar o proprietário dos arquivos.
```
chown -R sail:sail /var/www/html/storage /var/www/html/bootstrap/cache /var/www/html/.env
```

Alterar a permissão.
```
chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache
```

Gerar a chave do app.
```
php artisan key:generate
```

Sair do bash do container.
```
exit
```

Verificar se os containers estão rodando.
```
docker ps
```

Se não estiverem rodando, necessário subir novamente com:
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

Se já estiver tudo rodando, o Laravel deve estar acessível via o navegador pelo IP público da VPS.
```
http://SEU_IP
```
```
http://93.127.210.72
```

# Configurar o DNS e instalar o SSL

Alterar a porta no docker-compose.yml.
```
ports:
  - '${APP_PORT:-8080}:80'
```

Expor a porta 8080.
```
export APP_PORT=8080
```

Parar e reiniciar os containers.
```
./vendor/bin/sail down
./vendor/bin/sail up -d
```

Listar os containers.
```
docker ps
```

Instalar o Nginx na VPS (fora do Docker).
```
sudo apt update
sudo apt install nginx -y
```

Recarregar o Nginx.
```
sudo systemctl reload nginx
```

Testar a configuração.
```
sudo nginx -t
```

Se estiver tudo OK deve retornar.
```
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Iniciar o Nginx se estiver pausado.
```
sudo systemctl start nginx
```

Criar o arquivo com as configuração do domínio no Nginx.
```
sudo nano /etc/nginx/sites-available/seusite.com.br
```
```
sudo nano /etc/nginx/sites-available/celkeprime.com.br
```

Alterar as configurações no arquivo criado.
```
server {
    listen 80;
    server_name celkeprime.com.br www.celkeprime.com.br;

    # Aumenta o limite do tamanho do cookie/header
    large_client_header_buffers 4 16k;

    location / {
        proxy_pass http://localhost:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Ativar as configuração do site.
```
sudo ln -s /etc/nginx/sites-available/seusite.com.br /etc/nginx/sites-enabled/
```
```
sudo ln -s /etc/nginx/sites-available/celkeprime.com.br /etc/nginx/sites-enabled/
```

Listar os sites.
```
ls -l /etc/nginx/sites-enabled/
```

Remover o site diferente do domínio.
```
sudo rm /etc/nginx/sites-enabled/dominiositediferente.com.br
```
```
sudo rm /etc/nginx/sites-enabled/default
```

Recarregar o Nginx.
```
sudo systemctl reload nginx
```

Testar a configuração.
```
sudo nginx -t
```

Se estiver tudo OK deve retornar.
```
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Instalar o Certbot e plugin para Nginx.
```
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

Rodar o Certbot para gerar o certificado SSL.
```
sudo certbot --nginx -d celkeprime.com.br -d www.celkeprime.com.br
```

- Digitar durante a instalação do Certbot: seu_email@seu_dominio.com.br

Acessar o domínio configurado.

No tutorial o arquivo "/etc/nginx/sites-available/celkeprime.com.br" possui a configuração.
```
server {
    listen 80;
    server_name celkeprime.com.br www.celkeprime.com.br;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name celkeprime.com.br www.celkeprime.com.br;

    ssl_certificate /etc/letsencrypt/live/celkeprime.com.br/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/celkeprime.com.br/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://127.0.0.1:8080;  # redireciona para o Laravel rodando no Docker
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Autor

Este projeto foi desenvolvido por [Cesar Szpak](https://github.com/cesarszpak) e está hospedado no repositório da organização [Celke](https://github.com/celkecursos).

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE](LICENSE.txt) para mais detalhes.
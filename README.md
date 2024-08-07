# Web Server Php
Configura o apache pra rodar scripts PHP

## Etapa necessária:
<a href="https://github.com/Olliv3r/Web-Server">Configurar apache pra rodar com certificado</a>

### Etapa 1:
![mpm](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php_main.jpg)

#### Instalar dependencias
```
apt update && apt install nano apache2 php php-apache phpmyadmin mariadb -y
```

### Etapa 2
#### Alterar o arquivo  httpd.conf
```
nano $PREFIX/etc/apache2/httpd.conf
```

Com o arquivo aberto, digite `ctr+w` e pesquise essa linha e descomente ela:
> LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so

Ficando assim:
![Carregar biblioteca mod_mpm_prefork.iso](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-mpm_prefork_module.jpg)


Digite `ctr+w` e pesquise essa linha e comente ela:
> LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so

Ficando assim:
![Desativar biblioteca mod_mpm_worker.so](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-mpm-worker-module.jpg)

Digite `ctr+w` e pesquise essa linha:
> LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so

Acima dessa linha, adicione uma nova linha com este comando:
```
LoadModule php_module libexec/apache2/libphp.so
```
Ficando assim:
![Carregar biblioteca libphp.so](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-php_module.jpg)

Digite `ctr+w` e pesquise essa linha:
> &lt;IfModule ssl_module>

Abaixo do bloco nessa linha, adicione uma nova linha e cole esse bloco de comandos: 
```
<FilesMatch \.php?>
    SetHandler application/x-httpd-php
</FilesMatch>
```

Ficando assim:
![Definir o manipulador](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-ifmodule.jpg)

Digite `ctr+w` e pesquise essa linha:
> Include etc/apache2/conf.d/*.conf

Abaixo dessa linha adicione uma nova linha e cole esse comando:
```
Include etc/apache2/extra/php_module.conf
```

Ficando assim:
![Incluir modulo php_module.conf](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-php_module.conf.jpg)

Salve este arquivo digitando `ctr+x+y` e enter

Crie um arquivo chamado `php_module.conf`
```
touch $PREFIX/etc/apache2/extra/php_module.conf
```

Agora navegue até a pasta `/sdcard/htdocs` onde o apache usa pra executar os projetos:
```
cd /sdcard/htdocs
```

Crie o arquivo `index.php`:
```
nano /sdcard/hdocs/index.php
```

Adicione qualquer comando PHP neste arquivo, vou adicionar apenas `phpinfo();` para testar
```
<?php
phpinfo();
?>
```
Ficando assim:
![phpinfo();](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-phpinfo().jpg)

Salve o arquivo digitando `ctr+x+y` e enter

Reinicie o apache com `apachectl -k restart` ou `apachectl -k start` caso estiver parado, abra o link `http://localhost:8080` e clica no index.php que aparecerá no navegador

#### Carregar o index.php por padrão
![Index.html](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-index.html.jpg)

Pra fazer com que o php carregue o index.php por padrão, segue esses passos

Abra o arquivo `httpd.conf ` novamente:
```
nano $PREFIX/etc/apache2/httpd.conf
```

 E pesquise essa linha:
> DirectoryIndex index.html

Nessa linha substitua a extensão `.html` por `.php` ficando assim:
![Carregar index.php por padrão](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/php-index.php.jpg)

Terminamos com esse arquivo, salve digitando `ctr+x+y` e enter

Reinicie o apache
```
apachectl -k restart
```

E acesse
```
termux-open-url http://localhost:8080
```

#### Configurar o arquivo *htaccess* no apache (Opcional)

Abra o arquivo `httpd.conf` do apache:
```
nano $PREFIX/etc/apache2/httpd.conf
```
Pressione `ctrl+w` e pesquise esta linha `LoadModule rewrite_module libexec/apache2/mod_rewrite.so` e a descomente, ficando assim:
![mpm](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/mod_rewrite.jpg)

Agora pressione `ctrl+w` e pesquise por esta linha `AllowOverride None` e altere de `None` para `All`, ficando assim:
![mpm](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/allowOverRide.jpg)

Salve e com `ctrl+x+y`, neste arquivo ja finalizamos.

Agora vamos criar o arquivo de configuração `htaccess` no diretório dos projetos em `htdocs`, cole este bloco de códigos em um arquivo chamado `.htaccess` dentro do diretório dos projetos (Aviso: o arquivo precisa ser oculto com o ponto '.' na frente do nome):

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-l
RewriteRule ^(.+)$ index.php?url=$1 [QSA,L]

Options -Indexes

<Files .env>
    Order allow,deny
    Deny from all
</Files>
```

Ficando assim:
![mpm](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/htaccess.jpg)

Pronto, agora é so testar na url algumas rotas aleatórias e ver que não haverá um erro que não encontrou a página:
![mpm](https://github.com/Olliv3r/Web-Server-Php/blob/main/media/route_testing.jpg)

### Próxima etapa (opcional)
<a href="https://github.com/Olliv3r/Web-Server-Mysql">Configurar o phpmyadmin pra rodar no apache</a>

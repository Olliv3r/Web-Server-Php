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

Agora navegue até a pasta `htdocs` do apache
```
cd $PREFIX/share/apache2/default-site/htdocs
```

Renomeie o `index.html` para `index.php` e abra ele
```
mv index.html index.php
nano index.php
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

Inicie o apache2 com `apachectl -k start`, abra o link `http://localhost:8080` e clica no index.php que aparecerá no navegador

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

### Configurar o phpmyadmin pra rodar no apache
<a href="https://github.com/Olliv3r/Web-Server-Mysql">Configurar o phpmyadmin</a>

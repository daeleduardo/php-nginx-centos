# O que é?

Criação de um Dockerfile usando php-fpm, nginx e Centos 8.

## Como usar?
Basta copiar o contéudo da pasta `server` para o seu projeto e trocar os arquivos da pasta `hosts` para os arquivos de configurações de hosts do seu servidor NGINX.

A cópia dos arquivos de configuração para dentro do container, são feitas automaticamente pelo `docker-compose.yml`.


## Como testar?

É necessário adicionar no seu arquivo de hosts os seguintes endereços:
```
127.0.0.1       app1.local
127.0.0.1       app2.local
```

No docker-compose existem três serviços:
*   app  (disponibiliza o app1 e app2)
*   app1 (disponibiliza somente o app1)
*   app2 (disponibiliza somente o app2)

Para iniciar qualquer um dos três serviços, basta executar:
```docker-compose up -d [nome_do_servico]```
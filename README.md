# REST API Processo Seletivo SEPLAG/2025
(Analista de TI - Perfil Junior, Pleno e Sênior)

Número da inscrição: 8534

CPF: 916.496.921-53

Perfil: DESENVOLVEDOR PHP - PLENO


# Projeto API REST em PHP e framework Lavavel com Docker Compose e registros em base de dados postgreSQL.
Este repositório contém um projeto com uma solução que será utilizado exclusivamente para uma avaliação de processo seletivo da SEPLAG.

Nº Inscrição: 8066 - Perfil: DESENVOLVEDOR PHP - SÊNIOR


#### 🛠 Tecnologias

As seguintes ferramentas foram usadas na construção do projeto:

- PHP
- Laravel
- PostgreSQL
- MinIO
  
#### Pré-requisitos


#### Faça o Clone do Projeto, o mesmo encontra-se na branch master, para clonar a branch específica:

```bash
git clone --branch master https://github.com/getheo/api-seletivo-seplag.git
```


#### 🐳 VERIFICANDO O DOCKER

Verifica se o Docker Compose está instalado
```bash
docker --version
```

Verifica se já existe Containers instalados
```bash
docker ps -a
```

#### 🏗️ CONFIGURANDO O AMBIENTE
Os arquivos (Dockerfile e docker-compose.yml) estão configurados para instanciar e criar os containers (api-seletivo-seplag, db, minio_server) e suas respectivas imagens (api-seletivo-seplag, postgres, minio/minio). Desta forma, basta acessar a raiz do projeto pelo terminal e executar o comando:
```bash
docker compose up -d
```

Aguarde a instalação e configurações dos contaniers, após instalado, confirme a instalação executando novamente o comando:
```bash
docker ps -a
```

#### 🗄️ CONFIGURANDO O BANCO DE DADOS NO CONTAINER
Após a confirmação dos containers instalados com suas respectivas imagens, para garantir que tudo esteja funcionando, execute as migrations dentro do contaniner (api-seletivo-seplag)
```bash
docker exec api-seletivo-seplag php artisan migrate:fresh
```

Execute o comando abaixo para inserir alguns dados para os teste.
```bash
docker exec api-seletivo-seplag php artisan db:seed
```

#### 📚 GERANDO DOCUMENTAÇÃO
Execute o comando abaixo para criar a documentação Swagger, onde será possível testar todos os endpoints.
```bash
docker exec api-seletivo-seplag php artisan l5-swagger:generate
```

#### 🌐 INICIANDO O SERVIDOR WEB NO CONTAINER
Execute o comando abaixo para instanciar o servidor web no container (api-seletivo-seplag)
```
docker exec api-seletivo-seplag php artisan serve
```
#### 🧪 TESTANDO A API
Para realizar os teste, basta acessar pelo navegador:
```bash
http://localhost:8000/api/documentation
```

#### Pelo Swagger, 1º é necessário realizar o login na Autenticação. (já está pré-cadastrado)
- 📧 **Email:** `teste@seplag.mt.gov.br`
- 🔑 **Senha:** `seplag2025`

- Execute e será gerado o TOKEN. Copie e cole na variável "authorize" (canto superior direito).
- Após esta ação é possível realizar os testes. Tempo do token expira em 5 minutos.
- Para renovar o TOKEN


#### Para verificar os arquivos publicados no MinIO, acesse:
```bash
http://localhost:9090/login
```

Username: minio
Senha: miniostorage

#### Caso precise excluir tudo para refazer o processo, execute os comandos (prune = informações de cache:):
```bash
docker compose down
```
```bash
docker container prune -f
```
```bash
docker system prune
```

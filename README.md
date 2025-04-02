# Projeto prático para o Processo Seletivo SEPLAG/2025



Candidato: Guilherme Théo Coleta Arruda<br>
Número da inscrição: 8534<br>
CPF: 916.496.921-53<br>
Perfil: DESENVOLVEDOR PHP - PLENO



## Projeto API REST em PHP e framework Laravel com Docker Compose e registros em base de dados postgreSQL.
Este repositório contém um projeto com uma solução que será utilizado exclusivamente para uma avaliação de processo seletivo da SEPLAG.



### 🛠 Tecnologias



As seguintes ferramentas foram usadas na construção do projeto:
- PHP 8+
- Laravel 11+
- PostgreSQL
- MinIO (armazenamento das fotos)
- Docker e Docker Compose



### 🛠 Pré-requisitos
- Docker Desktop instalado


### Faça o Clone do Projeto
O projeto encontra-se na branch master, para clonar a branch específica:
```bash
git clone --branch master https://github.com/getheo/api-seletivo-seplag.git
```

---
### Acesse a raiz do projeto onde realizou o clone do projeto
Renomeie o arquivo (.env-exemplo) para (.env)
---


### Instale as dependências do PHP Laravel
```bash
composer install
```


### 🐳 Verificando o Docker

Verifica se o Docker Compose está instalado
```bash
docker --version
```

Verifica se já existe Containers instalados
```bash
docker ps -a
```

### 🏗️ Configurando o ambiente
#### Os arquivos (Dockerfile e docker-compose.yml) estão configurados para instanciar e subir os containers:
- api-seletivo-seplag
- db
- minio_server

#### Suas respectivas imagens 
- api-seletivo-seplag
- postgres
- minio/minio



Desta forma, basta acessar a raiz do projeto pelo terminal e executar o comando:
```bash
docker compose up -d --build
```

Aguarde a instalação e configurações dos contaniers, após instalado, confirme a instalação executando novamente o comando:
```bash
docker ps -a
```

### 🗄️ Configurando o bando de dados no Container
Após a confirmação dos containers instalados com suas respectivas imagens, para garantir que tudo esteja funcionando, execute as migrations dentro do contaniner (api-seletivo-seplag)
```bash
docker exec api-seletivo-seplag php artisan migrate:fresh
```

Execute o comando abaixo para inserir alguns dados para os teste.
```bash
docker exec api-seletivo-seplag php artisan db:seed
```

### 📚 Gerando a Documentação
Execute o comando abaixo para criar a documentação Swagger, onde será possível testar todos os endpoints.
```bash
docker exec api-seletivo-seplag php artisan l5-swagger:generate
```

### 🌐 Iniciando o Servidor Web no Container
Execute o comando abaixo para instanciar o servidor web no container (api-seletivo-seplag)
```
docker exec api-seletivo-seplag php artisan serve
```
### 🧪 Testando a API
Para realizar os teste, basta acessar pelo navegador:
```bash
http://localhost:8000/api/documentation
```

### Pelo Swagger, 1º é necessário realizar o login na Autenticação. (pré-cadastrado)
- 📧 **Email:** `teste@seplag.mt.gov.br`
- 🔑 **Senha:** `seplag2025`

- Execute e será gerado o TOKEN. Copie e cole na variável "authorize" (canto superior direito).
- Após esta ação é possível realizar os testes. Tempo do token expira em 5 minutos.
- Para renovar o token, utilize o serviço /api/refresh. Copie e cole o novo token na opção Authorize.


### Para verificar os arquivos publicados no MinIO, acesse:
```bash
http://localhost:9090/login
```

Username: minio
Senha: miniostorage


### Caso precise excluir tudo para refazer o processo, execute os comandos (prune = informações de cache:):
```bash
docker compose down
```
```bash
docker container prune -f
```
```bash
docker system prune
```


---



### 📌 Endpoints da API

Abaixo estão os principais endpoints da API.


#### 📝 Rotas e Funcionalidades

- Autenticação


| Método  | Endpoint      | Descrição                        |                       Parâmetros / Corpo                         |
|---------|---------------|----------------------------------|------------------------------------------------------------------|
| `GET`   | `/api/login`  | Autenticação do usuário          | `{"email": "teste@seplag.mt.gov.br", "password": "seplag2025" }` |
| `GET`   | `/api/logout` | Deslogar da API                  |                                                                  |
| `POST`  | `/api/refresh`| Renovar o Token de Acesso        | `{"email": "teste@seplag.mt.gov.br", "password": "seplag2025" }` |


### 🔄 Exemplo de Requisição

##### Autenticar um usuário (POST `/api/login`)

```json
{  
  "email": "teste@seplag.mt.gov.br",
  "password": "seplag2025"
}
```

---


- Unidades


| Método  | Endpoint                 | Descrição                      |                      Parâmetros / Corpo                       |
|---------|--------------------------|--------------------------------|---------------------------------------------------------------|
| `GET`   | `/api/unidade`           | Retorna todas as Unidades      | (paginado)                                                    |
| `GET`   | `/api/unidade/{unid_id}` | Retorna uma unidade específica | `unid_id`                                                     |
| `POST`  | `/api/unidade`           | Cadastra uma unidade           | `{ "unid_nome": "Nome unidade", "unid_sigla": "SIGLA-UNID" }` |
| `PUT`   | `/api/unidade/{unid_id}` | Atualiza uma unidade           | `{ "unid_nome": "Novo nome" }`                                |
| `DELETE`| `/api/unidade/{unid_id}` | Exclui uma unidade             | `unid_id`                                                     |


### 🔄 Exemplo de Requisição

##### Cadastrar uma unidade (POST `/api/unidade`)

```json
{
  "unid_nome": "Nome unidade",
  "unid_sigla": "SIGLA-UNID"
}
```

---


- Lotações


| Método  | Endpoint                         | Descrição                                                   |                      Parâmetros / Corpo                                                                                         |
|---------|----------------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| `GET`   | `/api/lotacao`                   | Retorna todas as Lotações (unidades e pessoas relacionadas) | (paginado)                                                                                                                      |
| `GET`   | `/api/lotacao/{lot_id}`          | Retorna lotação específica                                  | `lot_id`                                                                                                                        |
| `GET`   | `/api/lotacao/unidade/{unid_id}` | Pesquisa as pessoas lotadas em uma Unidade específica       | `unid_id`                                                                                                                       |
| `POST`  | `/api/lotacao`                   | Vincula uma pessoa a uma unidade (Lotação)                  | `{ "pes_id": 1, "unid_id": 2, "lot_data_lotacao": "2025-01-30", "lot_data_remocao": NULL, "lot_portaria": "Portaria 01-2025" }` |
| `PUT`   | `/api/lotacao/{lot_id}`          | Atualiza dados da lotação específica                        | `{ "lot_data_lotacao": "2025-01-30", "lot_data_remocao": "2025-04-01", "lot_portaria": "Portaria 01-2025" }`                    |
| `DELETE`| `/api/lotacao/{lot_id}`          | Exclui informação de vínculo de pessoa com unidade          | `lot_id`                                                                                                                        |


### 🔄 Exemplo de Requisição

##### Listas as pessoas lotadas na unidade (unid_id) pesquisada  (GET `/api/lotacao/1`)

```json
{
  "message": "Lotação encontrada",
  "lotacao": {
    "lot_id": 1,
    "pes_id": 1,
    "unid_id": 1,
    "lot_data_lotacao": "2021-01-01",
    "lot_data_remocao": null,
    "lot_portaria": "Portaria 01-2025",
    "created_at": "2025-04-01T20:55:35.000000Z",
    "updated_at": "2025-04-01T20:55:35.000000Z",
    "pessoa": {
      "pes_id": 1,
      "pes_nome": "Nome da primeira pessoa",
      "pes_data_nascimento": "2001-10-10",
      "pes_sexo": "M",
      "pes_mae": "Nome da Mãe 1 pessoa",
      "pes_pai": "Nome do Pai 1 pessoa",
      "created_at": "2025-04-01T20:55:34.000000Z",
      "updated_at": "2025-04-01T20:55:34.000000Z"
    },
    "unidade": {
      "unid_id": 1,
      "unid_nome": "Secretaria de Planejamento",
      "unid_sigla": "SEPLAG",
      "created_at": null,
      "updated_at": null
    }
  }
}
```

---


- Servidor Efetivo


| Método  | Endpoint                         | Descrição                              |                                 Parâmetros / Corpo                             |
|---------|----------------------------------|----------------------------------------|--------------------------------------------------------------------------------|
| `GET`   | `/api/servidor-efetivo`          | Retorna os servidores efetivos         | (paginado)                                                                     |
| `GET`   | `/api/servidor-efetivo/{pes_id}` | Retorna um servidor efetivo específico | `pes_id`                                                                       |
| `POST`  | `/api/servidor-efetivo`          | Cadastra um novo servidor efetivo      | `{ "pes_id": "1", "se_matricula": "00001" }` (necessário cadastrar uma pessoa) |
| `PUT`   | `/api/servidor-efetivo/pes_{id}` | Atualiza um servidor efetivo           | `{ "unid_nome": "Novo nome" }`                                                 |
| `DELETE`| `/api/servidor-efetivo/{pes_id}` | Exclui um servidor efetivo             | `pes_id`                                                                       |


### 🔄 Exemplo de Requisição

##### Cadastrar um Servidor Efetivo (POST `/api/servidor-efetivo`)

```json
{
  "pes_id": "1",
  "se_matricula": "00001"
}
```

---


- Servidor Temporário


| Método  | Endpoint                            | Descrição                               |                                       Parâmetros / Corpo                                 |
|---------|-------------------------------------|-----------------------------------------|------------------------------------------------------------------------------------------|
| `GET`   | `/api/servidor-temporario`          | Retorna os servidores temporários       | (paginado)                                                                               |
| `GET`   | `/api/servidor-temporario/{pes_id}` | Retorna um servidor servidor temporário | `pes_id`                                                                                 |
| `POST`  | `/api/servidor-temporario`          | Cadastra um novo servidor temporário    | `{ "pes_id": "1", "st_data_admissao": "2024-02-10", "st_data_demissao": "2025-01-01" }`  |
| `PUT`   | `/api/servidor-temporario/pes_{id}` | Atualiza um servidor temporário         | `{ "st_data_admissao": "2024-02-10", "st_data_demissao": "2025-01-01" }`                 |
| `DELETE`| `/api/servidor-temporario/{pes_id}` | Exclui um servidor temporário           | `pes_id`                                                                                 |


### 🔄 Exemplo de Requisição

##### Cadastrar um Servidor Temporário (POST `/api/servidor-temporario`)

```json
{
  "pes_id": "5",
  "st_data_admissao": "2024-02-10",
  "st_data_demissao": "2025-01-01"
}
```














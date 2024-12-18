# CRUD FASTAPI

## Instalação via docker

```bash
docker compose up
```

### Uso

Frontend:
Acesse o endereço http://localhost:8501

### Documentação

Backend:
Acesse o endereço http://localhost:8000/docs


## Estrutura de pastas e arquivos

```bash
├── README.md # arquivo com a documentação do projeto
├── backend # pasta do backend (FastAPI, SQLAlchemy, Uvicorn, Pydantic)
├── frontend # pasta do frontend (Streamlit, Requests, Pandas)
├── docker-compose.yml # arquivo de configuração do docker-compose (backend, frontend, postgres)
├── poetry.lock # arquivo de lock do poetry
└── pyproject.toml # arquivo de configuração do poetry
```

## Backend

Nosso backend vai ser uma API, que será responsável por fazer a comunicação entre o nosso frontend com o banco de dados. Vamos detalhar cada uma das pastas e arquivos do nosso backend.

### FastAPI

O FastAPI é um framework web para construir APIs com Python. Ele é baseado no Starlette, que é um framework assíncrono para construir APIs. O FastAPI é um framework que está crescendo muito, e que tem uma curva de aprendizado muito baixa, pois ele é muito parecido com o Flask.

### Uvicorn

O Uvicorn é um servidor web assíncrono, que é baseado no ASGI, que é uma especificação para servidores web assíncronos. O Uvicorn é o servidor web recomendado pelo FastAPI, e é o servidor que vamos utilizar nesse projeto.

### SQLAlchemy

O SQLAlchemy é uma biblioteca para fazer a comunicação com o banco de dados. Ele é um ORM (Object Relational Mapper), que é uma técnica de mapeamento objeto-relacional que permite fazer a comunicação com o banco de dados utilizando objetos.

Uma das principais vantagens de trabalhar com o SQLAlchemy é que ele é compatível com vários bancos de dados, como MySQL, PostgreSQL, SQLite, Oracle, Microsoft SQL Server, Firebird, Sybase e até mesmo o Microsoft Access.


### Pydantic

O Pydantic é uma biblioteca para fazer a validação de dados. Ele é utilizado pelo FastAPI para fazer a validação dos dados que são recebidos na API, e também para definir os tipos de dados que são retornados pela API.

## docker-compose.yml

Esse arquivo `docker-compose.yml` define uma aplicação composta por três serviços: `postgres`, `backend` e `frontend`, e cria uma rede chamada `mynetwork`. Vou explicar cada parte em detalhes:

### Services:

#### Postgres:

* `image: postgres:latest`: Esse serviço utiliza a imagem mais recente do PostgreSQL disponível no Docker Hub.
* `volumes`: Mapeia o diretório `/var/lib/postgresql/data` dentro do contêiner do PostgreSQL para um volume chamado `postgres_data` no sistema hospedeiro. Isso permite que os dados do banco de dados persistam mesmo quando o contêiner é desligado.
* `environment`: Define variáveis de ambiente para configurar o banco de dados PostgreSQL, como nome do banco de dados (`POSTGRES_DB`), nome de usuário (`POSTGRES_USER`) e senha (`POSTGRES_PASSWORD`).
* `networks`: Define que este serviço está na rede chamada `mynetwork`.

#### Backend:

* `build`: Especifica que o Docker deve construir uma imagem para esse serviço, usando um Dockerfile localizado no diretório `./backend`.
* `volumes`: Mapeia o diretório `./backend` (no sistema hospedeiro) para o diretório `/app` dentro do contêiner. Isso permite que as alterações no código fonte do backend sejam refletidas no contêiner em tempo real.
* `environment`: Define a variável de ambiente `DATABASE_URL`, que especifica a URL de conexão com o banco de dados PostgreSQL.
* `ports`: Mapeia a porta `8000` do sistema hospedeiro para a porta `8000` do contêiner, permitindo que o serviço seja acessado através da porta `8000`.
* `depends_on`: Indica que este serviço depende do serviço `postgres`, garantindo que o banco de dados esteja pronto antes que o backend seja iniciado.
* `networks`: Também define que este serviço está na rede `mynetwork`.

#### Frontend:

* `build`: Similar ao backend, especifica que o Docker deve construir uma imagem para este serviço, usando um Dockerfile localizado no diretório `./frontend`.
* `volumes`: Mapeia o diretório `./frontend` (no sistema hospedeiro) para o diretório `/app` dentro do contêiner, permitindo alterações em tempo real.
* `ports`: Mapeia a porta `8501` do sistema hospedeiro para a porta `8501` do contêiner, permitindo acesso ao frontend através da porta `8501`.
* `networks`: Define que este serviço também está na rede `mynetwork`.

### Networks:

* `mynetwork`: Define uma rede personalizada para os serviços se comunicarem entre si.

### Volumes:

* `postgres_data`: Define um volume para armazenar os dados do banco de dados PostgreSQL.

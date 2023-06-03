# Instruções

Esse é um boilerplate para utilização e criação de novos projetos em Phoenix Framework.

Para facilitar o início de novos projetos, esse repositório já contêm os arquivos Dockerfile e docker-compose.

### Duplicando esse repositório

Before you can push the original repository to your new copy, or mirror, of the repository, you must create the new repository on GitHub.com. In these examples, dhonysilva/fazenda.

No terminal, crie um `bare clone` desse repositório.

```
git clone --bare https://github.com/dhonysilva/phxboilerplate.git
```

Mirror-push to the new repository.

```
cd phxboilerplate.git
git push --mirror git@github.com:dhonysilva/fazenda.git
```

Remove the temporary local repository you created earlier.
```
$ cd ..
$ rm -rf phxboilerplate.git
```

Para mais informações, consulte essa [`documentação`](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository). 

### Comandos no docker-compose

Execute o build. Essa etapa irá construir a imagem.
```
docker-compose build
````

Em seguida o up para processar o conteiner.
```
docker-compose up
````


#### Nova aplicação

```
docker-compose run --rm phoenix mix phx.new . --app <nome da aplicação>
```

Uma mensagem de `Fetch and install dependencies` aparecerá. Selecione Yes.

Essa etapa levará um certo tempo.

```
running mix deps.get
running mix assets.setup
running mix deps.compile
```


Após gerar a aplicação, realize a seguinte alteração.

Acesse o arquivo da pasta `/scr/config/devs.exs`
Altere o nome que está na propriedade `hostname` de `localhost` para `db`.

```
# Configure your database
config :fazenda, Fazenda.Repo,
  username: "postgres",
  password: "postgres",
  hostname: "db",
  database: "fazenda_dev",
  stacktrace: true,
  show_sensitive_data_on_connection_error: true,
  pool_size: 10
```

`db` é o nome do service de banco de dados definido no `docker-compose`.

Liste todos os conteineres em operação.
```
docker ps
````

Copie o Id do container dbt.

Execute o comando abaixo para acessar o conteiner.
```
docker exec -it <container-id> /bin/bash
````

#### Novo banco de dados

```
docker-compose run --rm phoenix mix ecto.create
```

O banco de dados Fazenda.Repo será criado.

### Alterar o endereço de IP localhost

No arquivo `src/config/dev.exs`, altere

```
http: [ip: {127, 0, 0, 1}, port: 4000],
```

Para

```
http: [ip: {0, 0, 0, 0}, port: 4000],
```

#### Rodando a aplicação

Execute

```
docker-compose up
```

Now we can visit [`localhost:4000`](http://localhost:4000) from our browser.


Instruções que serão usadas especificamente durante a palestra no Meetup do dia 27/Abril/2013. Excluir após.


Criar o model User

```
docker-compose run --rm phoenix mix phx.gen.live Accounts User users name:string age:integer
```

Criar o model Server

```
docker-compose run --rm phoenix mix phx.gen.live Servers Server servers name:string status:string deploy_count:integer size:float framework:string last_commit_message:string
```

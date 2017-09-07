# Django Basico

## Ambiente de Dev

O ambiente de dev utilizar a box `ubuntu/trusty64`. Para o ambiente `web`
deve ser instalado

```
sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
sudo pip install django flake8 psycopg2
```

Para o ambiente `db` (banco de dados), deve ser instalado

```
sudo apt-get install -y postgresql
```

Após ter tudo instalado, prepare o ambiente `db` com os seguintes comandos:

```
sudo su - postgres -c "psql -f /vagrant/db-config.sql"
sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
sudo service postgresql restart
```

**Após** a preparação do ambiente `db`, prepare o ambiente `web` com os seguintes
comandos.

```
python manage.py migrate
python manage.py loaddata db.fixture.json
```

### Acesso

Para rodar o projeto:

```
python manage.py runserver 0:8000
```

O acesso deve ser feito através do `localhost:8000`.

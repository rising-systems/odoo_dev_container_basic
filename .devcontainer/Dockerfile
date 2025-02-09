ARG DEBIANVARIANT=bookworm
ARG POSTGRESVERSION="16"
FROM mcr.microsoft.com/devcontainers/python:1-3.12-${DEBIANVARIANT}

ARG DEBIANVARIANT
ARG POSTGRESVERSION


ENV PYTHONUNBUFFERED 1


# install postgresql & client + pgbadger (for log analysis)
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ ${DEBIANVARIANT}-pgdg main" >> /etc/apt/sources.list.d/pgdg.list
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN apt update && \
    apt install postgresql-${POSTGRESVERSION} postgresql-client-${POSTGRESVERSION} pgbadger -y

# install wkhtmltopdf (for pdf reports)
RUN apt-get install -y xfonts-75dpi xfonts-base fontconfig libxext6 libxrender1
# # RUN wget http://jaist.dl.sourceforge.net/project/wkhtmltopdf/0.12.1/wkhtmltox-0.12.1_linux-trusty-amd64.deb && \
# RUN wget -O wkhtmltox-amd64.deb https://github.com/wkhtmltopdf/packaging/releases/download/0.12.1.4-2/wkhtmltox_0.12.1.4-2.buster_amd64.deb
# RUN dpkg -i wkhtmltox-amd64.deb
RUN wget -O /tmp/wkhtmltox.deb https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox_0.12.6.1-3.${DEBIANVARIANT}_amd64.deb && \
    dpkg -i /tmp/wkhtmltox.deb && \
    rm /tmp/wkhtmltox.deb && \
    cp /usr/local/bin/wkhtmltopdf /usr/bin && \
    cp /usr/local/bin/wkhtmltoimage /usr/bin

RUN service postgresql start && \
    su - postgres -c "psql -c \"CREATE USER localdevdb WITH PASSWORD 'localdevdb';\"" && \
    su - postgres -c "psql -c \"ALTER USER localdevdb CREATEDB;\"" && \
    su - postgres -c "psql postgres -c \"CREATE EXTENSION IF NOT EXISTS unaccent;\"" && \
    su - postgres -c "psql postgres -c \"CREATE EXTENSION IF NOT EXISTS pg_trgm;\"" && \
    su - postgres -c "psql postgres -c \"CREATE EXTENSION IF NOT EXISTS pgcrypto;\"" && \
    su - postgres -c "psql postgres -c \"ALTER FUNCTION unaccent(text) OWNER to localdevdb;\"" && \
    su - postgres -c "psql postgres -c \"ALTER FUNCTION unaccent(text) IMMUTABLE;\"" && \
    su - postgres -c "psql template1 -c \"CREATE EXTENSION IF NOT EXISTS unaccent;\"" && \
    su - postgres -c "psql template1 -c \"CREATE EXTENSION IF NOT EXISTS pg_trgm;\"" && \
    su - postgres -c "psql template1 -c \"CREATE EXTENSION IF NOT EXISTS pgcrypto;\"" && \
    su - postgres -c "psql template1 -c \"ALTER FUNCTION unaccent(text) OWNER to localdevdb;\"" && \
    su - postgres -c "psql template1 -c \"ALTER FUNCTION unaccent(text) IMMUTABLE;\""

RUN apt update && \
    apt install -y libldap2-dev libsasl2-dev

RUN pip install pip --upgrade

RUN ln -s /workspaces/odoo/odoo.conf /home/vscode/.odoorc

RUN echo 'alias odoo-bin="python3 /workspaces/odoo/community/odoo-bin"' >> /home/vscode/.bashrc && \
    echo 'echo use alias odoo-bin to run odoo' >> /home/vscode/.bashrc && \
    echo 'echo example: odoo-bin -d some_new_db_to_create -i base,web --stop-after-init' >> /home/vscode/.bashrc

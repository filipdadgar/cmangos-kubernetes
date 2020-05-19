FROM mariadb
RUN apt-get update -y && apt-get upgrade -y

RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y \
	git \
	wget \
	bash

RUN git clone git://github.com/cmangos/classic-db.git
RUN git clone git://github.com/cmangos/mangos-classic.git

ENV MYSQL_ROOT_PASSWORD mangos
ENV MYSQL_USER mangos
ENV MYSQL_PASSWORD mangos

RUN mkdir -p /cmangos/sql
ADD db_create_mysql.sql /cmangos/sql
RUN wget -O /cmangos/sql/characters.sql https://raw.githubusercontent.com/cmangos/mangos-classic/master/sql/base/characters.sql
RUN wget -O /cmangos/sql/mangos.sql https://raw.githubusercontent.com/cmangos/mangos-classic/master/sql/base/mangos.sql
RUN wget -O /cmangos/sql/realmd.sql https://raw.githubusercontent.com/cmangos/mangos-classic/master/sql/base/realmd.sql

COPY mangos-classic/sql/base/dbc/cmangos_fixes /cmangos/base/dbc/cmangos_fixes
COPY mangos-classic/sql/base/dbc/original_data /cmangos/base/dbc/original_data
COPY mangos-classic/sql/updates /cmangos/sql/updates

ADD InstallFullDB.config /classic-db/InstallFullDB.config
RUN chmod -R 777 /classic-db/*.config

ADD sqlstart.sh /docker-entrypoint-initdb.d

USER mysql
VOLUME /var/lib/mysql

ADD my.cnf /etc/mysql/

EXPOSE 3306
CMD ["mysqld", "--max-allowed-packet=32505856"]

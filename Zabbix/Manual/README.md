# Instalação manual do zabbix

+ Passo a passo feito no sistema operacional Unbuntu Bionic.

## Passo a passo

1. Instalando dependencias:

  ```dependencias
  $ sudo apt install php apache2 mysql-server -y
  ```

2. Instalando repositório:

  ```repo
  $ wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+bionic_all.deb
  $ sudo dpkg -i zabbix-release_5.0-1+bionic_all.deb
  $ sudo apt update
  ```

3. Instalando o zabbix server, frontend e agent:

  ```zabbixSrvFrontAgent
  $ apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent
  ```

4. Criando o database:

  ```zabbixDB
  $ sudo mysql -u root
  mysql> CREATE DATABASE zabbix CHARACTER SET UTF8 COLLATE UTF8_BIN;
  mysql> CREATE USER zabbix@localhost IDENTIFIED BY 'admin123';
  mysql> GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost;
  mysql> QUIT;
  ```

5. Importar o esquema e os dados iniciais:

  ```zabbixInitialDB
  $ zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
  ```

6. Configurar a senha do DB no arquivo zabbix_server.conf:

  ```zabbixServerConf
  DBPassword=password
  ```

7. Descomente e altere para America/Bahia a linha no arquivo apache.conf:

  ```timeZone
  php_value date.timezone America/Bahia
  ```

8. Resetar e dar enable nos serviços:

  ```zabbixServRestEn
  $ sudo systemctl restart zabbix-server zabbix-agent apache2
  $ sudo systemctl enable zabbix-server zabbix-agent apache2
  ```


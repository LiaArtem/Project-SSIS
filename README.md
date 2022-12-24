- Project SSIS VS2022 (Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, MariaDB, IBM DB2, IBM Informix, Firebird, SQLite, XML file, XML web, JSON file, JSON web, CSV file -> MS SQL 2022).

При начальноей разработке:
- Установите для свойства SSIS Package ProtectionLevel значение EncryptSensitiveWithPassword - пароль 12345678
- Интерфейс -> Вид -> Другие окна -> SSIS Toolbox

!!! пароль для подключения 12345678

-> Oracle Driver
------------------------------------------------------
  *** ODBC (вариант 1)
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- скачиваем и ставим Oracle клиента x64 - NT_213000_client_x64.zip или более новый
- после установки меняем в глоб. реестре:
  - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
- копируем файлы tnsnames.ora и sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
- или настраиваем tnsnames.ora для настройки x64 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
  - запускаем -> Источники данных ODBC (64-разрядная версия)
  - проверяем -> Драйверы, должен появится -> Oracle in OraClient21Home1_64bit
  - выбираем -> Системный DSN -> Добавить:
    - Data Source Name: Oracle_x64
    - Description: Oracle_x64
    - TNS Service Name: XE
    - User ID: TEST_USER
    - OK
  - настраиваем ODBC подключение в SSIS
  - поставщик ODBC: System data source name = Oracle_x64
  - логин = TEST_USER и пароль = !Aa112233.

  *** OLE DB (вариант 2)
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- скачиваем и ставим Oracle клиента x64 - NT_213000_client_x64.zip или более новый
- после установки меняем в глоб. реестре:
  - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
- копируем файлы tnsnames.ora и sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
- или настраиваем tnsnames.ora для настройки x64 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
- настраиваем OLE DB подключение в SSIS
  - поставщик OLE DB: Oracle provider for OLE DB
  - имя базы = XE, логин = TEST_USER и пароль = !Aa112233.

  *** ODBC (вариант 3)
  - инструкция - https://www.oracle.com/cis/database/technologies/releasenote-odbc-ic.html
  - https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html

  - скачиваем Oracle Instant Client Basic Package - instantclient-basic-windows.x64-21.8.0.0.0dbru.zip
  - распаковываем в папку c:\oracle\product\, если их нет создаем
  - добавляем в Переменные среды -> Cистемные переменные -> Path = c:\oracle\product\instantclient_21_8\
  - скачиваем SQL*Plus Package - instantclient-sqlplus-windows.x64-21.8.0.0.0dbru.zip
  - распаковываем в папку c:\oracle\product\
  - копируем файлы tnsnames.ora и sqlnet.ora в папку c:\oracle\product\instantclient_21_8\network\admin\
  - проверяем cmd:
    - sqlplus /nolog
    - connect TEST_USER/!Aa112233@XE
    - exit
  - скачиваем ODBC Package - instantclient-odbc-windows.x64-21.8.0.0.0dbru.zip
  - распаковываем в папку c:\oracle\product\
  - запускаем cmd под администратором
    - cd c:\oracle\product\instantclient_21_8\
    - odbc_install
  - запускаем -> Источники данных ODBC (64-разрядная версия)
  - проверяем -> Драйверы, должен появится -> Oracle in instantclient_21_8
  - выбираем -> Системный DSN -> Добавить:
    - Data Source Name: Oracle_x64
    - Description: Oracle_x64
    - TNS Service Name: XE
    - User ID: TEST_USER
    - OK
  - настраиваем ODBC подключение в SSIS
  - поставщик ODBC: System data source name = Oracle_x64
  - логин = TEST_USER и пароль = !Aa112233.

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x64.zip или более новую
- ODBC прописать при подключении в SSIS
- Driver={PostgreSQL ANSI};server=localhost;uid=testdb;port=5432;database=testdb;Uid=testdb;Pwd=!Aa112233;

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.31-winx64.msi или более новую
- ODBC прописать при подключении в SSIS
- Driver={MySQL ODBC 8.0 ANSI Driver};server=localhost;uid=root;port=3306;found_rows=1;User=root;Password=!Aa112233;Option=3;

-> MariaDB ODBC Driver
------------------------------------------------------
- https://mariadb.com/kb/en/mariadb-connector-odbc/
- скачиваем и ставим mariadb-connector-odbc-3.1.17-win64.msi или более новую
- ODBC прописать при подключении в SSIS
- Driver={MariaDB ODBC 3.1 Driver};server=localhost;uid=root;port=3307;option=3;Password=!Aa112233;

-> SQLite ODBC Driver
------------------------------------------------------
- скачиваем и ставим http://www.ch-werner.de/sqliteodbc/ текущую версию sqliteodbc.exe.
- открыть «Администрирование» -> «Источники данных» (ODBC).
  - на вкладке System DSN -> SQLite3 Datasource (64bit) прописать путь к базе данных c:\DB_SQLite\CurrencyChartFXMaven.db

-> IBM DB2 Driver
------------------------------------------------------
- OLE DB устанавливается вместе с базой данных (если локальная база данных)
- настраиваем OLE DB подключение в SSIS
  - поставщик OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
  - логин = db2admin и пароль = !Aa112233.

- *** ODBC (если база данных находится на отдельном сервере)
- настраиваем ODBC подключение в SSRS
  - https://www.ibm.com/support/pages/node/6830623 поиск IBM Data Server Driver for ODBC and CLI (64-bit) -> Download
    - IBM Data Server Driver for ODBC and CLI (Windows/x86-64 64 bit) V11.5.8 Fix Pack 0
  - скачать IBM Data Server Driver for ODBC and CLI (64-bit) -> v11.5.8_ntx64_odbc_cli.zip
  - распаковать содержимое в c:\Program Files\IBM\ предварително создав папку IBM
  - добавляем в Переменные среды -> Cистемные переменные -> Path = c:\Program Files\IBM\clidriver\bin
  - запускаем под администатором: cmd db2oreg1 –i
  - запускаем под администатором: cmd db2oreg1 –setup
  - запускаем -> Источники данных ODBC (64-разрядная версия)
  - проверяем -> Драйверы, должен появится -> IBM DATA SERVER DRIVER for ODBC.....
  - выбираем -> Системный DSN -> Добавить:
    - Data Source Name: ibmdb2_odbc
    - Description: ibmdb2_odbc
    - Database alias: -> Add
      - Data Source:
        - User ID: DB2INST1
        - Password: !Aa112233
        - check Save password
      - TCP/IP:
        - Database name: sample
        - Host name: localhost
        - Port: 50000
      - OK
  - настраиваем ODBC подключение в SSIS
  - поставщик ODBC: System data source name = ibmdb2_odbc
  - логин = DB2INST1 и пароль = !Aa112233.

-> IBM Informix ODBC Driver
------------------------------------------------------
- Скачиваем и устанавливаем IBM Informix Client SDKV 4.50 (ibm.csdk.4.50.FC8.WIN.zip)
  - Если база данных не локальная необходимо настроить SSL подключение:
    - включить при установке - Use OPENSSL instead of GSKit?
    - Windows -> Источники данных ODBC (64 разрадная версия) -> Системный DSN
      - General:
        - Data Source Name - informix_odbc
        - Description - informix_odbc
      - Connection:
        - Server Name - informix_test
        - Host Name - localhost
        - Service - turbo_test
        - Database Name - sample
        - User Id - informix
        - Password - !Aa112233
  - настраиваем ODBC подключение в SSIS
  - поставщик ODBC: System data source name = informix_odbc
  - логин = informix и пароль = !Aa112233.

-> Firebird ODBC Driver (если локальная база данных)
------------------------------------------------------
- https://firebirdsql.org/en/odbc-driver/
- скачиваем и ставим Firebird_ODBC_2.0.5.156_x64.exe или более новую
- ODBC прописать при подключении в SSIS
- Driver={Firebird/InterBase(r) driver};uid=SYSDBA;dbname=C:\Windows\System32\SAMPLEDATABASE.FDB



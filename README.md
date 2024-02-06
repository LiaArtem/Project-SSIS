- Project SSIS VS2022 (Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, MariaDB, IBM DB2, IBM Informix, Firebird, SQLite, XML file, XML web, JSON file, JSON web, CSV file -> MS SQL 2022).

При початковій розробці:
- Встановіть для властивості SSIS Package ProtectionLevel значення EncryptSensitiveWithPassword - пароль 12345678
- Інтерфейс -> Вид -> Інші вікна -> SSIS Toolbox

!!! пароль для підключення 12345678
!!! якщо пише що несумістно - правою кнопкою - перезавантажити проект

1) Встановлюємо SQL Server та інші бази даних з проекту - Docker-Win11
   https://github.com/LiaArtem/Docker-Win11/
   https://github.com/LiaArtem/Oracle_23c_Free/

   !!!! Если у ПК <= 8 Гб ОП у Docker нормальна робота всіх тестових баз та бази IBM DB2 не можлива,
   не вистачає ресурсів, IBM DB2 тестується окремо с базой MSSQL.

2) Заповнюємо бази данних даними запускаючи проект - CurrencyChartFX-Java-21-Maven
   https://github.com/LiaArtem/CurrencyChartFX-Java-21-Maven (налаштуванная у файлі - Settings (SSIS).json)

-> Oracle OLE DB Driver
------------------------------------------------------
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- завантажуємо та ставимо Oracle клієнта x64 - NT_213000_client_x64.zip або новіший
- після встановлення міняємо у глоб. реєстрі:
  - Комп'ютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 з AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (або AMERICAN_AMERICA.CL8MSWIN1251)
- копіюємо файли tnsnames.ora і sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
- або налаштовуємо tnsnames.ora для налаштування x64 Oracle Client:
    FREE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP) (HOST = localhost) (PORT = 1521))) (CONNECT_DATA = (SERVICE_NAME = FREE)))
- налаштовуємо OLE DB підключення до SSIS
  - постачальник OLE DB: Oracle provider for OLE DB
  - ім'я бази = FREE, логін = TEST_USER та пароль = !Aa112233.

-> SQLite ODBC Driver
------------------------------------------------------
- Базу SQLite перенести - C:\DB_SQLite\CurrencyChartFXMaven.db
- http://www.ch-werner.de/sqliteodbc/
- завантажуємо та ставимо sqliteodbc_w64.exe
- Запускаємо -> Джерела даних ODBC x64 -> Системний DSN -> SQLite Datasource x64 -> Прописати Database Name = C:\DB_SQLite\CurrencyChartFXMaven.db
- ODBC прописати при підключенні до SSRS
- Driver={SQLite3 ODBC Driver};database=C:\\DB_SQLite\\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- завантажуємо та ставимо psqlodbc_16_00_0000-x64.zip або новішу
- ODBC прописати при підключенні до SSIS
- Driver={PostgreSQL ANSI};server=localhost;uid=testdb;port=5432;database=testdb;Uid=testdb;Pwd=!Aa112233;

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- завантажуємо та ставимо mysql-connector-odbc-8.3.0-winx64.msi або новішу
- ODBC прописати при підключенні до SSIS
- Driver={MySQL ODBC 8.3 ANSI Driver};server=localhost;uid=root;port=3306;found_rows=1;User=root;Password=!Aa112233;Option=3;

-> MariaDB ODBC Driver
------------------------------------------------------
- https://mariadb.com/kb/en/mariadb-connector-odbc/
- завантажуємо та ставимо mariadb-connector-odbc-3.1.20-win64.msi або новішу
- ODBC прописати при підключенні до SSIS
- Driver={MariaDB ODBC 3.1 Driver};server=localhost;uid=root;port=3307;option=3;Password=!Aa112233;

-> IBM DB2 Driver
------------------------------------------------------
- OLE DB встановлюється разом із базою даних (якщо локальна база даних)
- налаштовуємо OLE DB підключення до SSIS
   - постачальник OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
   - логін = db2admin та пароль = !Aa112233.

- *** ODBC (якщо база даних знаходиться на окремому сервері)
- налаштовуємо ODBC підключення до SSRS
   - https://www.ibm.com/support/pages/node/6830623 пошук IBM Data Server Driver for ODBC and CLI (64-bit) -> Download
     - IBM Data Server Driver for ODBC and CLI (Windows/x86-64 64-bit) V11.5.9 Fix Pack 0
   - скачати IBM Data Server Driver for ODBC and CLI (64-bit) -> v11.5.9_ntx64_odbc_cli.zip
   - розпакувати вміст у c:\Program Files\IBM\ попередньо створивши папку IBM
   - !!!!! запускаємо cmd під адміністратором
     - cmd -> sysdm.cpl -> Додатково -> Змінні оточення
     - додаємо в Змінні середовища -> Системні змінні -> Path = c:\Program Files\IBM\clidriver\bin
     - запускаємо під адміністатором: cmd -> cd c:\Program Files\IBM\clidriver\bin
     - запускаємо під адміністатором: cmd -> db2oreg1 -i
     - запускаємо під адміністатором: cmd -> db2oreg1 -setup
   - запускаємо -> Джерела даних ODBC (64-розрядна версія)
   - перевіряємо -> Драйвери, повинен з'явиться -> IBM DATA SERVER DRIVER for ODBC.
   - вибираємо -> Системний DSN -> Додати:
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
   - налаштовуємо ODBC підключення до SSIS
   - постачальник ODBC: System data source name = ibmdb2_odbc
   - логін = DB2INST1 та пароль = !Aa112233.

-> IBM Informix ODBC Driver (якщо локальна база даних)
------------------------------------------------------
- Завантажуємо та встановлюємо IBM Informix Client SDKV 4.50 (ibm.csdk.4.50.FC8.WIN.zip)
   - Якщо база даних не локальна, необхідно налаштувати SSL підключення:
     - увімкнути під час встановлення - Use OPENSSL instead of GSKit?
     - Windows -> Джерела даних ODBC (64 розрядна версія) -> Системний DSN
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
   - налаштовуємо ODBC підключення до SSIS
   - постачальник ODBC: System data source name = informix_odbc
   - логін = informix та пароль = !Aa112233.

-> Firebird ODBC Driver (якщо локальна база даних)
------------------------------------------------------
- https://firebirdsql.org/en/odbc-driver/
- завантажуємо та ставимо Firebird_ODBC_2.0.5.156_x64.exe або новішу
- ODBC прописати при підключенні до SSIS
- Driver={Firebird/InterBase(r) driver};uid=SYSDBA;dbname=C:\Windows\System32\SAMPLEDATABASE.FDB



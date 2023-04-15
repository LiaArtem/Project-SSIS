- Project SSIS VS2022 (Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, MariaDB, IBM DB2, IBM Informix, Firebird, SQLite, XML file, XML web, JSON file, JSON web, CSV file -> MS SQL 2022).

При початковій розробці:
- Встановіть для властивості SSIS Package ProtectionLevel значення EncryptSensitiveWithPassword - пароль 12345678
- Інтерфейс -> Вид -> Інші вікна -> SSIS Toolbox

!!! пароль для підключення 12345678
!!! якщо пише що несумістно - правою кнопкою - перезавантажити проект

-> Oracle Driver
------------------------------------------------------
*** ODBC (варіант 1)
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- завантажуємо та ставимо Oracle клієнта x64 - NT_213000_client_x64.zip або новіший
- після встановлення міняємо у глоб. реєстрі:
  - Комп'ютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 з AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (або AMERICAN_AMERICA.CL8MSWIN1251)
- копіюємо файли tnsnames.ora і sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
- або налаштовуємо tnsnames.ora для налаштування x64 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
- запускаємо -> Джерела даних ODBC (64-розрядна версія)
  - перевіряємо -> Драйвери, повинен з'явиться -> Oracle in OraClient21Home1_64bit
  - вибираємо -> Системний DSN -> Додати:
    - Data Source Name: Oracle_x64
    - Description: Oracle_x64
    - TNS Service Name: XE
    - User ID: TEST_USER
    - OK
  - налаштовуємо ODBC підключення до SSIS
  - постачальник ODBC: System data source name = Oracle_x64
  - логін = TEST_USER та пароль = !Aa112233.

*** OLE DB (варіант 2)
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- завантажуємо та ставимо Oracle клієнта x64 - NT_213000_client_x64.zip або новіший
- після встановлення міняємо у глоб. реєстрі:
  - Комп'ютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 з AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (або AMERICAN_AMERICA.CL8MSWIN1251)
- копіюємо файли tnsnames.ora і sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
- або налаштовуємо tnsnames.ora для налаштування x64 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP) (HOST = localhost) (PORT = 1521))) (CONNECT_DATA = (SERVICE_NAME = XE)))
- налаштовуємо OLE DB підключення до SSIS
  - постачальник OLE DB: Oracle provider for OLE DB
  - ім'я бази = XE, логін = TEST_USER та пароль = !Aa112233.

*** ODBC (варіант 3)
  - інструкція - https://www.oracle.com/cis/database/technologies/releasenote-odbc-ic.html
  - https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html

  - завантажуємо Oracle Instant Client Basic Package - instantclient-basic-windows.x64-21.8.0.0.0dbru.zip
  - розпаковуємо в папку c:\oracle\product\, якщо їх немає
  - додаємо в Змінні середовища -> Системні змінні -> Path = c:\oracle\product\instantclient_21_8\
  - завантажуємо SQL*Plus Package - instantclient-sqlplus-windows.x64-21.8.0.0.0dbru.zip
  - розпаковуємо в папку c:\oracle\product\
  - копіюємо файли tnsnames.ora і sqlnet.ora в папку c:\oracle\product\instantclient_21_8\network\admin\
  - перевіряємо cmd:
     - sqlplus /nolog
     - connect TEST_USER/!Aa112233@XE
     - exit
   - завантажуємо ODBC Package - instantclient-odbc-windows.x64-21.8.0.0.0dbru.zip
   - розпаковуємо в папку c:\oracle\product\
   - запускаємо cmd під адміністратором
     - cd c:\oracle\product\instantclient_21_8\
     - odbc_install
   - запускаємо -> Джерела даних ODBC (64-розрядна версія)
   - перевіряємо -> Драйвери, повинен з'явиться -> Oracle in instantclient_21_8
   - вибираємо -> Системний DSN -> Додати:
     - Data Source Name: Oracle_x64
     - Description: Oracle_x64
     - TNS Service Name: XE
     - User ID: TEST_USER
     - OK
   - налаштовуємо ODBC підключення до SSIS
   - постачальник ODBC: System data source name = Oracle_x64
   - логін = TEST_USER та пароль = !Aa112233.

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- завантажуємо та ставимо psqlodbc_13_02_0000-x64.zip або новішу
- ODBC прописати при підключенні до SSIS
- Driver={PostgreSQL ANSI};server=localhost;uid=testdb;port=5432;database=testdb;Uid=testdb;Pwd=!Aa112233;

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- завантажуємо та ставимо mysql-connector-odbc-8.0.31-winx64.msi або новішу
- ODBC прописати при підключенні до SSIS
- Driver={MySQL ODBC 8.0 ANSI Driver};server=localhost;uid=root;port=3306;found_rows=1;User=root;Password=!Aa112233;Option=3;

-> MariaDB ODBC Driver
------------------------------------------------------
- https://mariadb.com/kb/en/mariadb-connector-odbc/
- завантажуємо та ставимо mariadb-connector-odbc-3.1.17-win64.msi або новішу
- ODBC прописати при підключенні до SSIS
- Driver={MariaDB ODBC 3.1 Driver};server=localhost;uid=root;port=3307;option=3;Password=!Aa112233;

-> SQLite ODBC Driver
------------------------------------------------------
- завантажуємо та ставимо http://www.ch-werner.de/sqliteodbc/ поточну версію sqliteodbc.exe.
- відкрити «Адміністрування» -> «Джерела даних» (ODBC).
   - на вкладці System DSN -> SQLite3 Datasource (64bit) прописати шлях до бази даних c:\DB_SQLite\CurrencyChartFXMaven.db

-> IBM DB2 Driver
------------------------------------------------------
- OLE DB встановлюється разом із базою даних (якщо локальна база даних)
- налаштовуємо OLE DB підключення до SSIS
   - постачальник OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
   - логін = db2admin та пароль = !Aa112233.

- *** ODBC (якщо база даних знаходиться на окремому сервері)
- налаштовуємо ODBC підключення до SSRS
   - https://www.ibm.com/support/pages/node/6830623 пошук IBM Data Server Driver for ODBC and CLI (64-bit) -> Download
     - IBM Data Server Driver for ODBC and CLI (Windows/x86-64 64-bit) V11.5.8 Fix Pack 0
   - скачати IBM Data Server Driver for ODBC and CLI (64-bit) -> v11.5.8_ntx64_odbc_cli.zip
   - розпакувати вміст у c:\Program Files\IBM\ попередньо створивши папку IBM
   - додаємо в Змінні середовища -> Системні змінні -> Path = c:\Program Files\IBM\clidriver\bin
   - запускаємо під адміністатором: cmd db2oreg1 -i
   - запускаємо під адміністатором: cmd db2oreg1 -setup
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

-> IBM Informix ODBC Driver
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



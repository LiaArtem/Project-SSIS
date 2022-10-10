- Project SSIS VS2019 (Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, IBM DB2, IBM Informix, Firebird, SQLite, XML file, XML web, JSON file, JSON web, CSV file -> MS SQL 2022).

Установите для свойства SSIS Package ProtectionLevel значение EncryptSensitiveWithPassword - пароль 12345678

!!! Visual Studio 2019 запуск проекта под Администратором на ПК
----------------------------------------------------------------------------

----------------------------------------------------------------------------
Visual Studio 2019 - SSIS работает с x86 ODBC Drivers и x86 Oracle Client
----------------------------------------------------------------------------

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.30-win32.msi или более новую
- ODBC прописать при подключении в SSIS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=12345678;Option=3;

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x86.zip или более новую
- ODBC прописать при подключении в SSIS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=test_database;Uid=test_user;Pwd=12345678;

-> Oracle OLE DB Driver
------------------------------------------------------
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- скачиваем и ставим Oracle клиента x86 - NT_213000_client.zip или более новый
- после установки меняем в глоб. реестре:
  - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\ORACLE\KEY_OraClient19Home1_32bit c AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
- настраиваем tnsnames.ora для настройки x86 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
- настраиваем OLE DB подключение в SSIS
  - поставщик OLE DB: Oracle provider for OLE DB
  - имя базы = XE, логин = TEST_USER и пароль = TEST_USER.

-> SQLite ODBC Driver
------------------------------------------------------
- скачиваем и ставим http://www.ch-werner.de/sqliteodbc/ текущую версию sqliteodbc.exe.
- открыть «Администрирование» -> «Источники данных» (ODBC).
  - на вкладке System DSN -> SQLite3 Datasource (32bit) прописать путь к базе данных c:\DB_SQLite\CurrencyChartFXMaven.db

-> IBM DB2 OLE DB Driver
------------------------------------------------------
- OLE DB устанавливается вместе с базой данных
- настраиваем OLE DB подключение в SSRS
  - поставщик OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
  - логин = db2admin и пароль = 12345678.

-> IBM Informix ODBC Driver
------------------------------------------------------
- Windows -> Переменные среды -> Системные переменные -> параметр PATH добавляем C:\Windows\SysWOW64;
- Перегрузить ПК
- !!!! Скачиваем и устанавливаем IBM Informix Client SDKV 4.10 x32 (ibm.csdk.4.10.TC9DE.WIN (x32).zip) в папку C:\Program Files (x86)\IBM Informix Client SDK
- Windows -> Источники данных ODBC (32 разрадная версия) -> Системный DSN
  - General:
    - Data Source Name - informix_odbc_x32
    - Description - informix_odbc_x32
  - Connection:
    - Server Name - informix_test
    - Host Name - localhost
    - Service - turbo_test
    - Protocol - olsoctcp
    - Database Name - sample
    - User Id - informix
    - Password - 12345678

-> Firebird ODBC Driver
------------------------------------------------------
- Скачиваем и устанавливаем Firebird_ODBC_2.0.5.156_Win32.exe

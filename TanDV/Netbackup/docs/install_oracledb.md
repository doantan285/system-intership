# Cài Oracle DB trên Oracle linux 7.9

## I. Chuẩn bị hệ điều hành

### 1. Kiểm tra OS

```bash
cat /etc/os-release
```

Kết quả mong muốn:

```bash
Oracle Linux Server release 7.9
```

### 2. Cấu hình hostname và hosts

Đặt hostname:

```bash
hostnamectl set-hostname test-oracle-db-nbu.novalocal
```

Sửa file `/etc/hosts`, thêm dòng sau:

```bash
10.171.133.47  test-oracle-db-nbu.novalocal test-oracle-db-nbu
```

>Oracle rất nhạy DNS, hostname phải resolve được.

## II. Tắt SELinux và firewall

## III. Cài các package cần thiết cho oracle

### 1. Cài repo & update

```bash
yum clean all
yum update -y
```

### 2. Cài preinstall package

Oracle cung cấp sẵn package cấu hình kernel, user, limits.

```bash
yum install -y oracle-database-preinstall-19c
```

- Package này tự động:
  - Tạo user `oracle`
  - Set kernel params
  - Set ulimit
  - Cài thư viện phụ thuộc

### 3. Kiểm tra user oracle

```bash
id oracle
```

Kết quả mong muốn:

```bash
uid=54321(oracle) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54323(oper),54324(backupdba),54325(dgdba),54326(kmdba),54330(racdba)
```

## IV. Chuẩn bị thư mục cài Oracle

### 1. Tạo thư mục Oracle

```bash
# Chuyển sang user root
su -
# Tạo thư mục
mkdir -p /u01/app/oracle/product/19c/dbhome_1
mkdir -p /u01/app/oraInventory
chown -R oracle:oinstall /u01
chmod -R 775 /u01
```

### 2. Cấu hình biến môi trườn cho user oracle

```bash
# Chuyển sang user oracle
su - oracle
# Mở file
vi ~/.bash_profile
```

Thêm các dòng sau vào cuối file:

```bash
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/19c/dbhome_1
export ORACLE_SID=ORCL
export PATH=$PATH:$ORACLE_HOME/bin
```

Load lại:

```bash
source ~/.bash_profile
```

## V. Chuẩn bị bộ cài Oracle Database

### 1. File nằm ở đâu?

File cần: `LINUX.X64_193000_db_home.zip`

Tải trên trang Oracle Software Delivery Cloud

### 2. Giải nén

Copy file zip vào:

```bash
/u01/app/oracle/product/19c/dbhome_1
```

Giải nén:

```bash
cd $ORACLE_HOME
unzip LINUX.X64_193000_db_home.zip
```

## VI. Cài Oracle Software (runInstaller)

### 1. Chạy installer

```bash
cd $ORACLE_HOME
./runInstaller
```

Nếu lỗi DISPLAY (SSH), thực hiện các bước dưới đây:

Kiểm tra oracle user & biến môi trường

```bash
su - oracle
echo $ORACLE_HOME
echo $ORACLE_BASE
```

Phải ra:

```bash
/u01/app/oracle/product/19c/dbhome_1
/u01/app/oracle
```

Tạo file response cho installer, oracle đã có sẵn file mẫu:

```bash
cd $ORACLE_HOME/install/response
ls
```

Sẽ thấy file `db_install.rsp`

Copy và chỉnh file response:

```bash
cp db_install.rsp /home/oracle/db_install_19c.rsp
vi /home/oracle/db_install_19c.rsp
```

Chinh sửa các dòng sau:

```bash
oracle.install.option=INSTALL_DB_SWONLY

UNIX_GROUP_NAME=oinstall
INVENTORY_LOCATION=/u01/app/oraInventory

ORACLE_HOME=/u01/app/oracle/product/19c/dbhome_1
ORACLE_BASE=/u01/app/oracle

oracle.install.db.InstallEdition=EE

oracle.install.db.OSDBA_GROUP=dba
oracle.install.db.OSOPER_GROUP=oper

oracle.install.db.OSBACKUPDBA_GROUP=dba
oracle.install.db.OSDGDBA_GROUP=dba
oracle.install.db.OSKMDBA_GROUP=dba
oracle.install.db.OSRACDBA_GROUP=dba

oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
oracle.install.db.config.starterdb.globalDBName=ORCL
oracle.install.db.config.starterdb.SID=ORCL

oracle.install.db.config.starterdb.characterSet=AL32UTF8
oracle.install.db.config.starterdb.memoryOption=false
oracle.install.db.config.starterdb.installExampleSchemas=false

DECLINE_SECURITY_UPDATES=true
```

Chạy runInstalle

```bash
cd $ORACLE_HOME
./runInstaller \
-silent \
-responseFile /home/oracle/db_install_19c.rsp \
-ignorePrereqFailure
```

## VII. Tạo database (DBCA – tối ưu cho NetBackup test)

### 1. Tạo database

```bash
su - oracle

dbca -silent -createDatabase \
-templateName General_Purpose.dbc \
-gdbname ORCL \
-sid ORCL \
-createAsContainerDatabase false \
-databaseType OLTP \
-characterSet AL32UTF8 \
-sysPassword Oracle123 \
-systemPassword Oracle123 \
-totalMemory 1536 \
-emConfiguration NONE \
-storageType FS \
-datafileDestination /u01/app/oracle/oradata
```

Kiểm tra sau khi tạo:

```bash
ps -ef | grep pmon
```

Phải thấy:

```bash
ora_pmon_ORCL
```

### 2. SQLPlus

```bash
sqlplus / as sysdba
```

Chạy lệnh sql:

```sql
select name, open_mode, database_role from v$database;
```

Kết quả mong muốn:

```text
ORCL | READ WRITE | PRIMARY
```

## VIII. Kiểm tra listeners

```bash
lsnrctl status
```

Phải thấy service ORCL:

```text
Service "ORCL" has 1 instance(s).
```

Nếu chưa có listeners, chạy lệnh:

```bash
netca -silent -responseFile $ORACLE_HOME/assistants/netca/netca.rsp
```

## IX. Chuẩn bị oracle cho netbackup

### 1. Kiểm tra OSBACKUPDBA group

```bash
id oracle
```

Phải có:

```text
groups=...,54324(backupdba),...
```

> Netbackup RMAN dùng dòng này

### 2. Kiểm tra RMAN chạy OK

```bash
rman target /
show all;
```

Thoát RMAN:

```bash
exit;
```

## X. Cấu hình auto-start (Để netbackup không fail)

```bash
vi /etc/oratab
```

Thêm:

```bash
ORCL:/u01/app/oracle/product/19c/dbhome_1:Y
```

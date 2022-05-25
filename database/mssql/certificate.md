## How to backup database with encryption

#### Step 1: Create certificate

```sql
USE [master]
GO

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<MASTER KEY PASSWORD>'
GO

CREATE CERTIFICATE my_certificate WITH SUBJECT = '<USER>';
GO
```

#### Step 2: Backup certificate

```sql
BACKUP CERTIFICATE my_certificate
TO FILE = '<PATH>\my_certificate.cer'
WITH PRIVATE KEY (
        FILE = '<PATH>\my_certificate.pvk',
        ENCRYPTION BY PASSWORD = '<MASTER KEY PASSWORD>'
    );
GO
```

#### Step 3: Backup to disk with encryption

```sql
USE [master]
GO

BACKUP DATABASE [DB_Test] TO  DISK = N'<PATH>\DB_Test.bak' WITH
COPY_ONLY, NOFORMAT, NOINIT,
NAME = N'DB_Test-Full Database Backup',
SKIP, NOREWIND,
NOUNLOAD, COMPRESSION,
ENCRYPTION (
  ALGORITHM = AES_256,
  SERVER CERTIFICATE = my_certificate
),
STATS = 10
GO
```

### How to import certificate in another instance

```sql
USE [master]
GO

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<MASTER KEY PASSWORD>';
GO

CREATE CERTIFICATE my_certificate
    AUTHORIZATION [dbo]
    FROM FILE = '<PATH>\my_certificate.cer'
    WITH PRIVATE KEY (
    FILE = '<PATH>\my_certificate.pvk',
    DECRYPTION BY PASSWORD = '<MASTER KEY PASSWORD>'
);
GO
```

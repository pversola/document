# ETXNPAPRDLB02

/*
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Q2hAbndhbjFjaENTUA==';
CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate
   TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
   WITH PRIVATE KEY (
	FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
	ENCRYPTION BY PASSWORD = 'U3lzQGRtMW4='
   );
*/

/*
CREATE ENDPOINT [Hadr_endpoint]
    AS TCP (LISTENER_PORT = 33422)
    FOR DATABASE_MIRRORING (
	    ROLE = ALL,
	    AUTHENTICATION = CERTIFICATE dbm_certificate,
		ENCRYPTION = REQUIRED ALGORITHM AES
		);
ALTER ENDPOINT [Hadr_endpoint] STATE = STARTED;
*/

--DROP ENDPOINT [Hadr_endpoint]

--DROP AVAILABILITY GROUP [ag1]

/*
CREATE AVAILABILITY GROUP [ag1]
     WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
     FOR REPLICA ON
         N'ETXNPAPRDDB01' WITH (
  	       ENDPOINT_URL = N'tcp://ETXNPAPRDDB01:33422',
  	       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
  	       FAILOVER_MODE = EXTERNAL,
  	       SEEDING_MODE = AUTOMATIC
  	       ),
         N'ETXNPAPRDDB02' WITH ( 
  	       ENDPOINT_URL = N'tcp://ETXNPAPRDDB02:33422', 
  	       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
  	       FAILOVER_MODE = EXTERNAL,
  	       SEEDING_MODE = AUTOMATIC
  	       );

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
*/

/*
CREATE LOGIN [pacemakerLogin] with PASSWORD= N'cGFjZW1ha2VyTG9naW4='

ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]

GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::[ag1] TO [pacemakerLogin]
GRANT VIEW SERVER STATE TO [pacemakerLogin]

ALTER AVAILABILITY GROUP [ag1]
ADD LISTENER N'ag1_listener' (
WITH IP
((N'10.148.4.20', N'255.255.255.0')
)
, PORT=33414);
*/
/*

CREATE DATABASE [db1];
ALTER DATABASE [db1] SET RECOVERY FULL;
BACKUP DATABASE [db1] 
   TO DISK = N'/var/opt/mssql/data/db1.bak';


ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1];
*/

-------


/*
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Q2hAbndhbjFjaENTUA==';

CREATE CERTIFICATE dbm_certificate
    FROM FILE = '/var/opt/mssql/data/dbm_certificate.cer'
    WITH PRIVATE KEY (
		FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
		DECRYPTION BY PASSWORD = 'U3lzQGRtMW4='
	);
*/

/*
CREATE ENDPOINT [Hadr_endpoint]
    AS TCP (LISTENER_PORT = 33422)
    FOR DATABASE_MIRRORING (
	    ROLE = ALL,
	    AUTHENTICATION = CERTIFICATE dbm_certificate,
		ENCRYPTION = REQUIRED ALGORITHM AES
		);
ALTER ENDPOINT [Hadr_endpoint] STATE = STARTED;
*/

--DROP ENDPOINT [Hadr_endpoint]

/*
CREATE LOGIN [pacemakerLogin] with PASSWORD= N'cGFjZW1ha2VyTG9naW4='

ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]


ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
*/

SELECT * FROM sys.databases WHERE name = 'db1';
GO
SELECT DB_NAME(database_id) AS 'database', synchronization_state_desc FROM sys.dm_hadr_database_replica_states;
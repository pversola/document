```sql
SELECT
p.partition_number AS PartitionNumber,
f.name AS PartitionFilegroup,
p.rows AS NumberOfRows
FROM sys.partitions p
JOIN sys.destination_data_spaces dds ON p.partition_number = dds.destination_id
JOIN sys.filegroups f ON dds.data_space_id = f.data_space_id
WHERE OBJECT_NAME(OBJECT_ID) = '<OBJECT NAME>'
ORDER BY p.partition_number
```

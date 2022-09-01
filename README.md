
## LOAD FROM CSV TO NEO4J

### APOC LOAD CSV
```
LOAD CSV FROM "file:////data/cusinfo/cusinfo_202105_final.txt" AS row
RETURN row LIMIT 5;

LOAD CSV FROM "file:////data/cusinfo/cusinfo_202105_final.txt" AS row
WITH row LIMIT 100
MERGE (c:Customer {cif: row[0]}) SET c.buc = row[57];
```
### APOC LOAD CSV BATCH
```
CALL apoc.periodic.iterate('
  CALL apoc.load.csv("/data/cusinfo/cusinfo_202105_final.txt") YIELD list AS row RETURN row LIMIT 1000
  ','
  WITH row WHERE row[0] IS NOT NULL
  MERGE (c:Customer {cif: row[0]}) SET c.buc = row[57]
', {batchSize:10000, parallel:true});
```
### LOAD FROM GCS TO NEO4J
```
MERGE (e:Ecosystem {id: cpo}) SET e.name = 'Sawit', e.name_en = 'Palm Oil';

CALL apoc.load.csv('gs://gcs-apac-indo-mandiri-poc2/poc-data/references/cif_sawit.txt?authenticationType=GCP_ENVIRONMENT')
YIELD map as row
MATCH (e:Ecosystem {id: 'cpo'})
MERGE (c:Customer {cif: row.cif})
MERGE (c)-[:IN_ECOSYSTEM]->(e);
```

## LOAD FROM RDBMS

### LOAD INCREMENTAL FROM POSTGRESQL
```
CALL apoc.load.jdbc ("jdbc:postgresql://192.168.1.103/northwind?user=postgres&password=mri123","orders") yield row 
MERGE (p:orders{id:row.orderid}) 
ON CREATE SET
p.shipper_id=toInteger(row.shipperid),
p.customer_id=toInteger(row.customerid),
p.order_date=datetime(row.orderdate),
p.employee_id=toInteger(row.employeeid)
```
### LOAD SNAPSHOT FROM POSTGRESQL
```
CALL apoc.load.jdbc ("jdbc:postgresql://192.168.1.103/northwind?user=postgres&password=mri123","SELECT * FROM orders WHERE orderdate = '1996-12-23'") yield row 
MERGE (p:orders{id:row.orderid}) 
ON CREATE SET
p.shipper_id=toInteger(row.shipperid),
p.customer_id=toInteger(row.customerid),
p.order_date=datetime(row.orderdate),
p.employee_id=toInteger(row.employeeid)
```

## LOAD FROM HIVE IMPALA TO NEO4J

### IMPALA JDBC DEMO 
```
CALL apoc.load.jdbc("jdbc:impala://192.168.1.138:21050/default;AuthMech=0;KrbAuthType=1","SELECT * FROM users;") yield row 
MERGE (p:User{id:row.id}) 
on create SET 
p.gender=row.gender, 
p.username=row.user_name, 
p.lastname=row.last_name, 
p.firstname=row.first_name, 
p.email=row.email, 
p.age=row.age
```

### HIVE JDBC DEMO 
```
CALL apoc.load.jdbc("jdbc:hive2://192.168.1.138:10000/default;","SELECT * FROM users;") yield row
MERGE (p:User{id:row.id}) 
on create SET 
p.gender=row.gender, 
p.username=row.user_name, 
p.lastname=row.last_name, 
p.firstname=row.first_name, 
p.email=row.email, 
p.age=row.age
```



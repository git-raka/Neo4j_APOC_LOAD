# impala-hiveJDBC-neo4j
use plugin hive and impala


Data Integration is an important topic. Reading data from Hive/Impala to create and augment data models is a very helpful exercise,
In Impala “select * from data_impala;”
In hive “select * from data_test;”

Neo4j use apoc.load.jdbc method for integration hive/impala

# Cypher query for JDBC IMPALA : CALL apoc.load.jdbc("jdbc:impala://192.168.1.138:21050/default;AuthMech=0;KrbAuthType=1","SELECT * FROM data_impala;") yield row 
MERGE (p:Person {id:row.id}) set p.nama = row.name

# Cypher query for JDBC Hive: CALL apoc.load.jdbc("jdbc:hive2://192.168.1.138:10000/default;","SELECT * FROM data_test;") yield row 
MERGE (p:Person_2 {id:row.id}) set p.nama = row.name


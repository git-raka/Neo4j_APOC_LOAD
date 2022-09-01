## use plugin hive and impala

# IMPALA JDBC DEMO 
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

# HIVE JDBC DEMO 
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

# ksql-udf-entropy

#### Build
```bash
➜  ~ ./gradlew build
```

#### Deployment
The jar should be copied to the `ext/` directory that is part of the KSQL distribution. The `ext/` directory can be configured via the `ksql.extension.dir`.


#### Verify Function has been loaded 
The jars in the ext/ directory are only scanned at start-up, so you will need to restart your KSQL server instances to pick up new UD(A)Fs.

```bash
ksql> show functions;

 Function Name     | Type      
-------------------------------
 ABS               | SCALAR    
 ARRAYCONTAINS     | SCALAR    
 .
 .
 ENTROPY           | SCALAR    
 .
 .
 TRIM              | SCALAR    
 UCASE             | SCALAR    
 WINDOWEND         | AGGREGATE 
 WINDOWSTART       | AGGREGATE 
-------------------------------
ksql> 

```

#### Use UDF in KSQL
```sql
ksql> select EXTENSION['dhost'] as host, entropy(EXTENSION['dhost']) as entropy from KSTREAM_CEF_AVRO where EXTENSION['dhost'] is not null limit 5;
www.melody4arab.com | 3.576617644908667
gayporngames.net | 3.5
flash201-video.servehttp.com | 4.208966082694623
www.alorobanews.com | 3.366091329119193
vodcdn.ec.own3d.tv | 3.197159723424149
Limit Reached
Query terminated
ksql> 

```
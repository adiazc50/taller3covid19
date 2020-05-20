# Taller 3   ---  COVID-19


Alejandro Díaz Cano

Este github contiene las capturas de pantalla del taller 3 de la materia Topicos de Telematica,
Encontraremos la carpeta datasets, capturas del trabajo y los scrip y/o codigos empleados descritos a continuación.


con el siguiente comando ingresamos a traves del cliente mysql de la Base de dato.

    mysql -u admin -h database-1.cz0t8yw0mjly.us-east-1.rds.amazonaws.com -p
    
con el siguiente comando podriamos cargar las tablas.    
    
    import-all-tables --connect jdbc:mysql://database-2.c1zmlychw6t0.useast-1.rds.amazonaws.com:3306/taller3 --username=admin --     password= St026320201* --hive-database taller3 --hive-overwrite --hive-import --warehouse-dir=/tmp/taller3tmp -m l --mysql-     delimiters


creamos la base de datos

    CREATE DATABASE mydb
  

creamos la tabla confirmedGlobal

    CREATE EXTERNAL TABLE confirmedGlobal(ProvinceState STRING,CountryRegion STRING,Lat STRING,Long STRING,f12220 STRING,f12320     STRING,f12420 STRING,f12520 STRING,f12620 STRING,f12720 STRING,f12820 STRING,f12920 STRING,f13020 STRING,f13120 STRING,f2120     STRING,f2220 STRING,f2320 STRING,f2420 STRING,f2520 STRING,f2620 STRING,f2720 STRING,f2820 STRING,f2920 STRING,f21020           STRING,f21120 STRING,f21220 STRING,f21320 STRING,f21420 STRING,f21520 STRING,f21620 STRING,f21720 STRING,f21820                 STRING,f21920 STRING,f22020 STRING,f22120 STRING,f22220 STRING,f22320 STRING,f22420 STRING,f22520 STRING,f
    22620 STRING,f22720 STRING,f22820 STRING,f22920 STRING,f3120 STRING,f3220 STRING,f3320 STRING,f3420 STRING,f3520                 STRING,f3620 STRING,f3720 STRING,f3820 STRING,f3920 STRING,f31020 STRING,f31120 STRING,f31220 STRING,f31320 STRING,f31420       STRING,f31520 STRING,f31620 STRING,f31720 STRING,f31820 STRING,f31920 STRING,f32020 STRING,f32120 STRING,f32220                 STRING,f32320 STRING,f32420 STRING,f32520 STRING,f32620 STRING,f32720 STRING,f32820 STRING,f32920 STRING,f33020                 STRING,f33120 STRING,f4120 STRING,f4220 STRING,f4320 STRING,f4420 STRING,f4520 STRING,f4620 STRING,f4720 STRING,f4820           STRING,f4920 STRING,f41020 STRING,f41120 STRING,f41220 STRING,f41320 STRING,f41420 STRING,f41520 STRING,f41620 STRING,f41720     STRING,f41820 STRING,f41920 STRING,f42020 STRING,f42120 STRING,f42220 STRING,f42320 STRING,f42420 STRING,f42520                 STRING,f42620 STRING,f42720v STRING,f42820 STRING,f42920 STRING,f43020 STRING,f5120 STRING,f5220 STRING,f5320 STRING,f5420       STRING,f5520 STRING,f5620 STRING,f5720 STRING,f5820 STRING,f5920 STRING,f51020 STRING,f51120 STRING,f51220 STRING,f51320         STRING,f51420 STRING,f51520 STRING,f51620 STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','


con el siguiente Script llenamos la tabla anterior.

     use covid;
     load data inpath 's3a://taller3adiazc/datasets/time_series_covid19_confirmed_global.csv' into table confirmedglobal;

con el siguiente scrip creamos la tabla de datos colombia.

    CREATE EXTERNAL TABLE Colombia(id STRING,fecha STRING,Divipola STRING,Ciudad STRING,Departamento STRING, atencion STRING,       edad STRING, sexo STRING, tipo STRING, estado STRING, procedencia STRING, fis STRING, murio STRING, diagnostico STRING,         recuperado STRING, reporte STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','

con el siguiente Scrip cargamos la tabla colombia.
      
      use covid;
      load data inpath 's3a://taller3adiazc/datasets/data.csv' into table colombia;
      
      
creamos un super usuario y le damos permisos

    CREATE USER 'taller3dba'@'%' IDENTIFIED BY 'taller3dba';

    GRANT ALL PRIVILEGES ON taller3.* to 'taller3dba'@'%';
    
    GRANT SELECT ON taller3.* TO 'taller3db'@'localhost';
    
    
con el siguiente comado parchamos el EMR


    
    hdfs dfs -put /usr/share/java/mysql-connector-java.jar /user/oozie/share/lib/lib_20200518123328/sqoop/
    hdfs dfs -chown oozie /user/oozie/share/lib/lib_20200518123328/sqoop/mysql-connector-java.jar
    hdfs dfs -chgrp oozie /user/oozie/share/lib/lib_20200518123328/sqoop/mysql-connector-java.jar

    hdfs dfs -cp /user/oozie/share/lib/lib_20200416115647/hive/* /user/oozie/share/lib/lib_20200416115647/sqoop/
    hdfs dfs -chown oozie /user/oozie/share/lib/lib_20200518123328/sqoop/*
    hdfs dfs -chgrp oozie /user/oozie/share/lib/lib_20200518123328/sqoop/*

    oozie admin -sharelibupdate
    creamos la tabla confirmadosnarrow
    
    

  
creamos la tabla confirmadosnarrow    
    
    USE covid;
    CREATE EXTERNAL TABLE confirmadosnarrow(ProvinceState STRING,CountryRegion STRING,Lat STRING,Longi STRING,fecha STRING,Value      STRING,ISOCodes STRING,RegionCode STRING,SubregionCode STRING,
    IntermediateRegionCode STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','


con el siguiente scrip caramos la base de datos confirmadosnarrow.

    use mydb;
    load data inpath 's3a://taller3adiazc/datasets/time_series_covid19_confirmed_global_narrow.csv' into table                       confirmadosnarrow;
    
    
    
Para conectar las tablas es necesario descargar el controlador llamado: ODBC driver AmazonHiveODBC





Consultas a traves de Zeppelin donde se crea un notebook
y se gestiona a traves de pyspark


    %pyspark
    dataset = sc.textFile('s3a://taller3adiazc/datasets/*.csv')

    numerodedatos = dataset.count()
    numerodedatos
    
    
    %pyspark
    dataset = sc.textFile('s3a://taller3adiazc/datasets/time_series_covid19_confirmed_global_iso3_regions.csv')
    numerodedatos = dataset.count()
    numerodedatos

    %pyspark
    dataset = sc.textFile('s3a://taller3adiazc/datasets/time_series_covid19_deaths_global.csv')
    numerodedatos = dataset.count()
    numerodedatos


    %pyspark
    dataset = sc.textFile('s3a://taller3adiazc/datasets/time_series_covid19_deaths_global_iso3_regions.csv')
    numerodedatos = dataset.count()
    numerodedatos

    %pyspark
    dataset = sc.textFile('s3a://taller3adiazc/datasets/time_series_covid19_recovered_global.csv')
    numerodedatos = dataset.count()
    numerodedatos.
    
podemos encontrar mas consultas en pyspark en la carpeta de capturas de trabajo.

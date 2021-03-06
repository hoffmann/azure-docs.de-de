<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->

### <a name="basic-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Basic

| Poolgröße (eDTUs)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Max. Speicherkapazität pro Pool* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Max. In-Memory-OLTP-Speicher pro Pool* | N/V | N/V | N/V | N/V | N/V | N/V | N/V | N/V |
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Min. Anz. von eDTUs pro Datenbank | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} |
| Max. Anz. von eDTUs pro Datenbank | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} |
||||||||

### <a name="standard-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Standard

| Poolgröße (eDTUs)  | **50** | **100** | **200** | **300** | **400** | **800** | 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Max. Speicherkapazität pro Pool* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Max. In-Memory-OLTP-Speicher pro Pool* | N/V | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Max. gleichzeitige Worker pro Pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Min. Anz. von eDTUs pro Datenbank | {0,10,20,<br>50} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Max. Anz. von eDTUs pro Datenbank | {10,20,<br>50} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Standard (Fortsetzung) 

| Poolgröße (eDTUs)  |  **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Max. Speicherkapazität pro Pool* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Max. In-Memory-OLTP-Speicher pro Pool* | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker pro Pool |  2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Anmeldungen pro Pool |  2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Max. Anz. von eDTUs pro Datenbank | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Premium

| Poolgröße (eDTUs)  | **125** | **250** | **500** | **1000** | **1500** | 
|:---|---:|---:|---:| ---: | ---: | 
| Max. Speicherkapazität pro Pool* | 250 GB| 500 GB| 750 GB| 750 GB| 750 GB| 
| Max. In-Memory-OLTP-Speicher pro Pool* | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Max. Anzahl Datenbanken pro Pool | 50 | 100 | 100 | 100 | 100 |  
| Max. gleichzeitige Worker pro Pool | 200 | 400 | 800 | 1600 |  2400 | 
| Max. gleichzeitige Anmeldungen pro Pool | 200 | 400 | 800 | 1600 |  2400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} | {0,25,50,75,<br>125,250,500,<br>1000,1500} | 
| Max. Anz. von eDTUs pro Datenbank | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | {25,50,75,<br>125,250,500,<br>1000,1500} |  
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Premium (Fortsetzung) 

| Poolgröße (eDTUs)  |  **2000** | **2500** | **3000** | **3500** | **4000** |
|:---|---:|---:|---:| ---: | ---: | 
| Max. Speicherkapazität pro Pool* | 750 GB | 750 GB | 750 GB | 750 GB | 750 GB |
| Max. Speicherkapazität pro Pool* | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Max. Anzahl Datenbanken pro Pool | 100 | 100 | 100 | 100 | 100 | 
| Max. gleichzeitige Worker pro Pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Anmeldungen pro Pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} |  {0,25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Max. Anz. von eDTUs pro Datenbank | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
||||||||

\* In einem Pool zusammengefasste Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt. Der maximale Speicherplatz pro Pool bezieht sich auf den maximalen Speicher der Datendateien im Pool und beinhaltet nicht den von Protokolldateien belegten Speicherplatz.



<!--HONumber=Jan17_HO2-->



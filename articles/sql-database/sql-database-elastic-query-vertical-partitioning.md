---
title: "Ausführen von Abfragen über Clouddatenbanken mit unterschiedlichen Schemas hinweg | Microsoft Docs"
description: "Einrichten von datenbankübergreifenden Abfragen über vertikale Partitionen"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: multiple databases
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9d3c60eb630816698dc0c0bd3b4be8da5fec13eb


---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Ausführen von Abfragen über Clouddatenbanken mit unterschiedlichen Schemas hinweg (Vorschau)
![Abfrage über Tabellen in unterschiedlichen Datenbanken hinweg][1]

Vertikal partitionierte Datenbanken verwenden verschiedene Sätze von Tabellen in verschiedenen Datenbanken. Das bedeutet, dass das Schema bei verschiedenen Datenbanken unterschiedlich ist. Beispielsweise befinden sich alle bestandsbezogenen Tabellen in einer Datenbank, während alle Buchhaltungstabellen in einer zweiten Datenbank enthalten sind. 

## <a name="prerequisites"></a>Voraussetzungen
* Der Benutzer muss über die Berechtigung ALTER ANY EXTERNAL DATA SOURCE verfügen. Diese Berechtigung ist in der Berechtigung ALTER DATABASE enthalten.
* ALTER ANY EXTERNAL DATA SOURCE-Berechtigungen sind erforderlich, um auf die zu Grunde liegende Datenquelle zu verweisen.

## <a name="overview"></a>Übersicht
**HINWEIS**: Im Gegensatz zur horizontalen Partitionierung hängen diese DDL-Anweisungen nicht von der Festlegung einer Datenebene mit einer Shardzuordnung über die Clientbibliothek für elastische Datenbanken ab.

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx)
3. [CREATE EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Erstellen des Datenbankhauptschlüssels und der Anmeldeinformationen
Die Anmeldeinformationen werden von der elastischen Abfrage für die Verbindung mit Ihren Remotedatenbanken verwendet.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

**Hinweis**: Stellen Sie sicher, dass *<username>* kein *"@servername"*-Suffix enthält. 

## <a name="create-external-data-sources"></a>Erstellen externer Datenquellen
Syntax:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Wichtig**: Der TYPE-Parameter muss auf **RDBMS** festgelegt werden. 

### <a name="example"></a>Beispiel
Das folgende Beispiel veranschaulicht die Verwendung der CREATE-Anweisung für externe Datenquellen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

So rufen Sie die Liste der aktuellen externen Datenquellen ab 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe Tabellen
Syntax:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Beispiel
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Das folgende Beispiel zeigt, wie Sie die Liste der externen Tabellen aus der aktuellen Datenbank abrufen: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Anmerkungen
Die Abfrage für elastische Datenbanken erweitert die vorhandene externe Tabellensyntax zum Definieren von externen Tabellen, die externe Datenquellen vom Typ RDBMS verwenden. Eine externe Tabellendefinition für die vertikale Partitionierung behandelt die folgenden Aspekte: 

* **Schema**: Die DDL für externe Tabellen definiert ein Schema, das Ihre Abfragen verwenden kann. Das in Ihrer Definition für externe Tabellen angegebene Schema muss mit dem Schema der Tabellen in der Remotedatenbank übereinstimmen, in der die eigentlichen Daten gespeichert werden. 
* **Remotedatenbankverweis**: Die DDL für externe Tabellen verweist auf eine externe Datenquelle. Die externe Datenquelle gibt den logischen Servernamen und Datenbanknamen der Remotedatenbank an, in der die eigentlichen Tabellendaten gespeichert sind. 

Wenn Sie eine externe Datenquelle gemäß der Beschreibung im vorherigen Abschnitt verwenden, lautet die Syntax zum Erstellen externer Tabellen wie folgt: 

Die DATA_SOURCE-Klausel definiert die externe Datenquelle (d. h. die Remotedatenbank bei vertikaler Partitionierung), die für die externe Tabelle verwendet wird.  

Die Klauseln SCHEMA_NAME und OBJECT_NAME bieten die Möglichkeit, die Definition der externen Tabelle einer Tabelle in einem anderen Schema in der Remotedatenkbank bzw. einer Tabelle mit einem anderen Namen zuzuordnen. Das ist nützlich, wenn Sie eine externe Tabelle für eine Katalogsicht oder DMV in Ihrer Remotedatenbank definieren möchten oder wenn der Name der Remotetabelle bereits lokal verwendet wird.  

Die folgende DDL-Anweisung löscht eine vorhandene Definition für eine externe Tabelle aus dem lokalen Katalog. Das wirkt sich nicht auf die Remotedatenbank aus. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Berechtigungen für CREATE/DROP EXTERNAL TABLE**: Berechtigungen vom Typ ALTER ANY EXTERNAL DATA SOURCE sind für die DDL für externe Tabellen erforderlich, die auch für den Verweis auf die zugrunde liegende Datenquelle benötigt wird.  

## <a name="security-considerations"></a>Sicherheitshinweise
Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden Remotetabellen gemäß den Anmeldeinformationen, die in der externen Datenquellendefinition angegeben sind. Sie müssen den Zugriff auf die externe Tabelle sorgfältig verwalten, um eine unerwünschte Erhöhung von Berechtigungen über die Anmeldeinformationen für die externe Datenquelle zu vermeiden. Herkömmliche SQL-Berechtigungen können zum Gewähren (GRANT) oder Widerrufen (REVOKE) des Zugriffs auf eine externe Tabelle wie bei einer normalen Tabelle verwendet werden.  

## <a name="example-querying-vertically-partitioned-databases"></a>Beispiel: Abfragen von vertikal partitionierten Datenbanken
Die folgende Abfrage führt eine Dreiwegeverknüpfung zwischen den zwei lokalen Tabellen für Aufträge und Auftragspositionen und der Remotetabelle für Kunden aus. Hier sehen Sie ein Beispiel für den Verweisdatenanwendungsfall für die Abfrage elastischer Datenbanken: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Gespeicherte Prozedur für T-SQL-Remoteausführung: sp\_execute_remote
Mit der elastischen Abfrage wurde auch eine gespeicherte Prozedur eingeführt, die einen Direktzugriff auf die Shards bietet. Die gespeicherte Prozedur heißt [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) und kann verwendet werden, um gespeicherte Remoteprozeduren oder T-SQL-Code in den Remotedatenbanken auszuführen. Hierfür werden die folgenden Parameter verwendet: 

* Datenquellenname (nvarchar): Der Name der externen Datenquelle vom Typ RDBMS. 
* Abfrage (nvarchar): Die T-SQL-Abfrage, die für die einzelnen Shards ausgeführt wird. 
* Parameterdeklaration (nvarchar) – optional: Zeichenfolge mit Datentypdefinitionen für die Parameter, die im „Query“-Parameter verwendet werden (z. B. sp_executesql). 
* Parameterwertliste – optional: Durch Trennzeichen getrennte Liste von Parameterwerten (z.B. sp_executesql).

Die Prozedur „sp\_execute\_remote“ verwendet die externe Datenquelle, die in den Aufrufparametern angegeben ist, um die jeweilige T-SQL-Anweisung in den Remotedatenbanken auszuführen. Die Anmeldeinformationen der externen Datenquelle werden verwendet, um die Verbindung mit der ShardMapManager-Datenbank und den Remotedatenbanken herzustellen.  

Beispiel: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a>Konnektivität für Tools
Sie können herkömmliche SQL Server-Verbindungszeichenfolgen verwenden, um Ihre BI- und Datenintegrationstools mit Datenbanken auf dem SQL-Datenbankserver zu verbinden, für die die Abfrage für elastische Datenbanken aktiviert ist und für die externe Tabellen definiert sind. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Verweisen Sie dann auf die elastische Abfragedatenbank und die externen Tabellen wie auf jede andere SQL Server-Datenbank, zu der Sie mit dem Tool eine Verbindung herstellen würden. 

## <a name="best-practices"></a>Bewährte Methoden
* Stellen Sie sicher, dass die Endpunktdatenbank für elastische Abfragen Zugriff auf die Remotedatenbank hat, indem Sie den Zugriff für Azure-Dienste in der Firewallkonfiguration für SQL-Datenbanken aktivieren. Stellen Sie darüber hinaus sicher, dass mit den in der externen Datenquellendefinition angegebenen Anmeldeinformationen die Anmeldung bei der Remotedatenbank möglich ist und für die Anmeldeinformationen Berechtigungen zum Zugriff auf die Remotetabelle festgelegt wurden.  
* Eine Abfrage für elastische Datenbanken funktioniert am besten für Abfragen, in denen der größte Teil der Berechnung in den Remotedatenbanken erfolgt. In der Regel erhalten Sie optimale Abfrageleistung mit benutzerdefinierten Filterprädikaten, die in den Remotedatenbanken oder Verknüpfungen ausgewertet werden können, die vollständig in der Remotedatenbank ausgeführt werden können. Für andere Abfragemuster müssen möglicherweise große Mengen von Daten aus der Remotedatenbank geladen werden, wodurch Leistungseinbußen auftreten. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Abfragen horizontal partitionierter Datenbanken (auch Shartdatenbanken genannt) finden Sie unter [Ausführen von Abfragen über horizontal partitionierte Clouddatenbanken (Sharddatenbanken) hinweg](sql-database-elastic-query-horizontal-partitioning.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->



<!--HONumber=Dec16_HO2-->



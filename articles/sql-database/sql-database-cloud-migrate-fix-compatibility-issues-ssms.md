---
title: "Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank mit SQL Server Management Studio vor der Migration zu SQL-Datenbank | Microsoft Docs"
description: "Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5f7d3544-b07e-415a-a2ae-96e49bf5d756
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 355353fb15a00860573699cc652543b61c62c2c1


---
# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank mit SQL Server Management Studio vor der Migration zu einer SQL-Datenbank
> [!div class="op_single_selector"]
> * Mit dem [SQL Azure-Migrations-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
> * Mit [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * Mit [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> 
> 

Fortgeschrittene Benutzer können Kompatibilitätsprobleme von SQL Server-Datenbank mithilfe von SQL Server Management Studio vor der Migration zu Azure SQL-Datenbank beheben.

> [!IMPORTANT]
> Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="using-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio
Verwenden Sie SQL Server Management Studio zum Beheben von Kompatibilitätsproblemen mithilfe von verschiedenen Transact-SQL-Befehlen, z.B. **ALTER DATABASE**. Diese Methode richtet sich primär an fortgeschrittene Benutzer, die mit der Arbeit mit Transact-SQL in einer Livedatenbank vertraut sind. Andernfalls empfiehlt sich die Verwendung von SSDT. 

## <a name="next-steps"></a>Nächste Schritte
* [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Migrieren einer kompatiblen SQL Server-Datenbank zur SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md)
* [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->



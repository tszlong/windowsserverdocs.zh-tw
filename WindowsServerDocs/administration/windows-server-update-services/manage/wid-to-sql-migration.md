---
title: 移轉 WSUS 資料庫 （Windows 內部資料庫） 從 WID to SQL
description: Windows Server Update Service (WSUS) 主題為如何將 WSUS 資料庫 (SUSDB) 從 Windows 內部資料庫執行個體移轉到 SQL Server 的本機或遠端執行個體。
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: ed6f695947fc17d2e96b5282b3a67a221bb0140d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858029"
---
>適用於：Windows Server 2012 中，Windows Server 2012 R2 中，Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>將 WSUS 資料庫從 WID 移轉到 SQL

使用下列步驟來移轉 WSUS 資料庫 (SUSDB) 從 Windows 內部資料庫執行個體至 SQL Server 的本機或遠端執行個體。

## <a name="prerequisites"></a>先決條件

- SQL 執行個體。 這可以是預設值**MSSQLServer**或自訂的執行個體。
- SQL Server Management Studio
- WSUS 安裝 WID 角色
- （這是通常包含當您安裝 WSUS 透過 伺服器管理員） 的 IIS。 尚未安裝，它必須是。

## <a name="migrating-the-wsus-database"></a>移轉 WSUS 資料庫

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>將 WSUS 伺服器上停止的 IIS 和 WSUS 服務

從 PowerShell （提高權限），請執行：

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>卸離 SUSDB 從 Windows 內部資料庫

#### <a name="using-sql-management-studio"></a>使用 SQL Management Studio

1. 以滑鼠右鍵按一下**SUSDB** - &gt; **工作** - &gt;按一下**卸離**: ![image1](images/image1.png)
2. 請檢查**卸除現有的連接**然後按一下**確定**（選擇性，如果作用中連線存在）。
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>使用 [命令提示字元]

> [!IMPORTANT]
> 這些步驟將示範如何卸離 WSUS 資料庫 (SUSDB) 從 Windows 內部資料庫執行個體使用**sqlcmd**公用程式。 如需詳細資訊**sqlcmd**公用程式，請參閱[sqlcmd 公用程式](https://go.microsoft.com/fwlink/?LinkId=81183)。
1. 開啟提升權限的命令提示字元
2. 使用 Windows Internal Database 執行個體從執行下列 SQL 命令以卸離 WSUS 資料庫 (SUSDB) **sqlcmd**公用程式：

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>SUSDB 檔案複製到 SQL Server

1. 複製**SUSDB.mdf**並**SUSDB\_log.ldf**從 WID 資料資料夾 (**%systemdrive%**\** Windows\WID\Data * *) 到 SQL 執行個體資料資料夾。

> [!TIP]
> 例如，如果您的 SQL 執行個體資料夾是**C:\Program Files\Microsoft SQL Server\MSSQL12。MSSQLSERVER\MSSQL**，而且 WID 資料資料夾**C:\Windows\WID\Data，** SUSDB 檔案複製來源**C:\Windows\WID\Data**至**C:\Program Files\Microsoft SQL Server\MSSQL12.<INSTANCENAME>\。MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>附加 SUSDB SQL 執行個體

1. 在**SQL Server Management Studio**下方**執行個體**節點，以滑鼠右鍵按一下**資料庫**，然後按一下**附加**。
    ![image3](images/image3.png)
2. 在 **附加資料庫**的 **要附加的資料庫**，按一下 **新增**按鈕，然後找出**SUSDB.mdf**檔案 (從複製WID 資料夾），然後按一下**確定**。
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> 這也是能夠完成使用 Transact Sql。  請參閱[可附加資料庫的 SQL 文件](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database)如其所需的指示。
>
> 範例 （使用上一個範例中的路徑）：
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>確認 SQL Server 和資料庫登入和權限

#### <a name="sql-server-login-permissions"></a>SQL Server 登入權限

附加 SUSDB 之後，確認**NT AUTHORITY\NETWORK SERVICE**具有 SQL Server 執行個體的登入權限執行下列動作：

1. 移到 SQL Server Management Studio
2. 開啟執行個體
3. 按一下 **安全性**
4. 按一下 **登入**

**NT AUTHORITY\NETWORK SERVICE**應該會列出帳戶。 如果不是，您需要將它新增藉由新增新的登入名稱。

> [!IMPORTANT]
> 如果 SQL 執行個體是在不同電腦上從 WSUS，WSUS 伺服器的電腦帳戶中的格式應該會列出 **[FQDN]\\[WSUSComputerName] $**。  如果沒有，請執行下列步驟可用來將它加入，取代**NT AUTHORITY\NETWORK SERVICE**與 WSUS 伺服器的電腦帳戶 (**[FQDN]\\[WSUSComputerName] $**) 這會是***除了***權限授與**NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>新增 NT AUTHORITY\NETWORK SERVICE，並授與其權限

1. 以滑鼠右鍵按一下**登入**，按一下 **新登入...**
    ![image6](images/image6.png)
2. 上**一般**頁面上，填妥**登入名稱**(**NT AUTHORITY\NETWORK SERVICE**)，並將**預設資料庫**SUSDB 到。
    ![image7](images/image7.png)
3. 在上**伺服器角色**頁面上，確定**公用**並**sysadmin**已選取。
    ![image8](images/image8.png)
4. 在 **使用者對應**頁面：
    - 底下**對應到此登入的使用者**： 選取**SUSDB**
    - 在下**資料庫角色成員資格對象：SUSDB**，請確定已選取下列：
        - **public**
        - **webService** ![image9](images/image9.png)
5. 按一下 **[確定]**。

您現在應該會看到**NT AUTHORITY\NETWORK SERVICE**登入 下。
![image10](images/image10.png)

#### <a name="database-permissions"></a>資料庫權限

1. 以滑鼠右鍵按一下 SUSDB
2. 選取**屬性**
3. 按一下 **權限**

**NT AUTHORITY\NETWORK SERVICE**應該會列出帳戶。

1. 如果不是，新增的帳戶。
2. 在 [登入名稱] 文字方塊中，輸入 WSUS 電腦以下列格式：
    > [**FQDN]\\[WSUSComputerName]$**
3. 確認**預設資料庫**設為**SUSDB**。

    > [!TIP]
    > 在下列範例中，FQDN 是**Contosto.com** WSUS 電腦名稱，而且**WsusMachine**:
    >
    > ![image11](images/image11.png)

4. 在上**使用者對應**頁面上，選取**SUSDB**資料庫底下 **」 對應到此登入的使用者 」**
5. 檢查**webservice**下方 **"資料庫角色成員資格對象：SUSDB 」**: ![image12](images/image12.png)
6. 按一下 **確定**儲存設定。
    > [!NOTE]
    > 您可能需要重新啟動 SQL 服務，變更才會生效。

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>編輯登錄以 SQL Server 執行個體的點 WSUS

> [!IMPORTANT]
> 請仔細依照本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 您可以修改它，才能[備份進行還原登錄](https://support.microsoft.com/en-us/help/322756)在發生問題。

1. 依序按一下 [開始] 和 [執行]、輸入 **regedit**，然後按一下 [確定]。
2. 尋找下列機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. 在 **值**文字方塊中，輸入 **[伺服器名稱]\\[執行個體名稱]**，然後按一下 **[確定]**。 如果執行個體名稱的預設執行個體，請輸入 **[ServerName]**。
4. 尋找下列機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. 重新命名的索引鍵**更新服務資料庫** ![image41](images/image14.png)

    > [!NOTE]
    > 如果您未更新此金鑰，然後**WsusUtil**會嘗試將服務將 WID，而不是您已移轉的 SQL 執行個體。

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>WSUS 伺服器上啟動 IIS 及 WSUS 服務

從 PowerShell （提高權限），請執行：

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> 如果您使用 WSUS 主控台，請關閉並重新啟動它。

## <a name="uninstalling-the-wid-role-not-recommended"></a>正在解除安裝 （不建議） WID 角色

> [!WARNING]
> 移除 WID 角色也會移除資料庫資料夾 (**%SystemDrive%\Program Files\Update Services\Database**)，其中包含 WSUSUtil.exe 所需的安裝後續工作的指令碼。 如果您選擇將 WID 角色解除安裝，請確定您備份 **%SystemDrive%\Program Files\Update Services\Database**事先資料夾。

使用 PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

移除 WID 角色之後，請確認下列登錄機碼已存在：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-Database**
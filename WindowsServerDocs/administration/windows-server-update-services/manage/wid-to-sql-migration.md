---
title: 將 WSUS 資料庫從（Windows 內部資料庫） WID 遷移至 SQL
description: Windows Server Update Service （WSUS）主題-如何將 WSUS 資料庫（SUSDB）從 Windows 內部資料庫實例遷移到 SQL Server 的本機或遠端實例。
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 7238d71ed35b5c9645e6b4061717011cc2a02820
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203568"
---
# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>將 WSUS 資料庫從 WID 遷移至 SQL

> 適用于： Windows Server 2012、Windows Server 2012 R2、Windows Server 2016

使用下列步驟，將 WSUS 資料庫（SUSDB）從 Windows 內部資料庫實例遷移到 SQL Server 的本機或遠端實例。

## <a name="prerequisites"></a>先決條件

- SQL 實例。 這可以是預設的**MSSQLServer**或自訂的實例。
- SQL Server Management Studio
- 已安裝 WID 角色的 WSUS
- IIS （當您透過伺服器管理員安裝 WSUS 時，通常會包含這種情況）。 它尚未安裝，必須是。

## <a name="migrating-the-wsus-database"></a>遷移 WSUS 資料庫

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>停止 WSUS 伺服器上的 IIS 和 WSUS 服務

從 PowerShell （提高許可權），執行：

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>從 Windows 內部資料庫卸離 SUSDB

#### <a name="using-sql-management-studio"></a>使用 SQL Management Studio

1. 以滑鼠右鍵按一下 [ **SUSDB**工作]，按一下 [卸 - &gt; **Tasks** - &gt; **離**]： ![ image1](images/image1.png)
2. 核取 [卸載**現有的連接**]，然後按一下 **[確定]** （如果有作用中連線存在，則為選擇性
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>使用 [命令提示字元]

> [!IMPORTANT]
> 這些步驟說明如何使用**sqlcmd**公用程式，從 Windows 內部資料庫實例卸離 WSUS 資料庫（SUSDB）。 如需**sqlcmd**公用程式的詳細資訊，請參閱[sqlcmd utility](https://go.microsoft.com/fwlink/?LinkId=81183)。
> 1. 開啟提升許可權的命令提示字元
> 2. 執行下列 SQL 命令，使用**sqlcmd**公用程式從 Windows 內部資料庫實例卸離 WSUS 資料庫（SUSDB）：

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>將 SUSDB 檔案複製到 SQL Server

1. **從 WID \_ ** data 資料夾（**% SystemDrive%** **SUSDB.mdf** \* * WINDOWS\WID\DATA * *）將 SUSDB 複製到 SQL 實例資料檔案夾。

> [!TIP]
> 例如，如果您的 SQL 實例資料夾是**C:\Program FILES\MICROSOFT sql Server\MSSQL12。MSSQLSERVER\MSSQL**，並將 WID Data 資料夾**C:\Windows\WID\Data，** 將 SUSDB 檔案從**C:\Windows\WID\Data**複製到**C:\Program Files\Microsoft SQL Server\MSSQL12。MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>將 SUSDB 附加至 SQL 實例

1. 在**SQL Server Management Studio**的**實例**節點底下，以滑鼠右鍵按一下 [**資料庫**]，然後按一下 [**附加**]。
    ![image3](images/image3.png)
2. 在 [**附加資料庫**] 方塊的 [**要附加的資料庫**] 底下，按一下 [**新增**] 按鈕，然後找出**SUSDB** （從 WID 資料夾複製），再按一下 **[確定]**。
    ![image4.jpg ](images/image4.png) ![ image5](images/image5.png)

> [!TIP]
> 這也可以使用 Transact-sql 來完成。  請參閱[SQL 檔，以瞭解如何附加資料庫](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database)以取得其指示。
>
> 範例（使用上一個範例中的路徑）：
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

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>確認 SQL Server 和資料庫登入和許可權

#### <a name="sql-server-login-permissions"></a>SQL Server 登入許可權

附加 SUSDB 之後，請執行下列動作，以確認**NT AUTHORITY\NETWORK SERVICE**具有 SQL Server 實例的登入許可權：

1. 進入 SQL Server Management Studio
2. 開啟實例
3. 按一下 [**安全性**]
4. 按一下 **[** 登入]

應該會列出**NT AUTHORITY\NETWORK 服務**帳戶。 如果不是，您需要加入新的登入名稱來新增它。

> [!IMPORTANT]
> 如果 SQL 實例與 WSUS 位於不同的電腦上，WSUS 伺服器的電腦帳戶應會以 **[FQDN] \\ [WSUSComputerName] $** 格式列出。  如果沒有，您可以使用下列步驟來新增它，將**nt AUTHORITY\NETWORK 服務**取代為 WSUS 伺服器的電腦帳戶（**[FQDN] \\ [WSUSComputerName] $**），這是除了將許可權授與**NT AUTHORITY\NETWORK SERVICE** ***之外***

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>新增 NT AUTHORITY\NETWORK 服務並授與 it 權利

1. 以滑鼠右鍵按一下 [登入 **]，然後按一下** **[新增登**入 ...]
    ![image6](images/image6.png)
2. 在 [**一般**] 頁面上，填入**登入名稱**（**NT AUTHORITY\NETWORK SERVICE**），並將**預設資料庫**設定為 SUSDB。
    ![image7](images/image7.png)
3. 在 [**伺服器角色**] 頁面上，確定已選取 [**公用**] 和 [**系統管理員**]。
    ![image8](images/image8.png)
4. 在 [**使用者對應**] 頁面上：
    - 在 **[對應到此登入的使用者**] 下：選取 [ **SUSDB** ]
    - 在 [**資料庫角色成員資格： SUSDB**] 底下，確定已核取下列各項：
        - **公立**
        - **webService** ![image9](images/image9.png)
5. 按一下 [檔案] &gt; [新增] &gt; [專案] 

您現在應該會在 [登入] 下看到**NT AUTHORITY\NETWORK SERVICE** 。
![image10](images/image10.png)

#### <a name="database-permissions"></a>資料庫權限

1. 以滑鼠右鍵按一下 SUSDB
2. 選取**屬性**
3. 按一下 [**許可權**]

應該會列出**NT AUTHORITY\NETWORK 服務**帳戶。

1. 如果不是，請新增帳戶。
2. 在 [登入名稱] 文字方塊中，以下列格式輸入 WSUS 機器：
    > [**FQDN] \\[WSUSComputerName] $**
3. 確認 [**預設資料庫**] 已設定為 [ **SUSDB**]。

    > [!TIP]
    > 在下列範例中，FQDN 是**Contosto.com** ，而 WSUS 機器名稱是**WsusMachine**：
    >
    > ![image11](images/image11.png)

4. 在 [**使用者對應**] 頁面上，選取 [**對應到此登**入的使用者] 底下的 [ **SUSDB** ] 資料庫
5. 檢查**資料庫角色成員資格**下的**webservice** ： SUSDB： ![ image12](images/image12.png)
6. 按一下 **[確定]** 以儲存設定。
    > [!NOTE]
    > 您可能需要重新開機 SQL 服務，變更才會生效。

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>編輯登錄以將 WSUS 指向 SQL Server 實例

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

1. 依序按一下 [開始]  、[執行]  ，輸入 **regedit&** ，然後按一下 [確定]  。
2. 找出下列機碼： **HKEY_LOCAL_MACHINE \software\microsoft\updateservices\server\setup\sqlservername**
3. 在 [**值**] 文字方塊中，輸入 **[ServerName] \\ [InstanceName]**，然後按一下 [**確定**]。 如果實例名稱是預設實例，請輸入 **[ServerName]**。
4. 找出下列機碼： **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![ image13](images/image13.png)
5. 將金鑰重新命名為**UpdateServices-資料庫** ![ image41](images/image14.png)

    > [!NOTE]
    > 如果您未更新此金鑰，則**WsusUtil**會嘗試服務 WID，而不是您已遷移的 SQL 實例。

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>在 WSUS 伺服器上啟動 IIS 和 WSUS 服務

從 PowerShell （提高許可權），執行：

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> 如果您使用 WSUS 主控台，請將它關閉並重新啟動。

## <a name="uninstalling-the-wid-role-not-recommended"></a>卸載 WID 角色（不建議）

> [!WARNING]
> 移除 WID 角色也會移除資料庫檔案夾（**%SystemDrive%\Program Files\Update Services\Database**），其中包含 WSUSUtil 針對後續安裝工作所需的腳本。 如果您選擇卸載 WID 角色，請務必事先備份 **%SystemDrive%\Program Files\Update Services\Database**資料夾。

使用 PowerShell：

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

移除 WID 角色之後，請確認下列登錄機碼是否存在： **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed role Services\UpdateServices-Database**
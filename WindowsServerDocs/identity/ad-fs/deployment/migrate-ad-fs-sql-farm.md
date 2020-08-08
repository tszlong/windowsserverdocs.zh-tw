---
title: 遷移 AD FS 2.0 同盟伺服器 SQL 伺服器陣列
description: 提供將 AD FS 2.0 伺服器 SQL 伺服器陣列遷移至 Windows Server 2012 的相關資訊
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: f63a797a30d363b8662dfcc6bfcac46f79dc3eb0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938197"
---
# <a name="migrate-an-ad-fs-20-sql-farm"></a>遷移 AD FS 2.0 SQL 伺服器陣列
本檔提供將 AD FS 2.0 SQL 伺服器陣列遷移至 Windows Server 2012 的詳細資訊。


## <a name="migrate-a-sql-server-farm"></a>移轉 SQL Server 伺服器陣列
 若要將 SQL Server 服務器陣列遷移至 Windows Server 2012，請執行下列程式：

1.  針對 SQL Server 服務器陣列中的每部伺服器，請檢查並執行[遷移 SQL Server 服務器](prepare-to-migrate-a-sql-server-farm.md)陣列中的程式。

2.  從負載平衡器移除 SQL Server 伺服器陣列中的任何伺服器。

3.  將 SQL Server 服務器陣列中此伺服器上的作業系統從 Windows Server 2008 R2 或 Windows Server 2008 升級至 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11))。

> [!IMPORTANT]
>  作業系統升級會造成此伺服器上的 AD FS 設定遺失且 AD FS 2.0 伺服器角色會被移除。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須手動建立原始 AD FS 設定並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。

4. 使用 AD FS Windows PowerShell Cmdlet，在 SQL Server 伺服器陣列中這個伺服器上建立原始 AD FS 設定，將伺服器新增到現有的伺服器陣列。

> [!IMPORTANT]
>  如果您使用 SQL Server 儲存 AD FS 設定資料庫，則必須使用 Windows PowerShell 來建立原始 AD FS 設定。

  - 開啟 Windows PowerShell 並執行下列命令： `$fscredential = Get-Credential` 。
  - 輸入準備移轉 SQL Server 伺服器陣列時所記錄之服務帳戶的名稱和密碼。
  - 執行下列命令： `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` ，其中 `Data Source` 是下列檔案的原則存放區連接字串值中的資料來源值： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 。

5. 將您剛升級至 Windows Server 2012 的伺服器新增至負載平衡器。

6. 針對 SQL Server 伺服器陣列中的其餘節點重複步驟 2 到 6。

7. 當您 SQL Server 服務器陣列中的所有伺服器都升級至 Windows Server 2012 時，請還原任何剩餘的 AD FS 自訂專案，例如自訂屬性存放區。

## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)[準備遷移 AD FS 2.0 同盟伺服器 proxy](prepare-to-migrate-ad-fs-fed-proxy.md) [遷移 AD FS 2.0 同盟](migrate-the-ad-fs-fed-server.md)伺服器遷移[AD FS 2.0 同盟伺服器 proxy](migrate-the-ad-fs-2-fed-server-proxy.md) [遷移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)程式

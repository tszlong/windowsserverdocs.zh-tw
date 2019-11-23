---
title: 遷移 AD FS 2.0 同盟伺服器 SQL 伺服器陣列
description: 提供將 AD FS 2.0 伺服器 SQL 伺服器陣列遷移至 Windows Server 2012 的相關資訊
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3c43d26868f39896ec8632397dc0fce1dfe2dd2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408287"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>遷移 AD FS 2.0 WID 伺服器陣列  
本檔提供將 AD FS 2.0 SQL 伺服器陣列遷移至 Windows Server 2012 的詳細資訊。


## <a name="migrate-a-sql-server-farm"></a>移轉 SQL Server 伺服器陣列  
 若要將 SQL Server 服務器陣列遷移至 Windows Server 2012，請執行下列程式：  
  
1.  針對 SQL Server 服務器陣列中的每部伺服器，請檢查並執行[遷移 SQL Server 服務器](prepare-to-migrate-a-sql-server-farm.md)陣列中的程式。  
  
2.  從負載平衡器移除 SQL Server 伺服器陣列中的任何伺服器。  
  
3.  將 SQL Server 服務器陣列中此伺服器上的作業系統從 Windows Server 2008 R2 或 Windows Server 2008 升級至 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果會遺失這部伺服器上的 AD FS 設定，並移除 AD FS 2.0 伺服器角色。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須手動建立原始的 AD FS 設定，並還原剩餘的 AD FS 設定，以完成同盟伺服器遷移。  
  
4. 使用 AD FS Windows PowerShell Cmdlet 將伺服器新增至現有的伺服陣列，以在 SQL Server 服務器陣列中的這部伺服器上建立原始的 AD FS 設定。  
  
> [!IMPORTANT]
>  如果您使用 SQL Server 來儲存 AD FS 設定資料庫，您必須使用 Windows PowerShell 來建立原始的 AD FS 設定。  

  - 開啟 Windows PowerShell 並執行下列命令： `$fscredential = Get-Credential`。  
  - 輸入準備移轉 SQL Server 伺服器陣列時所記錄之服務帳戶的名稱和密碼。  
  - 執行下列命令： `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`，其中 `Data Source` 是下列檔案的原則存放區連接字串值中的資料來源值： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
5. 將您剛升級至 Windows Server 2012 的伺服器新增至負載平衡器。  
  
6. 針對 SQL Server 服務器陣列中的其餘節點，重複步驟2到6。  
  
7. 當您 SQL Server 服務器陣列中的所有伺服器都升級至 Windows Server 2012 時，請還原任何剩餘的 AD FS 自訂專案，例如自訂屬性存放區。  

## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備遷移 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [遷移 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [遷移 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)




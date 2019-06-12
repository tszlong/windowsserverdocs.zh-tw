---
title: 移轉 AD FS 2.0 同盟伺服器 SQL 陣列
description: 提供有關移轉到 Windows Server 2012 的 AD FS 2.0 伺服器的 SQL 伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afecbf80d78688b66b2392647ea3195be95e9f54
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445611"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>移轉 AD FS 2.0 WID 伺服器陣列  
本文件提供有關移轉到 Windows Server 2012 的 AD FS 2.0 SQL 伺服器陣列的詳細的資訊。


## <a name="migrate-a-sql-server-farm"></a>移轉 SQL Server 伺服器陣列  
 若要將 SQL Server 伺服器陣列移轉至 Windows Server 2012 中，執行下列程序：  
  
1.  針對 SQL Server 伺服器陣列中每部伺服器，請檢閱，並執行中的程序[移轉 SQL Server 伺服器陣列](prepare-to-migrate-a-sql-server-farm.md)。  
  
2.  從負載平衡器移除 SQL Server 伺服器陣列中的任何伺服器。  
  
3.  從 Windows Server 2008 R2 或 Windows Server 2008 的 SQL Server 伺服器陣列中這個伺服器上的作業系統升級到 Windows Server 2012 中。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定會遺失且 AD FS 2.0 伺服器角色會被移除。 相反地，安裝 Windows Server 2012 的 AD FS 伺服器角色，但尚未進行設定。 您必須手動建立原始 AD FS 設定，並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。  
  
4. 使用 AD FS Windows PowerShell cmdlet，將伺服器新增至現有的陣列，您的 SQL Server 伺服器陣列中這個伺服器上建立原始 AD FS 設定。  
  
> [!IMPORTANT]
>  您必須使用 Windows PowerShell 來建立原始 AD FS 設定，如果您使用 SQL Server 來儲存您的 AD FS 設定資料庫。  

  - 開啟 Windows PowerShell 並執行下列命令： `$fscredential = Get-Credential`。  
  - 輸入準備移轉 SQL Server 伺服器陣列時所記錄之服務帳戶的名稱和密碼。  
  - 執行下列命令： `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`，其中`Data Source`是在原則存放區連接字串值，在下列檔案中的資料來源值： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
5. 新增您剛才升級到 Windows Server 2012 的負載平衡器的伺服器。  
  
6. SQL Server 伺服器陣列中的其餘節點重複步驟 2 到 6。  
  
7. 當所有 SQL Server 伺服器陣列中的伺服器升級到 Windows Server 2012 時，還原任何剩餘的 AD FS 自訂，例如自訂屬性存放區。  

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)




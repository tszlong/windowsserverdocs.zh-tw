---
title: "移轉 AD FS 2.0 聯盟伺服器 SQL 陣列"
description: "AD FS 2.0 伺服器 SQL 發電廠移轉到 Windows Server 2012 上提供的資訊"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>移轉 AD FS 2.0 WID 陣列  
本文件會提供 AD FS 2.0 SQL 發電廠移轉到 Windows Server 2012 上的詳細的資訊。


## <a name="migrate-a-sql-server-farm"></a>移轉 SQL Server 陣列  
 若要將 SQL Server 發電廠移轉到 Windows Server 2012，執行下列程序：  
  
1.  每個伺服器陣列 SQL Server 中，檢視，並執行中的程序[移轉 SQL Server 發電廠](prepare-to-migrate-a-sql-server-farm.md)。  
  
2.  在您 SQL Server 發電廠移除負載平衡器的任何伺服器。  
  
3.  Windows Server 2008 R2 或 Windows Server 2008 的陣列 SQL Server 中此伺服器上的作業系統升級到 Windows Server 2012 中。 如需詳細資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定將會遺失，並且移除 AD FS 2.0 伺服器角色。 Windows Server 2012 AD FS 伺服器角色已安裝改為，但未設定。 您必須手動建立原始設定，AD FS，並還原剩餘 AD FS 設定完成聯盟伺服器移轉。  
  
4.  使用 AD FS Windows PowerShell cmdlet 來新增至現有陣列伺服器陣列 SQL Server 中此伺服器上建立原始設定，AD FS。  
  
> [!IMPORTANT]
>  您必須使用 Windows PowerShell 來建立原始 AD FS 設定，如果您正在使用 SQL Server 市集 AD FS 設定資料庫。  

  - 打開 Windows PowerShell 並執行下列命令：`$fscredential = Get-Credential`。  
  - 輸入名稱及服務帳號錄製時您 SQL Server 發電廠準備移轉的密碼。  
  - 執行下列命令：`Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`、在`Data Source`是原則市集連接字串值，在下列檔案中的資料來源值：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
5.  新增您只是升級到 Windows Server 2012 負載平衡器伺服器。  
  
6.  重複步驟 2 到 6 剩餘節點陣列 SQL Server 中。  
  
7.  當您 SQL Server 伺服器的所有的升級到 Windows Server 2012、還原任何剩餘 AD FS 的自訂項目，例如自訂屬性存放區。  

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)




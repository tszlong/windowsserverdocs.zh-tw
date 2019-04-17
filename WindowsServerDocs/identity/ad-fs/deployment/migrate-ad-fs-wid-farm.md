---
title: "移轉 AD FS 2.0 聯盟伺服器 WID 陣列"
description: "AD FS 2.0 伺服器 WID 發電廠移轉到 Windows Server 2012 上提供的資訊"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>移轉 AD FS 2.0 WID 陣列  
本文件會提供詳細的資訊移轉 AD FS 2.0 Windows 內部資料庫 (WID) 發電廠到 Windows Server 2012。

## <a name="migrate-an-ad-fs-wid-farm"></a>移轉 AD FS WID 陣列
若要將 WID 發電廠移轉到 Windows Server 2012，執行下列程序：  
  
1.  針對每個節點（伺服器）WID 陣列中，檢視，並執行中的程序[準備移轉 WID 發電廠](prepare-to-migrate-a-wid-farm.md)。  
  
2.  移除負載平衡器任何非主要節點。  
  
3.  升級此伺服器從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 的作業系統。 如需詳細資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定將會遺失，並且移除 AD FS 2.0 伺服器角色。 Windows Server 2012 AD FS 伺服器角色已安裝改為，但未設定。 您必須建立原始 AD FS 設定，並還原剩餘 AD FS 設定完成聯盟伺服器移轉。  
  
4.  在此伺服器上建立的原始 AD FS 設定。  
  
您可以建立原始設定，AD FS 使用**AD FS 聯盟伺服器設定精靈**來新增至 WID 陣列聯盟伺服器。 如需詳細資訊，請查看[新增聯盟伺服器聯盟伺服器陣列到](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達**指定主要聯盟伺服器及服務 Account**頁面中**AD FS 聯盟伺服器設定精靈**、輸入 WID 陣列的主要聯盟伺服器的名稱，請務必輸入您錄製準備 AD FS 移轉作業時的服務 account 資訊。 如需詳細資訊，請查看[準備 2.0 聯盟伺服器移轉 AD FS](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達**同盟服務名稱指定**頁面上，請務必選取相同 SSL 憑證您記錄在 [準備移轉 WID 發電廠」[準備 2.0 聯盟伺服器移轉 AD FS](prepare-to-migrate-a-wid-farm.md)。  
  
5.  更新您的 AD FS 網頁此伺服器上。 如果您的移轉準備時備份您自訂 AD FS 網頁，您需要使用覆寫預設 AD FS 網頁中的預設所建立的備份資料**%systemdrive%\inetpub\adfs\ls**目錄根據 AD FS 設定 Windows Server 2012 上。  
  
6.  新增您只是升級到 Windows Server 2012 負載平衡器伺服器。  
  
7.  重複執行步驟 1 到 6 WID 發電廠您在其他次要伺服器。  
  
8.  促銷其中一個做為主要伺服器 WID 陣列中的升級次要伺服器。 若要這樣做，請打開 Windows PowerShell 並執行下列命令：`PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 移除負載平衡器原始的主要您 WID 發電廠的伺服器。  
  
10. 降級原始 WID 陣列次要伺服器是使用 Windows PowerShell 中的主要伺服器。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將會次要伺服器原始主要伺服器降級：`PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. 從 Windows Server 2008 R2 或 Windows Server 2008 WID 陣列到 Windows Server 2012 中升級的作業系統上此最後一個節點（伺服器）。 如需詳細資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  升級作業系統的結果，在此伺服器上的 AD FS 設定將會遺失，並且移除 AD FS 2.0 伺服器角色。 Windows Server 2012 AD FS 伺服器角色已安裝改為，但未設定。 您必須手動建立原始設定，AD FS，並還原剩餘 AD FS 設定完成聯盟伺服器移轉。  
  
12. 建立您的 WID 陣列此最後一個節點（伺服器）原始設定，AD FS。  
  
您可以建立原始設定，AD FS 使用**AD FS 聯盟伺服器設定精靈**來新增至 WID 陣列聯盟伺服器。 如需詳細資訊，請查看[新增聯盟伺服器聯盟伺服器陣列到](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達**指定主要聯盟伺服器及服務 Account**頁面中**AD FS 聯盟伺服器設定精靈**，輸入您錄製準備 AD FS 移轉作業時的服務 account 資訊。 如需詳細資訊，請查看[準備 2.0 聯盟伺服器移轉 AD FS](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達**同盟服務名稱指定**頁面上，請務必選取相同 SSL 憑證您記錄在[準備 2.0 聯盟伺服器移轉 AD FS](prepare-to-migrate-a-wid-farm.md)。  
  
13. 更新您的 AD FS 網頁此 WID 陣列中的最後一個伺服器上。 如果您的移轉準備時備份您自訂 AD FS 網頁，使用您備份的資料覆寫預設 AD FS 網頁中的預設所建立的**%systemdrive%\inetpub\adfs\ls**目錄根據 AD FS 設定 Windows Server 2012 上。  
  
14. 新增您，您只是升級到 Windows Server 2012 負載平衡器 WID 發電廠此最後一個的伺服器。  
  
15. 還原任何剩餘 AD FS 的自訂項目，例如自訂屬性存放區。  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)
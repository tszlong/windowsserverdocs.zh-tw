---
title: 遷移 AD FS 2.0 同盟伺服器 WID 伺服器陣列
description: 提供將 AD FS 2.0 伺服器 WID 伺服器陣列遷移至 Windows Server 2012 的相關資訊
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89da3de4bc626e12a1fc34752841f2de1afb5322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408248"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>遷移 AD FS 2.0 WID 伺服器陣列  
本檔提供將 AD FS 2.0 Windows 內部資料庫（WID）伺服器陣列遷移至 Windows Server 2012 的詳細資訊。

## <a name="migrate-an-ad-fs-wid-farm"></a>遷移 AD FS WID 伺服器陣列
若要將 WID 伺服器陣列遷移至 Windows Server 2012，請執行下列程式：  
  
1.  針對 WID 伺服器陣列中的每個節點（伺服器），檢查並執行[準備遷移 WID 伺服器](prepare-to-migrate-a-wid-farm.md)陣列中的程式。  
  
2.  從負載平衡器移除任何非主要節點。  
  
3.  將此伺服器上的作業系統從 Windows Server 2008 R2 或 Windows Server 2008 升級至 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果會遺失這部伺服器上的 AD FS 設定，並移除 AD FS 2.0 伺服器角色。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須建立原始的 AD FS 設定，並還原其餘的 AD FS 設定，才能完成同盟伺服器遷移。  
  
4. 在這部伺服器上建立原始的 AD FS 設定。  
  
您可以使用 [ **AD FS 同盟伺服器設定] Wizard** ，將同盟伺服器新增到 WID 伺服器陣列，以建立原始的 AD FS 設定。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達 [ **AD FS 同盟伺服器設定] Wizard**中的 [**指定主要同盟伺服器和服務帳戶**] 頁面時，請輸入 WID 伺服器陣列的主要同盟伺服器名稱，並務必輸入您在準備 AD FS 遷移時所記錄的服務帳戶資訊。 如需詳細資訊，請參閱[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達 [**指定同盟服務名稱**] 頁面時，請務必選取您在[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)的「準備遷移 WID 伺服器陣列」中所記錄的同一個 SSL 憑證。  
  
5. 更新這個伺服器上您的 AD FS 網頁。 當您準備進行遷移時，如果您已備份自訂的 AD FS 網頁，則需要使用備份資料來覆寫 **%systemdrive%\inetpub\adfs\ls**目錄中預設建立的預設 AD FS 網頁，做為 Windows Server 2012 上 AD FS 設定的結果。  
  
6. 將您剛升級至 Windows Server 2012 的伺服器新增至負載平衡器。  
  
7. 針對 WID 伺服器陣列中剩餘的次要伺服器，重複步驟1到6。  
  
8. 將 WID 伺服器陣列中其中一個已升級的次要伺服器升級成主要伺服器。 若要這樣做，請開啟 Windows PowerShell，然後執行下列命令： `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 從負載平衡器移除 WID 伺服器陣列中的原始主要伺服器。  
  
10. 使用 Windows PowerShell，將 WID 伺服器陣列中的原始主要伺服器降級為次要伺服器。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將原始的主伺服器降級為次要伺服器： `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. 將 WID 伺服器陣列中這個最後一個節點（伺服器）上的作業系統從 Windows Server 2008 R2 或 Windows Server 2008 升級至 Windows Server 2012。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  升級作業系統之後，此伺服器上的 AD FS 設定會遺失，且會移除 AD FS 2.0 伺服器角色。 系統會改為安裝 Windows Server 2012 AD FS 伺服器角色，但未設定。 您必須手動建立原始的 AD FS 設定，並還原剩餘的 AD FS 設定，以完成同盟伺服器遷移。  
  
12. 在您的 WID 伺服器陣列中，于最後一個節點（伺服器）上建立原始的 AD FS 設定。  
  
您可以使用 [ **AD FS 同盟伺服器設定] Wizard** ，將同盟伺服器新增到 WID 伺服器陣列，以建立原始的 AD FS 設定。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達 [ **AD FS 同盟伺服器設定] Wizard**中的 [**指定主要同盟伺服器和服務帳戶**] 頁面時，請輸入準備 AD FS 遷移時所記錄的服務帳戶資訊。 如需詳細資訊，請參閱[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達 [**指定同盟服務名稱**] 頁面時，請務必選取您在[準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)中所記錄的同一個 SSL 憑證。  
  
13. 在 WID 伺服器陣列中的最後一部伺服器上更新您的 AD FS 網頁。 當您準備進行遷移時，如果您已備份自訂的 AD FS 網頁，請使用備份資料來覆寫 **%systemdrive%\inetpub\adfs\ls**目錄中預設建立的預設 AD FS 網頁，做為 Windows Server 2012 上 AD FS 設定的結果。  
  
14. 將您剛升級至 Windows Server 2012 的 WID 伺服器陣列的最後一部伺服器新增至負載平衡器。  
  
15. 還原任何剩餘的 AD FS 自訂項目，例如自訂屬性存放區。  
  
## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備遷移 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [遷移 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [遷移 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)
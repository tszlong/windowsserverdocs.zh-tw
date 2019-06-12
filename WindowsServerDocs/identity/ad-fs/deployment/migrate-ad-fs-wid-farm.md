---
title: 移轉 AD FS 2.0 同盟伺服器 WID 伺服器陣列
description: 提供有關移轉到 Windows Server 2012 的 AD FS 2.0 伺服器 WID 伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c85a02ae6a71cf31fd172ec012a14cd81c126e16
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445657"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>移轉 AD FS 2.0 WID 伺服器陣列  
本文件提供詳細的資訊移轉 AD FS 2.0 Windows 內部資料庫 (WID) 到 Windows Server 2012 的伺服器陣列。

## <a name="migrate-an-ad-fs-wid-farm"></a>移轉 AD FS WID 伺服器陣列
若要將 WID 伺服器陣列移轉至 Windows Server 2012 中，執行下列程序：  
  
1.  每個節點 （伺服器） 在 WID 伺服器陣列中，檢閱並執行中的程序[準備移轉 WID 伺服器陣列](prepare-to-migrate-a-wid-farm.md)。  
  
2.  從負載平衡器移除任何非主要節點。  
  
3.  在此伺服器的 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上的作業系統升級。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定會遺失且 AD FS 2.0 伺服器角色會被移除。 相反地，安裝 Windows Server 2012 的 AD FS 伺服器角色，但尚未進行設定。 您必須建立原始 AD FS 設定，並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。  
  
4. 在此伺服器上建立原始 AD FS 設定。  
  
您可以使用來建立原始 AD FS 設定**AD FS 同盟伺服器設定精靈**至同盟伺服器新增到 WID 伺服器陣列。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達**指定主要同盟伺服器和服務帳戶**頁面**AD FS 同盟伺服器設定精靈**，輸入 WID 伺服器陣列的主要同盟伺服器的名稱並請務必輸入您準備 AD FS 移轉時所記錄的服務帳戶資訊。 如需詳細資訊，請參閱 <<c0> [ 準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達**指定 Federation Service 名稱**頁面上，請務必選取您在 「 準備移轉 WID 伺服器陣列 」 所記錄的相同 SSL 憑證中[準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md).  
  
5. 更新這個伺服器上您的 AD FS 網頁。 如果您準備移轉時備份您自訂 AD FS 網頁，您需要使用您的備份資料來覆寫預設會在所建立的 AD FS 網頁 **%systemdrive%\inetpub\adfs\ls**目錄視為在 Windows Server 2012 的 AD FS 設定的結果。  
  
6. 新增您剛才升級到 Windows Server 2012 的負載平衡器的伺服器。  
  
7. 針對 WID 伺服器陣列中的其他次要伺服器重複步驟 1 到 6。  
  
8. 將 WID 伺服器陣列中其中一個已升級的次要伺服器升級成主要伺服器。 若要這樣做，請開啟 Windows PowerShell，然後執行下列命令： `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`。  
  
9. 從負載平衡器移除 WID 伺服器陣列中的原始主要伺服器。  
  
10. 使用 Windows PowerShell，將 WID 伺服器陣列中的原始主要伺服器降級為次要伺服器。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以降級為次要伺服器的原始主要伺服器： `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`。  
  
11. 從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 將 WID 伺服器陣列中升級此最後一個節點 （伺服器） 上的作業系統。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  升級作業系統的結果，在此伺服器上的 AD FS 設定會遺失且 AD FS 2.0 伺服器角色會被移除。 相反地，安裝 Windows Server 2012 的 AD FS 伺服器角色，但尚未進行設定。 您必須手動建立原始 AD FS 設定，並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。  
  
12. 將 WID 伺服器陣列中這個最後一個節點 （伺服器） 上建立原始 AD FS 設定。  
  
您可以使用來建立原始 AD FS 設定**AD FS 同盟伺服器設定精靈**至同盟伺服器新增到 WID 伺服器陣列。 如需詳細資訊，請參閱[將同盟伺服器新增至同盟伺服器陣列](add-a-federation-server-to-a-federation-server-farm.md)。  
  
> [!NOTE]
> 當您到達**指定主要同盟伺服器和服務帳戶**頁面**AD FS 同盟伺服器設定精靈**，輸入您所錄製的服務帳戶資訊時準備適用於 AD FS 移轉。 如需詳細資訊，請參閱 <<c0> [ 準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。 
>  
> 當您到達**指定 Federation Service 名稱**頁面上，請務必選取您在所記錄的相同 SSL 憑證[準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-a-wid-farm.md)。  
  
13. 更新您在 WID 伺服器陣列中這個最後一部伺服器上的 AD FS 網頁。 如果您準備移轉時備份您自訂 AD FS 網頁，請使用備份資料來覆寫預設會在所建立的 AD FS 網頁 **%systemdrive%\inetpub\adfs\ls**的目錄在 Windows Server 2012 AD FS 設定。  
  
14. 加入您剛才升級到 Windows Server 2012 的負載平衡器將 WID 伺服器陣列的這個最後一個伺服器。  
  
15. 還原任何剩餘的 AD FS 自訂項目，例如自訂屬性存放區。  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)
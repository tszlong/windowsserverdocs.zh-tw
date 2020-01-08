---
title: 在 Windows 10 1709 版、Windows Server 1709 版和更新版本中，預設不會安裝 SMBv1
description: 討論 Windows 10 秋季建立者更新和 Windows Server 版本1709和更新版本中 SMBv1 通訊協定的行為。
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9a6e9778e0ce1e50a70e68832390321fb2d9f971
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654359"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>在 Windows 10 1709 版、Windows Server 1709 版和更新版本中，預設不會安裝 SMBv1

## <a name="summary"></a>摘要

在 Windows 10 中的建立者更新和 Windows Server 版本1709（RS3）和更新版本中，預設不會再安裝伺服器訊息區第1版（SMBv1）網路通訊協定。 從2007年開始，SMBv2 和更新的通訊協定已取代此檔案。 Microsoft 已公開淘汰2014中的 SMBv1 通訊協定。 

SMBv1 在 Windows 10 秋季建立者更新和 Windows Server 版本1709（RS3）中具有下列行為： 
 
- SMBv1 現在具有可分別卸載的用戶端和伺服器子功能。    
- Windows 10 Enterprise 和 Windows 10 教育版在全新安裝之後，預設不再包含 SMBv1 用戶端或伺服器。    
- Windows Server 2016 在全新安裝之後，預設不再包含 SMBv1 用戶端或伺服器。    
- Windows 10 Home 和 Windows 10 Professional 在全新安裝之後，預設不再包含 SMBv1 伺服器。    
- Windows 10 家庭和 Windows 10 專業版在全新安裝之後，預設仍包含 SMBv1 用戶端。 如果 SMBv1 用戶端的總計未使用15天（不包括電腦關閉），它會自動卸載。    
- Windows 10 家庭和 Windows 10 專業版的就地升級和 Insider 航班，一開始不會自動移除 SMBv1。 如果 SMBv1 用戶端或伺服器的總計未使用15天（不包括電腦關閉的時間），它們會各自自動卸載。     
- Windows 10 企業版和 Windows 10 教育版的就地升級和 Insider 航班不會自動移除 SMBv1。 系統管理員必須決定在這些受管理的環境中卸載 SMBv1。 在 Windows 10 版本1809（RS5）和更新版本中，系統管理員可以藉由開啟「SMB 1.0/CIFS 自動移除」功能來啟用自動移除 SMBv1。    
- 15天后自動移除 SMBv1 是一次性的作業。 如果系統管理員重新安裝 SMBv1，將不會再嘗試卸載。
- SMB 版本2.02、2.1、3.0、3.02 和3.1.1 功能仍受到完整支援，且預設會納入為 SMBv2 二進位檔的一部分。    
- 因為電腦瀏覽器服務依賴 SMBv1，所以如果卸載 SMBv1 用戶端或伺服器，就會卸載服務。 這表示 Explorer Networkcan 不會再透過舊版 NetBIOS 資料包流覽方法來顯示 Windows 電腦。    
- 您仍然可以在所有版本的 Windows 10 和 Windows Server 2016 中重新安裝 SMBv1。    
 
  > [!NOTE]
  > Windows 10 版本1803（RS4） Professional 的處理方式與 Windows 10 版本1703（RS2）和 Windows 10，版本1607（RS1）相同。 此問題已在 Windows 10 版本1809（RS5）中修正。 您仍然可以手動卸載 SMBv1。 不過，在下列案例中，Windows 不會在15天后自動卸載 SMBv1： 
 
-  您會執行 Windows 10 版本1803的全新安裝。     
-  您可直接將 Windows 10 版本1607或 Windows 10 （版本1703）升級至 Windows 10，版本1803，而不需要先升級至 Windows 10 版本1709。     
 
如果您嘗試連線到僅支援 SMBv1 的裝置，或這些裝置嘗試連線到您，您可能會收到下列其中一則錯誤訊息：     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

當遠端伺服器需要來自此用戶端的 SMBv1 連線，但在用戶端上卸載或停用 SMBv1 時，會出現下列事件。

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

這些裝置不可能執行 Windows。 它們較可能執行較舊版本的 Linux、Samba 或其他類型的協力廠商軟體，以提供 SMB 服務。 這些版本的 Linux 和 Samba 通常已不再受支援。 

> [!NOTE]
> Windows 10 版本1709也稱為「秋季建立者更新」。   

## <a name="more-information"></a>更多資訊

若要解決此問題，請洽詢僅支援 SMBv1 的產品製造商，並要求支援 SMBv 2.02 或更新版本的軟體或固件更新。 如需目前已知的廠商清單及其 SMBv1 需求，請參閱下列 Windows 和 Windows Server Storage 工程小組的 Blog 文章： 

[SMBv1 產品 Clearinghouse](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>租用模式

如果需要 SMBv1 才能為舊版軟體行為提供應用程式相容性（例如停用 oplocks 的需求），Windows 會提供新的 SMB 共用旗標，稱為租用模式。此旗標會指定共用是否停用新式 SMB 的語義，例如租用和 oplocks。

您可以指定不使用 oplocks 或租用的共用，以允許繼承應用程式使用 SMBv2 或更新版本。 若要這麼做，請使用**directorynew-smbshare**或**directorynew-smbshare** PowerShell Cmdlet 搭配 **-LeasingMode None** 參數。

> [!NOTE]
> 您應該只在協力廠商應用程式需要的共用上使用此選項，才能取得舊版支援。 請勿在向外延展檔案伺服器所使用的使用者資料共用或 CA 共用上指定租用模式。 這是因為移除 oplocks 和租用會導致大部分應用程式中的不穩定和資料損毀。 租用模式只適用于共用模式。 可供任何用戶端作業系統使用。

#### <a name="explorer-network-browsing"></a>Explorer 網路流覽

電腦瀏覽器服務依賴 SMBv1 通訊協定來填入 Windows Explorer 網路節點（也稱為「網路鄰近」）。 這個舊版的通訊協定已過時，不會路由傳送，而且安全性也很有限。 因為服務無法在沒有 SMBv1 的情況下運作，所以會同時移除。

不過，如果您仍然需要使用 Explorer 網路輸入家庭和小型企業工作組環境來尋找以 Windows 為基礎的電腦，您可以在不再使用 SMBv1 的 Windows 電腦上執行下列步驟： 
 
1. 啟動 [函數探索提供者主機] 和 [函數探索資源發行集] 服務，然後將它們設定為 **[自動（延遲啟動）** ]。

2. 當您開啟 [Explorer 網路] 時，請在系統提示時啟用網路探索。    
 
該子網內所有具有這些設定的 Windows 裝置，現在都會出現在 [網路] 中供流覽。 這會使用 WS-DISCOVERY 通訊協定。 當 Windows 裝置出現之後，如果裝置仍未出現在此瀏覽清單中，請洽詢其他廠商和製造商。 這可能是因為已停用此通訊協定，或是僅支援 SMBv1。

> [!NOTE]
> 建議您對應磁片磁碟機和印表機，而不要啟用這項功能，但仍需要搜尋和流覽其裝置。 對應的資源較容易找到，需要較少的定型，而且更安全地使用。 如果透過群組原則自動提供這些資源，這就更是如此。 系統管理員可以使用 IP 位址、Active Directory Domain Services （AD DS）、Bonjour、Mdn、uPnP 等等，以不同于舊版電腦瀏覽器服務的方法來設定印表機。

如果您無法使用上述任何因應措施，或如果應用程式製造商無法提供支援的 SMB 版本，您可以遵循[如何在 Windows 中偵測、啟用和停用 SMBv1、SMBv2 和 SMBv3](detect-enable-and-disable-smbv1-v2-v3.md)中的步驟，手動重新啟用 SMBv1。

> [!IMPORTANT]
> 我們強烈建議您不要重新安裝 SMBv1。 這是因為這個較舊的通訊協定有關于勒索軟體和其他惡意程式碼的已知安全性問題。   

## <a name="references"></a>參考資料

[停止使用 SMB1](https://aka.ms/stopusingsmb1)
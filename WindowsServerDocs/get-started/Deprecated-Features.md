---
title: Windows Server 2016 已移除或過時的功能
description: 列出的 Windows Server 2016 特性與功能已從目前的產品版本中移除，或已計劃可能會從後續版本中移除 (已過時)。 適用對象是在商業環境中更新作業系統的 IT 專業人員。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 05/21/2019
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: a58b7d1fe7124eb26b29c13ca53031ded8ed3d62
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544549"
---
# <a name="features-removed-or-deprecated-in--windows-server-2016"></a>Windows Server 2016 已移除或過時的功能

>適用於：Windows Server 2016

以下清單列出的 Windows Server 2016 特性與功能已從目前的產品版本中移除，或已計劃可能會從後續版本中移除 (已過時)。 適用對象是在商業環境中更新作業系統的 IT 專業人員。 本清單在後續版本可能會有變更，且可能無法涵括每一項過時的功能。 如需特定功能或替代功能的詳細資訊，請參閱該功能的文件。

如需更新版本中已移除或已淘汰功能的資訊，請參閱[從 Windows Server 2019 開始移除或計劃取代的功能](../get-started-19/removed-features-19.md)。

## <a name="features-removed-from-windows-server-2016"></a>Windows Server 2016 已移除的功能

下列特性與功能已從這個版本的 Windows Server 2016 中移除。 除非您使用其他替代方法，否則與這些功能相依的應用程式、程式碼或使用方式，將無法在這個版本中運作。  

> [!NOTE]  
> 如果您是從 Windows Server 2012 R2 或 Windows Server 2012 之前的伺服器版本移至 Windows Server 2016，也應該檢閱 [Windows Server 2012 R2 已移除或過時的功能](https://technet.microsoft.com/library/dn303411.aspx)和 [Windows Server 2012 已移除或過時的功能](https://technet.microsoft.com/library/hh831568.aspx)。  


### <a name="file-server"></a>檔案伺服器  
Microsoft Management Console 的 [共用與存放管理] 嵌入式管理單元已經移除。 請改為執行下列其中一個動作：  

-   如果您想要管理的電腦正在執行的作業系統比 Windows Server 2016 還舊，請使用遠端桌面連線到該電腦，然後使用 [共用與存放管理] 嵌入式管理單元的本機版本。  

-   在執行 Windows 8.1 或更早版本的電腦上，使用 RSAT 的 [共用與存放管理] 嵌入式管理單元，來檢視您想要管理的電腦。  

-   在用戶端電腦上使用 Hyper-V，來執行在 RSAT 中具有 [共用與存放管理] 嵌入式管理單元且執行 Windows 7、Windows 8 或 Windows 8.1 的虛擬機器。  

### <a name="journaldll"></a>Journal.dll  
Journal.dll 已從 Windows Server 2016 中移除。 沒有任何取代項目。  

### <a name="security-configuration-wizard"></a>安全性設定精靈  
「安全性設定精靈」已經移除。 目前，功能依預設會受到保護。 如果您需要控制特定的安全性設定，您可以使用群組原則或 [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx)。  

### <a name="sqm"></a>SQM  
管理參與「客戶經驗改進計畫」的加入元件已經移除。 

### <a name="windows-update"></a>Windows Update
**wuauclt.exe /detectnow** 命令已移除，不再支援。 若要觸發對更新的掃描，請執行下列其中一項：

- 執行 PowerShell 命令：
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"
    $AutoUpdates.DetectNow()
    ````

- 或者，使用此 VBScript：
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## <a name="features-deprecated-starting-with-windows-server-2016"></a>自 Windows Server 2016 起過時的功能 
下列功能從這個版本開始過時。 雖然這些功能最後會完全從產品中移除，但是這個版本仍將提供這些功能 (可能會移除某些功能)。 您現在就應該開始針對所有依存於這些功能的應用程式、程式碼或使用方式，規劃替代方法。  

### <a name="configuration-tools"></a>設定工具  

-   **Scregedit.exe** 已淘汰。 如果您的指令碼相依於 Scregedit.exe，請調整它們以使用 Reg.exe 或 Windows PowerShell 方法。  

-   **Sconfig.exe** 已淘汰。 請改用 Windows PowerShell。  

### <a name="netcfg-custom-apis"></a>NetCfg 自訂 API  
使用 NetCfg 自訂 API 安裝 PrintProvider、NetClient 及 ISDN 的方式已經過時。  

### <a name="remote-management"></a>遠端管理  
WinRM.vbs 已經過時。 請改用 Windows PowerShell 的 WinRM 提供者中的功能。  

### <a name="smb"></a>SMB  
SMB 2+ over NetBT 已經過時。 請改為實作 SMB over TCP 或 RDMA。 

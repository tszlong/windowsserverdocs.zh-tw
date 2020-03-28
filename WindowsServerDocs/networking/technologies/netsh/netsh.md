---
title: 網路殼層 (Netsh)
description: 本主題提供 Windows Server 2016 中的網路殼層 (Netsh) 命令列公用程式概觀。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: d9103585d1868f586f169f01096c4d37961e7033
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316681"
---
# <a name="network-shell-netsh"></a>網路殼層 \(Netsh\)

>適用於：Windows Server (半年度管道)、Windows Server 2016

網路殼層 (Netsh) 是命令列公用程式，可讓您將各種網路通訊伺服器角色與元件安裝在執行 Windows Server 2016 的電腦之後，加以設定並顯示狀態。

某些用戶端技術 (例如動態主機組態通訊協定 \(DHCP\) 用戶端和 BranchCache) 也提供 netsh 命令，可讓您設定執行 Windows 10 的用戶端電腦。

在大部分的情況下，當您針對每個網路伺服器角色或網路功能使用 Microsoft Management Console \(MMC\) 嵌入式管理單元\-時，netsh 命令會提供相同的功能。 例如，您可以使用 NPS MMC 嵌入式管理單元或 **netsh NPS** 內容中的 netsh 命令﹐來設定網路原則伺服器 \(NPS\)。

此外，還有適用於網路技術的 netsh 命令 (例如 IPv6、網路橋接器，以及遠端程序呼叫 \(RPC\))，無法在 Windows Server 中作為 MMC 嵌入式管理單元使用。

>[!IMPORTANT]
>建議您使用 Windows PowerShell 來管理 [Windows Server 2016 和 Windows 10](https://technet.microsoft.com/library/mt156917.aspx) 中的網路技術，而不是使用網路殼層。 不過，為了與您的指令碼相容，系統會納入網路殼層，並可支援使用。

## <a name="network-shell-netsh-technical-reference"></a>網路殼層 (Netsh) 技術參考

Netsh 技術參考提供完整的 netsh 命令參考，包括語法、參數和 Netsh 命令範例。 您可以使用 Netsh 技術參考，在執行 Windows Server 2016 和 Windows 10 的電腦上，使用 netsh 命令來建立指令碼和批次檔，以在本機或遠端管理網路技術。  
  
### <a name="content-availability"></a>內容可用性  
  
您可以從 TechNet 資源庫中以 Windows Help \(* .chm\) 格式下載網路殼層技術參考：[ 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---

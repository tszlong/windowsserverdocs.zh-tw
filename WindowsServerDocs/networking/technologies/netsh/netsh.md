---
title: 網路 Shell (Netsh)
description: 本主題提供的網路介面 (netsh) 命令列公用程式，Windows Server 2016 中的概觀。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>網路殼層 \(Netsh\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

網路殼層 (netsh) 是一個命令列的公用程式，可讓您設定，並在執行 Windows Server 2016 的電腦上安裝了之後顯示各種不同的網路通訊伺服器角色與元件的狀態。

>[!NOTE]
>本主題中，除了下列網路殼層 content 使用。
>
> - [Netsh 命令語法、內容，以及格式](netsh-contexts.md)
> - [網路 Shell (Netsh) 範例批次檔案](netsh-wins.md)
> - [Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

有些 client 技術，例如動態主機設定通訊協定 \(DHCP\) client 和 BranchCache，也提供 netsh 命令可讓您設定 client 電腦正在執行 Windows 10。

在大部分案例中，netsh 命令提供了當您使用 Microsoft Management Console \(MMC\) snap\ 時所提供的相同功能-中的每個網路伺服器角色或網路功能。 例如，您可以設定網路原則伺服器 \(NPS\) 使用 NPS MMC 嵌入式管理單元或中的 netsh 命令**netsh nps**操作。

除此之外，有 netsh 命令網路技術，例如 IPv6、網路橋接器和遠端程序呼叫 \(RPC\)，時無法使用 Windows Server MMC 嵌入式管理單元中。

>[!IMPORTANT]
>建議您使用 Windows PowerShell 來管理網路技術[Windows Server 2016 和 Windows 10](https://technet.microsoft.com/library/mt156917.aspx)除了網路殼層。 網路殼層更新也包含您的指令碼，與相容性不過，以及其使用支援。

## <a name="network-shell-netsh-technical-reference"></a>網路 Shell (Netsh) 技術參考

Netsh 技術參考提供完整 netsh 命令參考資料，包括語法、參數和 netsh 命令的範例。 使用 netsh 命令本機或遠端管理執行 Windows Server 2016 和 Windows 10 電腦上的網路技術建置的指令碼與「批次檔案，您可以使用 Netsh 技術參考。  
  
### <a name="content-availability"></a>內容可用性  
  
網路殼層技術參考可供下載 TechNet 主題館的 Windows 協助 \(*.chm\) 格式：[Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  


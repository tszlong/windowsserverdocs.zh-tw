---
title: 網路殼層 (Netsh)
description: 本主題提供 Windows Server 2016 中的網路殼層 (netsh) 命令列公用程式概觀。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849359"
---
# <a name="network-shell-netsh"></a>網路殼層\(Netsh\)

>適用於：Windows Server （半年通道），Windows Server 2016

網路殼層 (netsh) 是命令列公用程式，可讓您設定並將它們安裝在執行 Windows Server 2016 的電腦上後顯示各種網路通訊伺服器角色和元件的狀態。

某些用戶端技術，例如動態主機設定通訊協定\(DHCP\)用戶端和 BranchCache，也提供可讓您設定執行 Windows 10 的用戶端電腦的 netsh 命令。

在大部分情況下，netsh 命令提供相同的功能所提供當您使用 Microsoft Management Console \(MMC\)貼齊\-中針對每個網路的伺服器角色或網路功能。 例如，您可以設定網路原則伺服器\(NPS\)使用 NPS MMC 嵌入式管理單元或中的 netsh 命令**netsh nps**內容。

此外，還有適用於網路技術的 netsh 命令這類 IPv6 」、 「 網路橋接器，和 「 遠端程序呼叫\(RPC\)，不是 MMC 嵌入式管理單元的 Windows Server 中。

>[!IMPORTANT]
>建議使用 Windows PowerShell 管理網路技術的[Windows Server 2016 和 Windows 10](https://technet.microsoft.com/library/mt156917.aspx)而不是網路介面。 網路殼層包含與您的指令碼的相容性，但可支援其使用。

## <a name="network-shell-netsh-technical-reference"></a>網路殼層 (Netsh) 技術參考

Netsh 技術參考提供完整的 netsh 命令參考，包括語法、 參數和 netsh 命令的範例。 您可以使用 Netsh 技術參考，來建置指令碼和批次檔使用 netsh 命令的網路技術，在執行 Windows Server 2016 和 Windows 10 電腦上的本機或遠端管理。  
  
### <a name="content-availability"></a>內容的可用性  
  
網路介面技術參考是可供下載 Windows 協助\(*.chm\)從 TechNet 組件庫的格式：[Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---

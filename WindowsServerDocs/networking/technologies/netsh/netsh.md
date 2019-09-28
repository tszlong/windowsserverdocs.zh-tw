---
title: 網路殼層 (Netsh)
description: 本主題提供 Windows Server 2016 中的 Network Shell （netsh）命令列公用程式的總覽。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405549"
---
# <a name="network-shell-netsh"></a>網路命令介面 \(Netsh @ no__t-1

>適用於：Windows Server (半年度管道)、Windows Server 2016

Network shell （netsh）是一種命令列公用程式，可讓您在執行 Windows Server 2016 的電腦上安裝各種網路通訊伺服器角色和元件之後，設定和顯示其狀態。

某些用戶端技術（例如動態主機設定通訊協定 \(DHCP @ no__t-1 用戶端和 BranchCache）也提供 netsh 命令，可讓您設定執行 Windows 10 的用戶端電腦。

在大部分的情況下，當您針對每個網路伺服器角色或網路功能使用 Microsoft Management Console \(MMC @ no__t-1 貼齊 @ no__t-2in 時，netsh 命令會提供相同的功能。 例如，您可以使用 NPS MMC 嵌入式管理單元或**netsh NPS**內容中的 netsh 命令，來設定網路原則伺服器 \(NPS @ no__t-1。

此外，還有適用于網路技術的 netsh 命令，例如 IPv6、網路橋接器和遠端程序呼叫 \(RPC @ no__t-1，在 Windows Server 中無法當做 MMC 嵌入式管理單元使用。

>[!IMPORTANT]
>建議您使用 Windows PowerShell 來管理[Windows Server 2016 和 windows 10](https://technet.microsoft.com/library/mt156917.aspx)中的網路技術，而不是使用網路介面。 為了與您的腳本相容，會包含網路介面，並支援其使用方式。

## <a name="network-shell-netsh-technical-reference"></a>Network Shell （Netsh）技術參考

Netsh 技術參考提供完整的 netsh 命令參考，包括語法、參數和 Netsh 命令的範例。 您可以使用 Netsh 技術參考，在執行 Windows Server 2016 和 Windows 10 的電腦上，使用 netsh 命令來建立腳本和批次檔，以進行本機或遠端系統管理網路技術。  
  
### <a name="content-availability"></a>內容可用性  
  
您可以從 TechNet 資源庫中的 Windows Help \( * .chm @ no__t-1 格式下載網路命令介面技術參考：[Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---

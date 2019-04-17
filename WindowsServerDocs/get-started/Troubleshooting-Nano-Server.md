---
title: 針對 Nano 伺服器進行疑難排解
description: 修復主控台、緊急管理服務、核心偵錯
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 0f5d3e352cd022853a1602c67c3aaf2530cfc696
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081994"
---
# <a name="troubleshooting-nano-server"></a>針對 Nano 伺服器進行疑難排解

>適用於︰Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 版本 1709 開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

本主題所包含的相關資訊是有關可用來連線至、診斷和修復 Nano 伺服器安裝的工具。  
  
## <a name="using-the-nano-server-recovery-console"></a>使用 Nano 伺服器修復主控台 
 
Nano 伺服器包含修復主控台，確保即使錯誤網路組態干擾與 Nano 伺服器的連線，您還是可以存取 Nano 伺服器。 您可以使用修復主控台修正網路，然後使用一般的遠端管理工具。  
  
當您在虛擬機器中或已連接監視器和鍵盤的實體電腦上啟動 Nano 伺服器時，會看到全螢幕的文字模式登入提示。 使用系統管理員帳戶登入這個提示字元，以查看 Nano 伺服器的電腦名稱和 IP 位址。 您可以使用下列命令，在這個主控台中進行瀏覽︰  
  
-   使用方向鍵捲動  
  
-   使用 TAB 移至任何開頭為 **>** 的文字，然後按 ENTER 進行選取。  
  
-   若要往回一個畫面或頁面，請按 ESC 鍵。 如果您在首頁，請按 ESC 鍵登出。  
  
-   有些畫面會在畫面的最後一行顯示其他功能。 例如，如果您瀏覽網路介面卡，F4 會停用網路介面卡。  
  
修復主控台可讓您檢視與設定網路介面卡和 TCP/IP 設定，以及防火牆規則。
> [!NOTE]  
    > 修復主控台只支援基本鍵盤功能。 不支援鍵盤背光、10 個按鍵區段和鍵盤配置切換 (例如 Caps Lock 和 Number Lock)。 只支援英文鍵盤和字元集。

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>使用緊急管理服務透過序列連接埠存取 Nano 伺服器  
緊急管理服務 (EMS) 可讓您執行基本疑難排解、取得網路狀態，以及使用終端機模擬器透過序列連接埠開啟主控台工作階段 (包括 CMD/PowerShell)。 這會取代使用鍵盤和監視器進行伺服器疑難排解的需求。 如需 EMS 的詳細資訊，請參閱 [Emergency Management Services Technical Reference](https://technet.microsoft.com/library/cc784411(v=ws.10).aspx) (緊急管理服務技術參照)。

若要在 Nano 伺服器映像上啟用 EMS，供您稍後需要使用時使用，請執行這個 Cmdlet︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
這個範例 Cmdlet 會使用 9600 bps 的傳輸速率在序列連接埠 3 上啟用 EMS。 如果您不想包含這些參數，則預設值是連接埠 1 和 115200 bps。 若要將這個 Cmdlet 用於 VHDX 媒體，請一定要包含 Hyper-V 功能，以及對應的 Windows PowerShell 模組。

## <a name="kernel-debugging"></a>核心偵錯  
您可以設定 Nano 伺服器映像，透過各種方法支援核心偵錯。 若要搭配使用核心偵錯與 VHDX 映像，請一定要包含 Hyper-V 功能，以及對應的 Windows PowerShell 模組。 如需遠端核心偵錯的基本詳細資訊，請參閱[Setting Up Kernel-Mode Debugging over a Network Cable Manually](https://msdn.microsoft.com/library/windows/hardware/hh439346%28v=vs.85%29.aspx) (透過網路纜線手動設定核心模式偵錯) 和 [Remote Debugging Using WinDbg](https://msdn.microsoft.com/library/windows/hardware/hh451173%28v=vs.85%29.aspx) (使用 WinDbg 的遠端偵錯)。  
  
### <a name="debugging-using-a-serial-port"></a>使用序列連接埠偵錯  
請使用這個 Cmdlet，透過序列連接埠針對映像進行偵錯︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
這個範例會使用 9600 bps 的傳輸速率在序列連接埠 2 上啟用偵錯。 如果您未指定這些參數，則預設值是連接埠 2 和 115200 bps。 如果您想要同時使用 EMS 和核心偵錯，則必須將其設定成使用兩個不同的序列連接埠。  
  
### <a name="debugging-over-a-tcpip-network"></a>透過 TCP/IP 網路偵錯  
請使用這個 Cmdlet，透過 TCP/IP 網路針對映像進行偵錯︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
這個 Cmdlet 會啟用核心偵錯，只允許 IP 位址為 192.168.1.100 的電腦連線，而且所有通訊都是透過連接埠 64000 進行。 -DebugRemoteIP 和 -DebugPort 是必要參數，而且連接埠號碼大於 49152。 這個 Cmdlet 會在檔案和產生的 VHD 中產生加密金鑰，這個加密金鑰是透過連接埠進行通訊所需的項目。 或者，您可以使用 -DebugKey 參數指定自己的金鑰 (如這個範例所示)︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>使用 IEEE1394 通訊協定偵錯 (Firewire)  
若要透過 IEEE1394 啟用偵錯，請使用這個範例 Cmdlet︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
-DebugChannel 是必要參數。  
  
### <a name="debugging-using-usb"></a>使用 USB 偵錯  
您可以使用這個 Cmdlet，透過 USB 進行偵錯︰  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
當您將遠端偵錯工具連線至產生的 Nano 伺服器時，請指定 -DebugTargetName 參數所設定的目標名稱。    
---
title: HYPER-V 虛擬機器連線
description: 描述虛擬機器連線，可讓您的虛擬機器的遠端存取。 包含有關如何執行一般工作，例如傳送 Ctrl Alt 刪除虛擬機器的詳細資料。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e1f3260fdbbd82a97c3b0949936afc6a04ec5e5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887839"
---
# <a name="hyper-v-virtual-machine-connection"></a>HYPER-V 虛擬機器連線

>適用於：Windows Server 2016，Windows 10，Windows 8.1、 Windows Server 2012 R2、 Windows Server 2012，Windows 8

虛擬機器連線\(VMConnect\)是您用來連接至虛擬機器，以便您可以安裝或與其互動客體作業系統的虛擬機器中的工具。 您可以使用 VMConnect 執行的工作包括下列：  
  
-   啟動和關閉虛擬機器  
  
-   連接到 DVD 映像\(.iso 檔案\)或 USB 快閃磁碟機  
  
-   建立檢查點  
  
-   修改虛擬機器的設定  
    
## <a name="tips-for-using-vmconnect"></a>使用 VMConnect 的秘訣  
您可能會發現下列資訊很有幫助使用 VMConnect:  
  
|若要這樣做...|執行此動作...|  
|---------------|------------|  
|按下滑鼠或鍵盤輸入虛擬機器傳送|按一下 [虛擬機器] 視窗中的任何位置。 將滑鼠指標可能會顯示為一個小點，當您連接到執行中虛擬機器。|  
|傳回按下滑鼠或鍵盤輸入到實體電腦|按住 ctrl 鍵\+ALT\+左箭號，然後將滑鼠指標在虛擬機器的時段之外。 可以在 Hyper-v 中變更此滑鼠釋放按鍵組合\-Hyper V 設定\-V 管理員。|  
|傳送 CTRL\+ALT\+DELETE 按鍵組合的虛擬機器|選取 **動作** > **Ctrl\+Alt\+刪除**或使用按鍵組合 CTRL\+ALT\+結束。|  
|從視窗模式切換到完整\-螢幕模式|選取 **檢視** > **全螢幕模式**。 若要切換回視窗模式，請按 CTRL\+ALT\+中斷。|  
|建立檢查點，以擷取目前狀態的機器進行疑難排解|選取 **動作** > **檢查點**或使用按鍵組合 CTRL\+n。|  
|變更虛擬機器的設定|選取 **檔案** > **設定**。|  
|連接到 DVD 映像\(.iso 檔案\)或 虛擬磁碟片\(.vfd 檔案\)|選取 **媒體**。<br /><br />第 2 代虛擬機器不支援虛擬軟碟機。 如需詳細資訊，請參閱 <<c0> [ 應該在 HYPER-V 中建立 1 或 2 代虛擬機器？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)。|  
|使用 Hyper-v 主機的本機資源\-V 虛擬機器，例如 USB 快閃磁碟機|開啟加強的工作階段模式，在 HYPER-V 主機、 使用 VMConnect 來連線至虛擬機器，並在連接之前，選擇您想要使用的本機資源。 如需特定步驟，請參閱[Hyper-v 上使用本機資源\-V 虛擬機器搭配 VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。|  
|儲存虛擬機器的 VMConnect 設定變更。|在 Windows PowerShell 或命令提示字元中執行下列命令：<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|防止 VMConnect 使用者接管另一位使用者 VMConnect 工作階段|[開啟 HYPER-V 主機上的 增強的工作階段模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER)。<br /><br />不需要增強式開啟的工作階段模式可能會造成安全性和隱私權風險。 如果使用者已連接並登入連接到相同的虛擬機器的虛擬機器透過 VMConnect 和其他授權的使用者、 工作階段會接手第二個使用者和第一位使用者將會遺失工作階段。 第二個使用者將能夠檢視第一位使用者的桌面、 文件和應用程式。|
|管理 integration services 或可讓 VM 與 HYPER-V 主機通訊的元件| 在執行 Windows 10 或 Windows Server 2016 的 HYPER-V 主機，您無法管理搭配 VMConnect integration services。 請參閱下列主題： <br />- [開啟/關閉 整合服務，從 HYPER-V 主機](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [開啟/關閉 integration services，從 Windows 虛擬機器](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [開啟/關閉 整合服務，從 Linux 虛擬機器](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [新的更新虛擬機器的整合服務](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />對於執行 Windows Server 2012 或 Windows Server 2012 R2 的主機，請參閱[Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx)。|
|VMConnect 視窗大小調整|您可以變更 VMConnect 視窗，執行 Windows 作業系統的第 2 代虛擬機器的大小。 若要這樣做，您可能需要開啟加強的工作階段模式，在 HYPER-V 主機。 如需詳細資訊，請參閱 <<c0> [ 開啟 HYPER-V 主機上的 增強的工作階段模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER)。 對於執行 Ubuntu 的虛擬機器，請參閱[HYPER-V VM 中變更 Ubuntu 螢幕解析度](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。|


## <a name="keyboard-shortcuts"></a>鍵盤快速鍵  
根據預設，鍵盤輸入和滑鼠按下滑鼠就會傳送至虛擬機器。 因此您可能需要按下 CTRL + ALT + 左箭號，再使用下列快速鍵。 

|按鍵組合|描述|  
|-------------------|---------------|  
|CTRL\+ALT\+向左箭號|滑鼠釋放|  
|CTRL\+ALT\+END|CTRL 相等\+ALT\+刪除虛擬機器中|  
|CTRL\+ALT\+BREAK|從全文切換\-回到視窗模式的螢幕模式|  
|CTRL\+O|開啟虛擬機器的設定|  
|CTRL\+S|啟動虛擬機器|  
|CTRL\+N|建立檢查點|  
|CTRL\+E|還原至檢查點|  
|CTRL\+C|請勿螢幕擷取畫面|  

## <a name="see-also"></a>另請參閱  
-   [搭配 VMConnect 的 HYPER-V 虛擬機器上使用本機資源](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Windows Server 2016 上的 hyper V](../Hyper-V-on-Windows-Server.md)  
-   [在 Windows 10 的 hyper V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  

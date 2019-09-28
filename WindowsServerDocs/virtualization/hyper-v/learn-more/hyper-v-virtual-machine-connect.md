---
title: Hyper-v 虛擬機器連線
description: 描述可提供虛擬機器遠端存取的虛擬機器連線。 包含如何執行一般工作的詳細資料，例如傳送 Ctrl-Alt-Delete 至虛擬機器。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fba83d22d9e5d9f31a5809781aa04943cc4cd3af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364149"
---
# <a name="hyper-v-virtual-machine-connection"></a>Hyper-v 虛擬機器連線

>適用於：Windows Server 2016、Windows 10、Windows 8.1、Windows Server 2012 R2、Windows Server 2012、Windows 8

虛擬機器\(連線 VMConnect\)是一種工具，可讓您用來連線到虛擬機器，以便在虛擬機器中安裝或與客體作業系統進行互動。 您可以使用 VMConnect 執行的一些工作包括下列各項：  
  
-   啟動和關閉虛擬機器  
  
-   連接到 DVD 圖像\( \) .iso 檔案或 USB 快閃磁片磁碟機  
  
-   建立檢查點  
  
-   修改虛擬機器的設定  
    
## <a name="tips-for-using-vmconnect"></a>使用 VMConnect 的秘訣  
您可能會發現下列資訊對於使用 VMConnect 很有説明：  
  
|若要執行此動作 。|執行此動作 。|  
|---------------|------------|  
|將滑鼠按一下或鍵盤輸入傳送至虛擬機器|按一下 [虛擬機器] 視窗中的任何位置。 當您連接到執行中的虛擬機器時，滑鼠指標可能會顯示為小點。|  
|將滑鼠按一下或鍵盤輸入返回實體電腦|按 CTRL\+ALT\+向左鍵，然後將滑鼠指標移至 [虛擬機器] 視窗外部。 您可以在 [hyper-v\- \-管理員] 的 [hyper-v 設定] 中變更此滑鼠放開按鍵組合。|  
|將 CTRL\+ALT\+DELETE 按鍵組合傳送到虛擬機器|選取 [**動作** > ] **\+ctrl alt\+Delete**或使用按鍵組合 ctrl\+ \+alt END。|  
|從視窗模式切換至全\-螢幕模式|選取 [**全螢幕模式]** 。  >  若要切換回視窗模式，請按\+CTRL\+ALT BREAK。|  
|建立檢查點以取得電腦的目前狀態以進行疑難排解|選取 [**動作** > **檢查點**] 或使用按鍵\+組合 CTRL N。|  
|變更虛擬機器的設定|選取 [檔案**設定**]。  > |  
|連接到 DVD 映射\( \) .iso 檔案或虛擬磁片\(.vfd 檔案\)|選取 [**媒體**]。<br /><br />第2代虛擬機器不支援虛擬磁片。 如需詳細資訊，請參閱[我應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)。|  
|在 hyper-v\-虛擬機器上使用主機的本機資源，例如 USB 快閃磁片磁碟機|在 Hyper-v 主機上開啟加強的會話模式、使用 VMConnect 連線到虛擬機器，然後在連接之前，選擇您要使用的本機資源。 如需特定步驟，請參閱在[具有 VMConnect 的\-hyper-v 虛擬機器上使用本機資源](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。|  
|變更虛擬機器的已儲存 VMConnect 設定|在 Windows PowerShell 或命令提示字元中執行下列命令：<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|防止 VMConnect 使用者接管另一位使用者的 VMConnect 會話|在[hyper-v 主機上開啟加強的會話模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host)。<br /><br />未開啟增強的會話模式可能會造成安全性和隱私權風險。 如果使用者透過 VMConnect 連線並登入虛擬機器，而另一個已授權的使用者連接到相同的虛擬機器，則會話會被第二個使用者接管，而第一個使用者將會遺失會話。 第二個使用者將能夠查看第一個使用者的桌面、檔和應用程式。|
|管理可讓 VM 與 Hyper-v 主機通訊的整合服務或元件| 在執行 Windows 10 或 Windows Server 2016 的 Hyper-v 主機上，您無法使用 VMConnect 來管理 integration services。 請參閱下列主題： <br />- [開啟/關閉 Hyper-v 主機的整合服務](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [開啟/關閉 Windows 虛擬機器中的 integration services](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [開啟/關閉 Linux 虛擬機器的 integration services](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [為虛擬機器保持整合服務的更新](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />如需執行 Windows Server 2012 或 Windows Server 2012 R2 的主機，請參閱[Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx)。|
|調整 VMConnect 視窗的大小|您可以變更執行 Windows 作業系統之第2代虛擬機器的 VMConnect 視窗大小。 若要這麼做，您可能需要在 Hyper-v 主機上開啟加強的會話模式。 如需詳細資訊，請參閱在[hyper-v 主機上開啟加強的會話模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host)。 如需執行 Ubuntu 的虛擬機器，請參閱[變更 HYPER-V VM 中的 Ubuntu 螢幕解析度](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。|


## <a name="keyboard-shortcuts"></a>鍵盤快速鍵  
根據預設，鍵盤輸入和滑鼠點按動作會傳送至虛擬機器。 因此，在使用下列快速鍵之前，您可能需要按下 CTRL + ALT + 向左鍵。 

|按鍵組合|描述|  
|-------------------|---------------|  
|CTRL\+ ALT\+向左箭號|滑鼠版本|  
|CTRL\+ALT\+結束|等同于虛擬\+機\+中的 CTRL ALT DELETE|  
|CTRL\+ALT\+BREAK|從全\-螢幕模式切換回視窗模式|  
|CTRL\+O|開啟虛擬機器的設定|  
|CTRL\+S|啟動虛擬機器|  
|CTRL\+N|建立檢查點|  
|CTRL\+E|還原為檢查點|  
|CTRL\+C|執行螢幕擷取畫面|  

## <a name="see-also"></a>另請參閱  
-   [在 Hyper-v 虛擬機器上使用本機資源搭配 VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Windows Server 2016 上的 hyper-v](../Hyper-V-on-Windows-Server.md)  
-   [Windows 10 上的 hyper-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  

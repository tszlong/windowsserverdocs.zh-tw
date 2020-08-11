---
title: Hyper-V 虛擬機器連線
description: 描述可提供虛擬機器遠端存取的虛擬機器連線。 包含如何執行一般工作的詳細資料，例如傳送 Ctrl-Alt-Delete 至虛擬機器。
manager: dongill
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 2ffe54a1699fdb23833adc5e0036747d516e3056
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990175"
---
# <a name="hyper-v-virtual-machine-connection"></a>Hyper-V 虛擬機器連線

>適用於：Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

您可以使用虛擬機器連線 \(VMConnect\) 工具來連線到虛擬機器，如此即可在虛擬機器中安裝客體作業系統或與之互動。 下列是可以使用 VMConnect 執行的部分工作：

-   啟動和關閉虛擬機器

-   連線到 DVD 映像 \(.iso 檔案\) 或 USB 快閃磁碟機

-   建立檢查點

-   修改虛擬機器的設定

## <a name="tips-for-using-vmconnect"></a>使用 VMConnect 的秘訣
您可能會發現下列資訊有助於使用 VMConnect：

|若要進行此動作...|請執行此動作...|
|---------------|------------|
|將滑鼠點按動作或鍵盤輸入傳送至虛擬機器|按一下虛擬機器視窗中的任何位置。 當您連線到執行中的虛擬機器時，滑鼠指標可能會顯示為小點。|
|使滑鼠點按動作或鍵盤輸入返回實體電腦|按 CTRL\+ALT\+向左鍵，然後將滑鼠指標移至虛擬機器視窗外部。 您可以在 Hyper\-V 管理員中的 Hyper\-V 設定中變更此滑鼠釋放鍵組合。|
|將 CTRL\+ALT\+DELETE 按鍵組合傳送到虛擬機器|選取 [動作]   > **Ctrl\+Alt\+Delete** 或使用按鍵組合：CTRL\+ALT\+END。|
|從視窗模式切換至全螢幕模式|選取 [檢視]   > [全螢幕模式]  。 若要切換回視窗模式，請按 CTRL\+ALT\+BREAK。|
|為進行疑難排解，建立檢查點來取得機器的目前狀態|選取 [動作]   > [檢查點]  ，或使用按鍵組合：CTRL\+N。|
|變更虛擬機器的設定|選取 [檔案]   > [設定]  。|
|連線至 DVD 映像 \(.iso 檔案\)或虛擬軟式磁碟 \(.vfd 檔案\)|選取 [媒體]  。<p>第 2 代虛擬機器不支援虛擬軟式磁碟。 如需詳細資訊，請參閱[我應在 Hyper-V 建立第 1 代或第 2 代的虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)。|
|在 Hyper\-V 虛擬機器 (例如 USB 快閃磁碟機) 上使用主機的本機資源|在 Hyper-V 主機上開啟加強的工作階段模式、使用 VMConnect 來連線到虛擬機器，並在連線之前，選擇您想要使用的本機資源。 如需了解特定步驟，請參閱[使用 VMConnect 在 Hyper\-V 虛擬機器上使用本機資源](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。|
|變更虛擬機器的已儲存 VMConnect 設定|從 Windows PowerShell 或命令提示字元執行下列命令：<p>`VMConnect.exe <ServerName> <VMName> /edit`|
|防止 VMConnect 使用者接管另一位使用者的 VMConnect 工作階段|[在 Hyper-V 主機上開啟增強的工作階段模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host)。<p>未開啟增強的工作階段模式可能會造成安全性和隱私權上的風險。 如果使用者透過 VMConnect 連線並登入虛擬機器，但有另一個已授權的使用者連線到相同的虛擬機器，則工作階段會被第二個使用者接管，而第一個使用者將會失去工作階段。 第二個使用者將能夠查看第一個使用者的桌面、文件和應用程式。|
|管理可讓 VM 與 Hyper-V 主機通訊的整合服務或元件| 在執行 Windows 10 或 Windows Server 2016 的 Hyper-V 主機上，您無法使用 VMConnect 來管理整合服務。 請參閱下列主題： <br />- [從 Hyper-V 主機開啟/關閉整合服務](../manage/manage-hyper-v-integration-services.md) <br />- [從 Windows 虛擬機器開啟/關閉整合服務](../manage/manage-hyper-v-integration-services.md#start-and-stop-an-integration-service-from-a-windows-guest)<br />- [從 Linux 虛擬機器開啟/關閉整合服務](../manage/manage-hyper-v-integration-services.md#start-and-stop-an-integration-service-from-a-linux-guest) <br />- [將整合服務維持在可用於虛擬機器的最新狀態](../manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date)  <br />如需了解執行 Windows Server 2012 或 Windows Server 2012 R2 的主機，請參閱[整合服務](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11))。|
|調整 VMConnect 視窗的大小|您可以為執行 Windows 作業系統的第 2 代虛擬機器變更 VMConnect 視窗大小。 若要這麼做，您可能需要在 Hyper-V 主機上開啟增強的工作階段模式。 如需詳細資訊，請參閱[在 Hyper-V 主機上開啟增強的工作階段模式](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host)。 如需執行 Ubuntu 的虛擬機器詳細資訊，請參閱[變更 Hyper-V VM 中的 Ubuntu 螢幕解析度](/archive/blogs/virtual_pc_guy/changing-ubuntu-screen-resolution-in-a-hyper-v-vm)。|


## <a name="keyboard-shortcuts"></a>鍵盤快速鍵
根據預設，鍵盤輸入和滑鼠點按動作會傳送至虛擬機器。 因此，在使用下列快速鍵之前，您可能需要按下 CTRL + ALT + 向左鍵。

|按鍵組合|說明|
|-------------------|---------------|
|CTRL\+ALT\+向左鍵|滑鼠釋放|
|CTRL\+ALT\+END|等同於虛擬機器中的 CTRL\+ALT\+DELETE|
|CTRL\+ALT\+BREAK|從全螢幕模式切換回視窗模式|
|CTRL\+O|開啟虛擬機器的設定|
|CTRL\+S|啟動虛擬機器|
|CTRL\+N|建立檢查點|
|CTRL\+E|還原至檢查點|
|CTRL\+C|螢幕擷取|

## <a name="see-also"></a>另請參閱
-   [使用 VMConnect 在 Hyper-V 虛擬機器上使用本機資源](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
-   [Windows Server 2016 上的 Hyper-V](../Hyper-V-on-Windows-Server.md)
-   [Windows 10 上的 Hyper-V](/virtualization/hyper-v-on-windows/)
---
title: 識別並解決「遠端存取」伺服器操作問題
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: cae96d3034cb650b84b40a1a35364400b182a619
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947364"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>識別並解決「遠端存取」伺服器操作問題

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。

您可以使用下列程式來識別遠端存取服務器作業問題、其根本原因，以及修正問題所需的解決方法。

> [!NOTE]
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您是以 Administrators 群組成員的帳戶登入，而無法完成工作，請在使用 Domain Admins 群組成員的帳戶登入時，嘗試執行工作。

本主題包含有關執行下列工作的資訊：

- 模擬操作問題

- 找出作業問題，並採取矯正措施

- 還原 IP Helper 服務

### <a name="simulate-an-operations-issue"></a><a name="BKMK_Simulate"></a>模擬操作問題

> [!CAUTION]
> 因為您的遠端存取服務器可能已正確設定且未遇到任何問題，所以您可以使用下列程式來模擬操作問題。 如果您的伺服器目前在生產環境中提供服務用戶端，您可能不想要在這段時間採取這些動作。 相反地，您可以閱讀這些步驟，瞭解如何解決未來可能會在遠端存取服務器上發生的問題。

IP 協助程式服務 (IPHlpSvc) 裝載 IPv6 轉換技術 (例如 IP-HTTPS、6to4 或 Teredo) ，而且 DirectAccess 伺服器必須能夠正常運作。 若要示範遠端存取服務器上的模擬操作問題，您必須停止 (IPHlpSvc) network service。

##### <a name="to-stop-the-ip-helper-service"></a>停止 IP Helper 服務

1.  在遠端存取服務器的 [ **開始** ] 畫面上，按一下 [系統 **管理工具**]，然後按兩下 [ **服務**]。

2.  在 **服務** 清單中，向下移動並以滑鼠右鍵按一下 [ **IP Helper**]，然後按一下 [ **停止**]。

### <a name="identify-the-operations-issue-and-take-corrective-action"></a><a name="BKMK_Identify"></a>找出作業問題，並採取矯正措施
關閉 IP 協助程式服務將會在遠端存取服務器上造成嚴重的錯誤。 監視儀表板會顯示伺服器的作業狀態和問題的詳細資料。

##### <a name="to-identify-the-details-and-take-corrective-action"></a>找出詳細資料並採取更正動作

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。

3.  確定已在左窗格中選取您的遠端存取服務器，然後在中間窗格中，按一下 [ **操作狀態**]。

4.  您將會看到具有綠色或紅色圖示的元件清單，指出其操作狀態。 按一下清單中 **的 ip-HTTPs 資料列。** 當您選取資料列時，[ **詳細資料** ] 窗格中會顯示作業的詳細資料，如下所示：

    **錯誤**

    IP Helper 服務 (IPHlpSvc) 已停止。 DirectAccess 可能無法如預期般運作。 IP 協助程式服務會使用連線平臺、IPv6 轉換技術和 IP-HTTPS 來提供通道連線能力。

    **原因**

    1.  IP 協助程式服務已停止。

    2.  IP Helper 服務沒有回應。

    **解決方法**

    1.  若要確定服務正在執行，請在 Windows PowerShell 提示字元中輸入 **get-help iphlpsvc** 。

    2.  若要啟用服務，請從提高許可權的 Windows PowerShell 提示字元中輸入 **Start-service iphlpsvc** 。

    3.  若要重新開機服務，請從提高許可權的 Windows PowerShell 提示字元中，輸入 **restart-service iphlpsvc** 。

### <a name="restore-the-ip-helper-service"></a><a name="BKMK_Restart"></a>還原 IP Helper 服務
若要在您的遠端存取服務器上還原 IP 協助程式服務，您可以遵循上述的解決步驟來啟動或重新開機服務，或者您可以使用下列程式，將用來模擬 IP 協助程式服務失敗的程式復原。

##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>在遠端存取服務器上重新開機 IP Helper 服務

1.  在 [ **開始** ] 畫面上，按一下 [系統 **管理工具**]，然後按兩下 [ **服務**]。

2.  在 **服務** 清單中，向下移動並以滑鼠右鍵按一下 [ **IP Helper**]，然後按一下 [ **啟動**]。

![Windows PowerShell ](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif) * *_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property _
```

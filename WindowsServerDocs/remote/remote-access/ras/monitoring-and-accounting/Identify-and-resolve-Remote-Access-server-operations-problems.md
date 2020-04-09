---
title: 識別並解決「遠端存取」伺服器操作問題
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae48f9f59bcc297c9bcc2b65a4d094f6d0279359
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860551"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>識別並解決「遠端存取」伺服器操作問題

>適用於：Windows Server (半年通道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。  
  
您可以使用下列程式來識別遠端存取服務器作業問題、其根本原因，以及解決問題所需的解決方法。  
  
> [!NOTE]  
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您使用 Administrators 群組成員的帳戶登入時無法完成工作，請嘗試在使用屬於 Domain Admins 群組成員的帳戶登入時執行此項工作。  
  
本主題包含執行下列工作的相關資訊：  
  
- 模擬作業問題  
  
- 找出作業問題並採取矯正措施  
  
- 還原 IP Helper 服務  
  
### <a name="simulate-an-operations-issue"></a><a name="BKMK_Simulate"></a>模擬作業問題  
  
> [!CAUTION]  
> 因為您的遠端存取服務器可能已正確設定，而不會發生任何問題，所以您可以使用下列程式來模擬作業問題。 如果您的伺服器目前在生產環境中服務用戶端，您可能不想要在此時執行這些動作。 相反地，您可以仔細閱讀這些步驟，瞭解如何解決未來遠端存取服務器可能發生的問題。  
  
IP 協助程式服務（IPHlpSvc）會裝載 IPv6 轉換技術（例如 IP-HTTPS、6to4 或 Teredo），而 DirectAccess 伺服器必須能夠正常運作。 若要示範遠端存取服務器上的模擬作業問題，您必須停止（IPHlpSvc）網路服務。  
  
##### <a name="to-stop-the-ip-helper-service"></a>停止 IP Helper 服務  
  
1.  在遠端存取服務器的 [**開始**] 畫面上，按一下 [系統**管理工具**]，然後按兩下 [**服務**]。  
  
2.  在**服務**清單中，向下並以滑鼠右鍵按一下 [ **IP Helper**]，然後按一下 [**停止**]。  
  
### <a name="identify-the-operations-issue-and-take-corrective-action"></a><a name="BKMK_Identify"></a>找出作業問題並採取矯正措施  
關閉 IP 協助程式服務會在遠端存取服務器上造成嚴重的錯誤。 [監視] 儀表板會顯示伺服器的作業狀態和問題的詳細資料。  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>識別詳細資料並採取矯正措施  
  
1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。  
  
2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。  
  
3.  請確定已選取左窗格中的 [遠端存取服務器]，然後在中間窗格中，按一下 [**作業狀態**]。  
  
4.  您會看到具有綠色或紅色圖示的元件清單，表示其操作狀態。 按一下清單中的**ip-HTTPs**資料列。 當您選取資料列時，作業的詳細資料會顯示在**詳細資料**窗格中，如下所示：  
  
    **糾錯**  
  
    IP Helper 服務（IPHlpSvc）已停止。 DirectAccess 可能無法如預期般運作。 IP 協助程式服務使用連線平臺、IPv6 轉換技術和 IP-HTTPS 來提供通道連線能力。  
  
    **都會**  
  
    1.  IP 協助程式服務已停止。  
  
    2.  IP Helper 服務沒有回應。  
  
    **解決方法**  
  
    1.  若要確定服務正在執行，請在 Windows PowerShell 命令提示字元中輸入「**服務 iphlpsvc** 」。  
  
    2.  若要啟用此服務，請從提升許可權的 Windows PowerShell 命令提示字元中輸入 [**啟動-服務 iphlpsvc** ]。  
  
    3.  若要重新開機服務，請從提升許可權的 Windows PowerShell 命令提示字元中輸入**restart-服務 iphlpsvc** 。  
  
### <a name="restore-the-ip-helper-service"></a><a name="BKMK_Restart"></a>還原 IP Helper 服務  
若要在您的遠端存取服務器上還原 IP 協助程式服務，您可以遵循上述解決步驟來啟動或重新開機服務，或者您可以使用下列程式來反轉用來模擬 IP Helper 服務失敗的程式。  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>重新開機遠端存取服務器上的 IP Helper 服務  
  
1.  在 [**開始**] 畫面上，按一下 [系統**管理工具**]，然後按兩下 [**服務**]。  
  
2.  在**服務**清單中，向下並以滑鼠右鍵按一下 [ **IP Helper**]，然後按一下 [**啟動**]。  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>windows powershell 對等命令</em>***  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```

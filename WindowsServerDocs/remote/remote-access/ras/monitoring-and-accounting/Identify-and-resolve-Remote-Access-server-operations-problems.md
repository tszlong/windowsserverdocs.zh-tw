---
title: 識別並解決「遠端存取」伺服器操作問題
description: 本主題是適用於遠端存取 」 監視和指南 Windows Server 2016 中的帳戶處理的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b682685883a2200caf8f4286674bb3e2cbe6651b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282776"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>識別並解決「遠端存取」伺服器操作問題

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 以及「路由及遠端存取服務」(RRAS) 合併成一個遠端存取角色。  
  
您可以使用下列程序來識別遠端存取 」 伺服器操作問題，其根本原因和修正問題所需的解析度。  
  
> [!NOTE]  
> 您必須登入以 Domain Admins 群組的成員或每一部電腦上的 Administrators 群組的成員才能完成本主題中所述的工作。 如果您在登入是系統管理員群組成員的帳戶，您無法完成工作，請嘗試執行工作，您在登入的帳戶是 Domain Admins 群組的成員。  
  
本主題包含有關執行下列工作：  
  
- 模擬的作業問題  
  
- 找出操作問題並採取更正動作  
  
- 還原 IP 協助程式服務  
  
### <a name="BKMK_Simulate"></a>模擬的作業問題  
  
> [!CAUTION]  
> 因為您可能是伺服器的遠端存取設定正確，並不會遇到任何問題，您可以使用下列程序來模擬的作業問題。 如果您的伺服器目前正在服務在生產環境中的用戶端，您可能不想在這個階段中執行這些動作。 相反地，您也可以閱讀以了解如何解決問題，未來可能發生在遠端存取伺服器的步驟。  
  
IP 協助程式服務 (IPHlpSvc) 主機 IPv6 轉換技術 （例如 IP-HTTPS，6to4 或 Teredo），而且很正常的 DirectAccess 伺服器需要時。 為了示範遠端存取伺服器上的模擬的作業問題，您必須停止 (IPHlpSvc) 網路服務。  
  
##### <a name="to-stop-the-ip-helper-service"></a>若要停止 IP 協助程式服務  
  
1.  上**開始**螢幕的遠端存取伺服器，按一下**系統管理工具**，然後按兩下**Services**。  
  
2.  在這份**Services**，向下捲動並以滑鼠右鍵按一下**IP 協助程式**，然後按一下**停止**。  
  
### <a name="BKMK_Identify"></a>找出操作問題並採取更正動作  
關閉 IP 協助程式服務會導致嚴重的錯誤，遠端存取伺服器上。 監視儀表板會顯示伺服器的作業狀態和問題的詳細資料。  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>若要識別詳細資料，並採取更正動作  
  
1.  在 [伺服器管理員]  中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  按一下 [儀表板]  以瀏覽到 [遠端存取管理主控台]  中的 [遠端存取儀表板]  。  
  
3.  請確定您的遠端存取伺服器已在左窗格中，選取，然後在中間窗格中，按一下**操作狀態**。  
  
4.  您會看到具有綠色或紅色圖示，指出其操作狀態的元件清單。 按一下  **IP-HTTPS**清單中的資料列。 當您選取一個資料列時，此作業的詳細資料所示**詳細資料**窗格中的，如下所示：  
  
    **錯誤**  
  
    IP 協助程式服務 (IPHlpSvc) 已停止。 DirectAccess 可能無法如預期般運作。 IP 協助程式服務會提供使用連線平台、 IPv6 轉換技術和為 IP-HTTPS 通道連線能力。  
  
    **原因**  
  
    1.  IP 協助程式服務已停止。  
  
    2.  IP 協助程式服務未在回應中。  
  
    **解決方法**  
  
    1.  若要確認服務正在執行，請輸入**Get-service iphlpsc**在 Windows PowerShell 提示字元。  
  
    2.  若要啟用服務，請輸入**Start-service iphlpsvc**從提升權限的 Windows PowerShell 提示字元。  
  
    3.  若要重新啟動服務，請輸入**Restart-service iphlpsvc**從提升權限的 Windows PowerShell 提示字元。  
  
### <a name="BKMK_Restart"></a>還原 IP 協助程式服務  
若要還原遠端存取伺服器上的 IP 協助程式服務，您可以解析上述步驟，以啟動或重新啟動服務，請依照下列，或您可以使用下列程序，若要反轉您用來模擬 IP 協助程式服務失敗的程序。  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>若要重新啟動遠端存取伺服器的 IP 協助程式服務  
  
1.  在上**開始**畫面上，按一下**系統管理工具**，然後按兩下**服務**。  
  
2.  在這份**Services**，向下捲動並以滑鼠右鍵按一下**IP 協助程式**，然後按一下 **啟動**。  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  



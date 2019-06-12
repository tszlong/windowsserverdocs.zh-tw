---
title: 使用歷程記錄資料來產生遠端用戶端的使用狀況報告
description: 本主題是適用於遠端存取 」 監視和指南 Windows Server 2016 中的帳戶處理的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa82ba8e2fc3fe19b9e73f602605d3ef76f4b9a5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446907"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>使用歷程記錄資料來產生遠端用戶端的使用狀況報告

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 以及「路由及遠端存取服務」(RRAS) 合併成一個遠端存取角色。  
  
遠端存取伺服器上的 [管理] 主控台可用來針對遠端存取伺服器的用戶端產生使用量報告。 若要產生使用量報告遠端用戶端，您會先啟用遠端存取伺服器上的計量。 產生報告之後，您可以使用監視儀表板來檢視伺服器上的負載統計資料的遠端存取伺服器上的 [管理] 主控台中提供。  
  
> [!NOTE]  
> 您必須登入以 Domain Admins 群組的成員或每一部電腦上的 Administrators 群組的成員才能完成本主題中所述的工作。 如果您在登入是系統管理員群組成員的帳戶，您無法完成工作，請嘗試執行工作，您在登入的帳戶是 Domain Admins 群組的成員。  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>若要啟用遠端存取伺服器上的計量  
  
1.  在 [伺服器管理員]  中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  按一下  **REPORTING**瀏覽至**遠端存取報告**中**遠端存取管理主控台**。  
  
3.  按一下 **設定 Accounting**中**遠端存取報告**工作窗格。  
  
4.  選取 **使用收件匣計量**核取方塊以啟用遠端存取伺服器上的帳戶處理。  
  
5.  按一下 **套用**以啟用帳戶處理伺服器上的設定，然後按一下**關閉**伺服器已成功套用設定之後。  
  
#### <a name="to-generate-the-usage-report"></a>若要產生使用量報告  
  
1.  在 [伺服器管理員]  中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  按一下  **REPORTING**瀏覽至**遠端存取報告**中**遠端存取管理主控台**。  
  
3.  在中間窗格中，按一下行事曆中的日期選取的報表時間長度**開始日期：** 並**結束日期：** ，然後按一下**產生報告**。  
  
4.  您會看到已連線到遠端存取伺服器上，選取的時間和其相關的詳細統計資料內的使用者清單。 按一下清單中的第一個資料列。 當您選取一個資料列時，遠端使用者活動會顯示在 [預覽] 窗格中。 現在，選取**伺服器載入統計資料** 索引標籤以查看歷程記錄的負載在伺服器上的 預覽 窗格中。  
  
    按一下 [**伺服器載入統計資料**] 索引標籤以查看歷程記錄的負載在伺服器上的 [預覽] 窗格中。  
  
> [!NOTE]  
> **了解工作階段**  
>   
> 遠端存取計量為基礎的概念**工作階段**。 相對於**連接**，則**工作階段**以遠端用戶端 IP 位址和使用者名稱的組合唯一識別。 比方說，如果電腦通道由遠端用戶端，其名稱是 Client1，一個工作階段將會建立並儲存在計量資料庫。 當的使用者稍後具名的 User1 會從該用戶端連接會傳遞 （但電腦通道仍然處於作用中） 時，工作階段會記錄為個別的工作階段。 工作階段的差別是保留電腦通道與使用者通道之間的差異。  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
在下列程式碼變更，您想在報表的日期範圍 **-StartDateTime**並 **-EndDateTime**參數。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  



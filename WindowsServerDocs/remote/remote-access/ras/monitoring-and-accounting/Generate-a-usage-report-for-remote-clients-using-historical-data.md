---
title: 使用歷程記錄資料來產生遠端用戶端的使用狀況報告
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b8dafad68546cfe20ab1e7704b60694aea4d44e0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947374"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>使用歷程記錄資料來產生遠端用戶端的使用狀況報告

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與「路由及遠端存取服務」(RRAS) 整合成一個遠端存取角色。

您可以使用遠端存取服務器上的管理主控台，為存取伺服器的遠端用戶端產生使用方式報告。 若要產生遠端用戶端的使用方式報告，您必須先在遠端存取服務器上啟用帳戶處理。 產生報告之後，您可以使用遠端存取服務器上管理主控台所提供的 [監視] 儀表板，來查看伺服器上的負載統計資料。

> [!NOTE]
> 您必須以 Domain Admins 群組成員或每部電腦上 Administrators 群組成員的身分登入，才能完成本主題中所述的工作。 如果您是以 Administrators 群組成員的帳戶登入，而無法完成工作，請在使用 Domain Admins 群組成員的帳戶登入時，嘗試執行工作。

#### <a name="to-enable-accounting-on-the-remote-access-server"></a>在遠端存取服務器上啟用帳戶處理

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [**報告**]，以流覽至 [**遠端存取管理] 主控台** 中的 [**遠端存取報告**]。

3.  按一下 [**遠端存取報告**] 工作窗格中的 [**設定帳戶** 處理]。

4.  選取 [ **使用收件匣帳戶** 處理] 核取方塊，在遠端存取服務器上啟用帳戶處理。

5.  按一下 [套用] 以啟用伺服器上的帳戶處理設定，然後在伺服器成功套用設定 **之後按一下 [** **關閉** ]。

#### <a name="to-generate-the-usage-report"></a>若要產生使用量報告

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [**報告**]，以流覽至 [**遠端存取管理] 主控台** 中的 [**遠端存取報告**]。

3.  在中間窗格中，按一下行事曆中的 [日期]，以選取 [報告持續時間 **開始日期** ] 和 [ **結束日期：**]，然後按一下 [ **產生報告**]。

4.  您會看到所選時間內已連線到遠端存取服務器的使用者清單，以及這些使用者的詳細統計資料。 按一下清單中的第一個資料列。 當您選取資料列時，[遠端使用者] 活動會顯示在預覽窗格中。 現在，在 [預覽] 窗格中選取 [ **伺服器負載統計資料** ] 索引標籤，以查看伺服器上的歷程記錄負載。

    按一下 [預覽] 窗格中的 [ **伺服器負載統計資料** ] 索引標籤，以查看伺服器上的歷程記錄負載。

> [!NOTE]
> **瞭解會話**
>
> 遠端存取帳戶處理是以 **會話** 的概念為基礎。 相對 **于連線，****會話** 是由遠端用戶端 IP 位址和使用者名稱的組合來唯一識別。 例如，如果電腦通道是由名為 Client1 的遠端用戶端所組成，則會建立一個會話，並將其儲存在帳戶處理資料庫中。 當名為 User1 的使用者在經過一段時間之後從該用戶端連線 (但電腦通道仍在使用中) 時，會將會話記錄為個別的會話。 會話的差異在於保留電腦通道與使用者通道之間的區別。

![Windows PowerShell ](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif) * *_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

在下列腳本中，于 _ *-StartDateTime** 和 **-EndDateTime** 參數中變更您要報告的日期範圍。

```
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"
Shows server load statistics.
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"
```




---
title: 針對 DHCP 用戶端上的問題進行疑難排解
description: 本 artilce 介紹如何疑難排解 DHCP 用戶端上的問題並收集資料。
ms.service: na
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 650b3f83ebd0467df2a747d865db2d0a346bcddc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954575"
---
# <a name="troubleshoot-problems-on-the-dhcp-client"></a>針對 DHCP 用戶端上的問題進行疑難排解

本文討論如何疑難排解 DHCP 用戶端上發生的問題。

## <a name="troubleshooting-checklist"></a>疑難排解檢查清單

檢查下列裝置和設定：

  - 纜線已連線且運作正常。

  - 已在用戶端連線的交換器上啟用 MAC 篩選。

  - 網路介面卡已啟用。

  - 已安裝並更新正確的網路介面卡驅動程式。

  - DHCP 用戶端服務已啟動且正在執行。 若要核取此選項，請執行**net start**命令，並尋找**DHCP 用戶端**。

  - 用戶端電腦上沒有防火牆封鎖埠67和 68 UDP。

## <a name="event-logs"></a>事件記錄檔

檢查 Microsoft-Windows DHCP 用戶端事件/操作和 Microsoft Windows DHCP 用戶端事件/系統管理事件記錄檔。 所有與 DHCP 用戶端服務相關的事件都會傳送至這些事件記錄檔。
Microsoft-Windows DHCP 用戶端事件位於 [**應用程式及服務記錄**檔] 下的 [事件檢視器。

"Get-netadapter-IncludeHidden" PowerShell 命令會提供必要的資訊，以解讀記錄中列出的事件。 例如，介面識別碼、MAC 位址等等。

## <a name="data-collection"></a>資料集合

我們建議您在發生問題時，同時在 DHCP 用戶端和伺服器端同時收集資料。 不過，視實際的問題而定，您也可以使用 DHCP 用戶端或 DHCP 伺服器上的單一資料集開始進行調查。

若要從伺服器和受影響的用戶端收集資料，請使用[Wireshark](https://www.wireshark.org/download.html)。 在 DHCP 用戶端和 DHCP 伺服器電腦上同時開始收集。

在遇到問題的用戶端上執行下列命令：

```console
ipconfig /release
ipconfig /renew
```

然後，在用戶端和伺服器上停止 Wireshark。 檢查產生的追蹤。 至少應該告訴您通訊停止的階段。
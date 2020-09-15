---
title: 針對 DHCP 用戶端上的問題進行疑難排解
description: 本 artilce 介紹如何針對 DHCP 用戶端上的問題進行疑難排解，並收集資料。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: d7cfe92272ad65ca4b413eb91039a9ab21de6c17
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078625"
---
# <a name="troubleshoot-problems-on-the-dhcp-client"></a>針對 DHCP 用戶端上的問題進行疑難排解

本文討論如何針對 DHCP 用戶端上發生的問題進行疑難排解。

## <a name="troubleshooting-checklist"></a>疑難排解檢查清單

檢查下列裝置和設定：

  - 纜線已連線且正常運作。

  - 用戶端所連線的交換器上會啟用 MAC 篩選。

  - 已啟用網路介面卡。

  - 安裝並更新正確的網路介面卡驅動程式。

  - DHCP 用戶端服務已啟動且正在執行。 若要檢查這一點，請執行 **net start** 命令，並尋找 **DHCP 用戶端**。

  - 用戶端電腦上沒有防火牆封鎖埠67和 68 UDP。

## <a name="event-logs"></a>事件記錄檔

檢查 Microsoft Windows-DHCP 用戶端事件/操作和 Microsoft Windows DHCP 用戶端事件/系統管理事件記錄檔。 與 DHCP 用戶端服務相關的所有事件都會傳送至這些事件記錄檔。
Microsoft Windows DHCP 用戶端事件位於 [ **應用程式及服務記錄**檔] 下的 [事件檢視器]。

"Get-netadapter-IncludeHidden" PowerShell 命令提供必要的資訊，以解讀記錄檔中所列的事件。 例如，介面識別碼、MAC 位址等等。

## <a name="data-collection"></a>資料收集

當問題發生時，建議您同時在 DHCP 用戶端和伺服器端同時收集資料。 不過，根據實際的問題，您也可以在 DHCP 用戶端或 DHCP 伺服器上使用單一資料集來開始進行調查。

若要從伺服器和受影響的用戶端收集資料，請使用 [Wireshark](https://www.wireshark.org/download.html)。 在 DHCP 用戶端和 DHCP 伺服器電腦上同時開始收集。

在遇到問題的用戶端上執行下列命令：

```console
ipconfig /release
ipconfig /renew
```

然後，在用戶端和伺服器上停止 Wireshark。 檢查產生的追蹤。 至少應該告訴您通訊停止的階段。
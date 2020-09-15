---
title: HTTP 1.1/2 的效能微調
description: HTTP 1.1/2 的效能微調建議
ms.topic: article
ms.author: ivanpash
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 853c71aea99d296c89f772b0ddba03ed024e385a
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078075"
---
# <a name="performance-tuning-http-112"></a>效能微調 HTTP 1.1/2

HTTP/2 的目的是要改善用戶端的效能 (例如瀏覽器) 的頁面載入時間。 在伺服器上，它可能代表 CPU 成本稍微增加。 伺服器不再需要針對每個要求使用單一 TCP 連線，因此某些狀態現在將會保留在 HTTP 層中。 此外，HTTP/2 有標頭壓縮，這表示額外的 CPU 負載。

有些情況需要 HTTP/1.1 回復 (重設 HTTP/2 連線，而改為使用 HTTP/1.1) 來建立新的連接。 尤其是，TLS 重新協商和 HTTP 驗證 (基本和摘要式) 需要 HTTP/1.1 的回復。 雖然這會增加額外負荷，但這些作業已代表一些延遲，因此不會特別影響效能。

## <a name="additional-references"></a>其他參考資料
- [Web 服務器效能調整](index.md)
- [IIS 10.0 效能調整](tuning-iis-10.md)
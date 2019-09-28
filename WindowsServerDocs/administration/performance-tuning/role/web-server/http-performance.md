---
title: HTTP 1.1/2 的效能微調
description: HTTP 1.1/2 的效能微調建議
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f7d7bd5145a0804b9ec86438602dfed7c75a2b02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384974"
---
# <a name="performance-tuning-http-112"></a>效能微調 HTTP 1.1/2

HTTP/2 是用來改善用戶端的效能（例如瀏覽器上的頁面載入時間）。 在伺服器上，它可能代表 CPU 成本稍微增加。 伺服器不需要針對每個要求使用單一 TCP 連線，而其中一些狀態現在會保留在 HTTP 層中。 此外，HTTP/2 具有標頭壓縮，這代表額外的 CPU 負載。

有些情況需要 HTTP/1.1 回溯（重設 HTTP/2 連線，而改為使用 HTTP/1.1 來建立新的連線）。 特別的是，TLS 重新協商和 HTTP 驗證（基本和摘要除外）都需要 HTTP/1.1 回溯。 雖然這會增加額外負荷，但這些作業已經代表一些延遲，因此不會特別區分效能。

## <a name="see-also"></a>另請參閱
- [Web 服務器效能調整](index.md) 
- [IIS 10.0 效能調整](tuning-iis-10.md)
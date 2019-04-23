---
title: AD FS 疑難排解-循環偵測
description: 本文件說明如何疑難排解迴圈偵測
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830909"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>AD FS 疑難排解-循環偵測 
 
拒絕的有效安全性權杖和重新導向回 AD FS 持續的信賴憑證者的合作對象時，就會發生在 AD FS 中進行迴圈。

## <a name="loop-detection-cookie"></a>迴圈偵測 cookie
為了避免這種情況，AD FS 已實作所謂迴圈偵測 cookie。 根據預設，AD FS 會將 cookie 寫入名為 web 被動用戶端**MSISLoopDetectionCookie**。 此 cookie 包含時間戳記值和幾個權杖的發出的值。  這可讓追蹤許多次的用戶端頻率及如何瀏覽特定的時間範圍內的 Federation Service 的 AD FS。

如果被動用戶端造訪五 （5） 的時間內 20 秒，AD FS 權杖的 Federation Service 就會擲回下列錯誤：

**MSIS7042:已進行相同的用戶端瀏覽器工作階段 '{0}'要求在最後一個'{1}' 秒。如需詳細資訊，請連絡您的系統管理員。**

進入無限迴圈通常發生行為異常的信賴憑證者應用程式並未成功地耗用 AD FS 所發出的權杖，且應用程式正在傳送被動用戶端回 AD FS 中，重複，新權杖。  AD FS 是會發出被動用戶端新的權杖，每次只要它們不會超過 5 個要求在 20 秒內。 

## <a name="adjusting-the-loop-detection-cookie"></a>調整迴圈偵測 cookie
您可以使用 PowerShell 來變更值和時間範圍的值所簽發的權杖的數目。

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
最小值**LoopDetectionMaximumTokensIssuedInterval**為 1。

最小值**LoopDetectionTimeIntervalInSeconds**為 5。

當您在進行效能測試，您也可以停用迴圈偵測。

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>它是不建議您若要永久停用迴圈偵測，因為這可防止使用者進入無限迴圈的狀態。


## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)




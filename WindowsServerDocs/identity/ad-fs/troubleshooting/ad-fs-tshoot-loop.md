---
title: AD FS 疑難排解-迴圈偵測
description: 本檔說明如何針對迴圈偵測進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: e3d654f44ba75d9b647c0c1d9db7345c7ea75435
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953088"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>AD FS 疑難排解-迴圈偵測

當信賴憑證者持續拒絕有效的安全性權杖，並重新導向回 AD FS 時，就會發生 AD FS 中的迴圈。

## <a name="loop-detection-cookie"></a>迴圈偵測 cookie
為了避免發生這種情況，AD FS 已實作為「迴圈偵測」 cookie 的名稱。 根據預設，AD FS 會將 cookie 寫入名為**MSISLoopDetectionCookie**的 web 被動用戶端。 此 cookie 會保存時間戳記值和一些已發出的權杖值。  這可讓 AD FS 追蹤用戶端在特定時間範圍內造訪同盟服務的頻率和次數。

如果被動用戶端在20秒內造訪權杖的同盟服務五 (5) 次，AD FS 會擲回下列錯誤：

**MSIS7042：相同的用戶端瀏覽器會話 {0} 在最後 ' ' 秒內提出 ' ' 個要求 {1} 。請洽詢您的系統管理員以取得詳細資料。**

進入無限迴圈通常是因為未成功取用 AD FS 所簽發權杖的不正常信賴憑證者應用程式所造成，而應用程式會針對新的權杖重複地將被動用戶端傳送回 AD FS。  AD FS 會每次發出一個新權杖給被動用戶端，只要它們在20秒內不會超過5個要求。

## <a name="adjusting-the-loop-detection-cookie"></a>調整迴圈偵測 cookie
您可以使用 PowerShell 來變更已發出的權杖數目值和 timespan 值。

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
**LoopDetectionMaximumTokensIssuedInterval**的最小值為1。

**LoopDetectionTimeIntervalInSeconds**的最小值為5。

當您執行效能測試時，您也可以停用迴圈偵測。

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>不建議永久停用迴圈偵測，因為這會讓使用者無法進入無限迴圈狀態。


## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)




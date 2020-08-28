---
title: verifier
description: 執行驅動程式驗證器管理員的 verifier 參考文章。
ms.topic: reference
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5374fb5e9c56cc288496ff7077587b1576399d0e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023032"
---
# <a name="verifier"></a>verifier

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

驅動程式驗證器管理員。

## <a name="syntax"></a>語法
```
verifier /standard /driver <name> [<name> ...]
verifier /standard /all
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all
verifier /querysettings
verifier /volatile /flags <flags>
verifier /volatile /adddriver <name> [<name>...]
verifier /volatile /removedriver <name> [<name>...]
verifier /volatile /faults [<probability> [<tags> [<applications>]]
verifier /reset
verifier /query
verifier /log <LogFileName> [/interval <seconds>]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<flags>|必須是十進位或十六進位的數位，且位的組合：<p>-   **值：描述**<br />-   **位0：** 特殊集區檢查<br />-   **位1：強制執行** irql 檢查<br />-   **位2：** 低資源模擬<br />-   **位3：** 集區追蹤<br />-   **位4：** I/o 驗證<br />-   **位5：** 鎖死偵測<br />-   **bit 6：** 未使用<br />-   **位7：** DMA 驗證<br />-   **位8：** 安全性檢查<br />-   **位9：** 強制暫止 i/o 要求<br />-   **位10：** IRP 記錄<br />-   **位11：** 其他檢查<p>例如， **/flags 27** 等同于 **/flags 0x1b**|
|/volatile|用來動態變更驗證器設定，而不需要重新開機系統。 當系統重新開機時，將會遺失任何新的設定。|
|\<probability>|介於1和10000之間的數位，指定錯誤插入機率。 例如，指定100表示 1% (100/10000) 的錯誤插入機率。<p>如果未指定此參數，則會使用預設機率6%。|
|\<tags>|指定將使用錯誤插入的集區標籤，並以空白字元分隔。 如果未指定此參數，則任何集區配置都可以插入錯誤。|
|\<applications>|指定要插入錯誤的應用程式的影像檔案名稱，並以空白字元分隔。 如果未指定此參數，則可能會在任何應用程式中進行低資源模擬。|
|\<minutes>|正數，指定在重新開機後的期間長度（以分鐘為單位），在這段期間內不會發生任何錯誤插入。 如果未指定此參數，則會使用預設長度8分鐘。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
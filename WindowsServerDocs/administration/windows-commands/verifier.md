---
title: verifier
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865869"
---
# <a name="verifier"></a>verifier

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

驅動程式檢查器管理員。  

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
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|\<flags>|必須是數字的位元的十進位或十六進位，組合：<br /><br />-   **值： 描述**<br />-   **位元 0:** 特殊的集區檢查<br />-   **位元 1:** 強制 irql 檢查<br />-   **位元 2:** 低資源模擬<br />-   **位元 3:** 追蹤的集區<br />-   **位元 4:** I/O 驗證<br />-   **位元 5:** 死結偵測<br />-   **位元 6:** 未使用<br />-   **位元 7:** DMA 驗證<br />-   **位元 8:** 安全性檢查<br />-   **位元 9:** 強制暫止 I/O 要求<br />-   **位元 10:** IRP 記錄<br />-   **位元 11:** 其他檢查<br /><br />例如， **/27 加上旗標**相當具有  **/0x1B 加上旗標**|  
|/volatile|用來動態變更的驗證器設定，而不重新啟動系統。 重新啟動系統時，將會遺失任何新的設定。|  
|\<機率 >|介於 1 與指定的錯誤資料隱碼攻擊的機率的 10,000 之間的數字。 例如，指定 100 所表示的錯誤資料隱碼攻擊機率約為 1%（100/10,000 個）。<br /><br />如果未指定此參數，則會使用預設機率的 6%。|  
|\<tags>|指定將插入的集區標記具有以空白字元分隔的瑕疵。 如果沒有指定這個參數是任何集區配置可以插入與錯誤。|  
|\<applications>|指定將插入的應用程式的映像檔案名稱具有空格字元所隔開的瑕疵。 如果未指定此參數低資源模擬可能需要在任何應用程式的位置。|  
|\<minutes>|指定的長度超過此時限後重新開機，以分鐘為單位，插入式攻擊會發生的任何錯誤期間的正數。 如果未指定此參數，然後將使用 8 分鐘的預設長度。|  
|/?|在命令提示字元顯示說明。|  

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
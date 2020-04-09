---
title: cacls
description: Cacls 的 Windows 命令主題，它會顯示或修改指定檔案上的任意存取控制清單（DACL）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8cc6300563880a587b6544ec8cb61e9010ad6c2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848241"
---
# <a name="cacls"></a>cacls

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示或修改指定檔案上的任意存取控制清單（DACL）。  

## <a name="syntax"></a>語法  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
#### <a name="parameters"></a>參數  

|        參數        |                                                                                            描述                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<filename\>       |                                                                            必要。 顯示指定檔案的 Acl。                                                                             |
|           /t            |                                                          變更目前目錄和所有子目錄中指定檔案的 Acl。                                                          |
|           /m            |                                                                          變更掛接至目錄之磁片區的 Acl。                                                                           |
|           /l            |                                                                        使用符號連結本身與目標。                                                                         |
|         /s： sddl         |                                       以 SDDL 字串中指定的 Acl （不是有效的 **/e**、 **/g**、 **/r**、 **/p**或 **/d**）來取代 acl。                                        |
|           /e            |                                                                                 編輯 ACL，而不是取代它。                                                                                  |
|           /c            |                                                                                 在拒絕存取錯誤時繼續。                                                                                  |
|    /g user：\<永久\>     |   授與指定的使用者存取權限。<p>許可權的有效值：<p>-n-無<br />-r-讀取<br />-w-寫入<br />-c-變更（寫入）<br />-f-完全控制   |
|      /r 使用者 [...]      |                                                                  撤銷指定的使用者存取權限（僅適用于 **/e**）。                                                                   |
| [/p user：\<永久\> [...] | 取代指定的使用者存取權限。<p>許可權的有效值：<p>-n-無<br />-r-讀取<br />-w-寫入<br />-c-變更（寫入）<br />-f-完全控制 |
|     [/d user [...]      |                                                                                    拒絕指定的使用者存取權。                                                                                     |
|           /?            |                                                                                在命令提示字元顯示說明。                                                                                |

## <a name="remarks"></a>備註  
- 此命令已被取代。 請改用[icacls](icacls.md) 。  
- 使用下表來解讀結果：  


  |      輸出       |                存取控制專案（ACE）適用于                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               物件繼承。 此資料夾和檔案。                |
  |        CI         |           容器繼承。 這個資料夾和子資料夾。            |
  |        IO         | 僅繼承。 ACE 不會套用至目前的檔案/目錄。 |
  | 沒有輸出訊息 |                          僅此資料夾。                          |
  |     OICI      |                 這個資料夾、子資料夾和檔案。                 |
  |   OICI輸出    |                     僅限子資料夾和檔案。                      |
  |     CI輸出      |                          僅限子資料夾。                           |
  |     OI輸出      |                             僅限檔案。                             |


- 您可以使用萬用字元（ **？** 和 **\\\*** ）來指定多個檔案。  
- 您可以指定一個以上的使用者。  

## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)   
-   [icacls](icacls.md)  

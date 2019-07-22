---
title: cacls
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f620a417f9d7bd06f779802e684e0196efc6a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434597"
---
# <a name="cacls"></a>cacls

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示或修改指定的檔案上的判別存取控制清單 (DACL)。  
## <a name="syntax"></a>語法  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>參數  

|        參數        |                                                                                            描述                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<filename\>       |                                                                            必要。 顯示指定檔案的 Acl。                                                                             |
|           /t            |                                                          變更目前的目錄和所有子目錄中的指定檔案的 Acl。                                                          |
|           /m            |                                                                          變更 Acl 的磁碟區掛接至目錄。                                                                           |
|           /l            |                                                                        適用於符號連結本身與目標。                                                                         |
|         /s:sddl         |                                       以 SDDL 字串中指定的取代 Acl (具有不正確 **/e**， **/g**， **/r**， **/p**，或 **/d**).                                        |
|           /e            |                                                                                 編輯 ACL，而不是取代它。                                                                                  |
|           /c            |                                                                                 拒絕存取錯誤時仍然繼續。                                                                                  |
|    /g 使用者：\<為永久\>     |   授與指定的使用者存取權限。<br /><br />有效權限的值：<br /><br />-n-none<br />-r-讀取<br />-w-寫入<br />-c-變更 （寫入）<br />-f-完全控制   |
|      /r 使用者 [...]      |                                                                  撤銷指定的使用者存取權限 (只適用於 **/e**)。                                                                   |
| [/p user:\<perm\> [...] | 取代指定之的使用者的存取權限。<br /><br />有效權限的值：<br /><br />-n-none<br />-r-讀取<br />-w-寫入<br />-c-變更 （寫入）<br />-f-完全控制 |
|     [/d 使用者 [...]      |                                                                                    拒絕指定的使用者存取。                                                                                     |
|           /?            |                                                                                在命令提示字元顯示說明。                                                                                |

## <a name="remarks"></a>備註  
- 此命令已被取代。 請改用[icacls](icacls.md)改。  
- 您可以使用下表來解譯結果：  


  |      輸出       |                適用於存取控制項目 (ACE)                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               物件繼承。 這個資料夾及檔案。                |
  |        CI         |           容器繼承。 這個資料夾及子資料夾。            |
  |        IO         | 只會繼承。 ACE 不適用於目前的檔案/目錄中。 |
  | 沒有輸出訊息 |                          只有這個資料夾。                          |
  |     (OI)(CI)      |                 這個資料夾、 子資料夾和檔案。                 |
  |   (OI)(CI)(IO)    |                     子資料夾及檔案。                      |
  |     (CI)(IO)      |                          僅限子資料夾。                           |
  |     (OI)(IO)      |                             僅限檔案。                             |


- 您可以使用萬用字元 (**嗎？** 和 **\\** *) 來指定多個檔案。  
- 您可以指定多個使用者。  

#### <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)   
-   [icacls](icacls.md)  

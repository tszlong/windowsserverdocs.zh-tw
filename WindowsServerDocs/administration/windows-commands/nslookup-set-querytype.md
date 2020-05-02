---
title: nslookup set querytype
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc992d83de8537c285b6d2d97e5f44545e2f930f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723599"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更查詢的資源記錄類型。
## <a name="syntax"></a>語法
```
set querytype=<ResourceRecordtype>
```
### <a name="parameters"></a>參數
<ResourceRecordtype>指定 DNS 資源記錄類型。 預設的資源記錄類型為。下表列出此命令的有效值。

| 值 |                                                   描述                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      指定電腦&#39;的 IP 位址                                      |
|  ANY  |                                     指定電腦&#39;的 IP 位址。                                      |
| CNAME |                                    指定別名的標準名稱。                                     |
|  GID  |                                  指定組名的群組識別碼。                                  |
| HINFO |                          指定電腦&#39;s CPU 和作業系統類型。                           |
|  MB   |                                        指定信箱的功能變數名稱。                                         |
|  MG   |                                         指定郵件群組成員。                                          |
| MINFO |                                   指定信箱或郵寄清單資訊。                                   |
|  MR   |                                     指定郵件重新命名功能變數名稱。                                      |
|  MX   |                                          指定郵件交換器。                                          |
|  NS   |                                 指定命名區域的 DNS 名稱伺服器。                                 |
|  PTR  | 如果查詢是 IP 位址，則指定電腦名稱稱;否則，指定其他資訊的指標。 |
|  SOA  |                                指定 DNS 區域的啟動許可權。                                 |
|  TXT  |                                         指定文字資訊。                                         |
|  UID  |                                         指定使用者識別碼。                                          |
| UINFO |                                         指定使用者資訊。                                         |
|  WKS  |                                         描述知名的服務。                                         |
| {說明 |                                                       ?}                                                        |

顯示<strong>nslookup</strong>子命令的簡短摘要
## <a name="remarks"></a>備註
- <strong>Set type</strong>命令會執行與<strong>set querytype</strong>命令相同的功能。
- 如需資源記錄類型的詳細資訊，請參閱徵求意見（Rfc）1035。
  ## <a name="additional-references"></a>其他參考
  <href = 命令列語法 key.md 的資料-原始來源 =-[命令列](command-line-syntax-key.md)語法索引鍵>命令列語法索引鍵</a> <href = nslookup-type.md 資料-原始來源 =[nslookup 集合類型](nslookup-set-type.md)>nslookup 設定類型</a>

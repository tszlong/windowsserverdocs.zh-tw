---
title: nslookup set type
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3f41cb6bc5117fdd26bba85c6cfd806414bbab4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372885"
---
# <a name="nslookup-set-type"></a>nslookup set type

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更查詢的資源記錄類型。
## <a name="syntax"></a>語法
```
set type=<ResourceRecordtype>
```
## <a name="parameters"></a>參數
<ResourceRecordtype>指定 DNS 資源記錄類型。 預設的資源記錄類型為。下表列出此命令的有效值。

| 值 |                                                   描述                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      指定電腦&#39;的 IP 位址                                      |
|  任何  |                                     指定電腦&#39;的 IP 位址。                                      |
| CNAME |                                    指定別名的標準名稱。                                     |
|  GID  |                                  指定組名的群組識別碼。                                  |
| HINFO |                          指定電腦&#39;的 CPU 和類型的作業系統。                           |
|  MB   |                                        指定信箱的功能變數名稱。                                         |
|  MG   |                                         指定郵件群組成員。                                          |
| MINFO |                                   指定信箱或郵寄清單資訊。                                   |
|  MR   |                                     指定郵件重新命名功能變數名稱。                                      |
|  MX   |                                          指定郵件交換器。                                          |
|  NS   |                                 指定命名區域的 DNS 名稱伺服器。                                 |
|  指標  | 如果查詢是 IP 位址，則指定電腦名稱稱;否則，指定其他資訊的指標。 |
|  SOA  |                                指定 DNS 區域的啟動許可權。                                 |
|  TXT  |                                         指定文字資訊。                                         |
|  UID  |                                         指定使用者識別碼。                                          |
| UINFO |                                         指定使用者資訊。                                         |
|  WKS  |                                         描述知名的服務。                                         |
| {說明 |                                                       ?}                                                        |

顯示<strong>nslookup</strong>子命令的簡短摘要。
## <a name="remarks"></a>備註
- <strong>Set type</strong>命令會執行與<strong>set querytype</strong>命令相同的功能。
- 如需資源記錄類型的詳細資訊，請參閱徵求意見（Rfc）1035。
  ## <a name="additional-references"></a>其他參考
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">命令列語法索引鍵</a>
   <a href="nslookup-set-querytype.md" data-raw-source="[nslookup set querytype](nslookup-set-querytype.md)">nslookup set querytype</a>

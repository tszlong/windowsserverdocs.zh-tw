---
title: nslookup set querytype
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5af54ac5-fc1a-4af6-977b-f8e97c8eba90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0015db716bd8c74bc4366063009bda41d338d19
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436735"
---
# <a name="nslookup-set-querytype"></a>nslookup set querytype

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更查詢的資源記錄類型。
## <a name="syntax"></a>語法
```
set querytype=<ResourceRecordtype>
```
## <a name="parameters"></a>參數
<ResourceRecordtype> 指定的 DNS 資源記錄類型。 預設資源記錄類型為 a。下表列出有效的值，這個命令。

| 值 |                                                   描述                                                   |
|-------|-----------------------------------------------------------------------------------------------------------------|
|   A   |                                      指定的電腦名稱&#39;IP 位址                                      |
|  任何  |                                     指定的電腦名稱&#39;IP 位址。                                      |
| CNAME |                                    指定別名的正式名稱。                                     |
|  GID  |                                  指定群組名稱的群組識別碼。                                  |
| HINFO |                          指定的電腦名稱&#39;的 CPU 和作業系統的類型。                           |
|  MB   |                                        指定的信箱的網域名稱。                                         |
|  MG   |                                         指定郵件的群組成員。                                          |
| MINFO |                                   指定信箱或郵件清單資訊。                                   |
|  MR   |                                     指定電子郵件重新命名網域名稱。                                      |
|  MX   |                                          指定郵件交換程式。                                          |
|  NS   |                                 指定的具名區域的 DNS 名稱伺服器。                                 |
|  PTR  | 指定的電腦名稱，如果查詢是 IP 位址;否則，請指定其他資訊的指標。 |
|  SOA  |                                指定開始授權的 DNS 區域。                                 |
|  TXT  |                                         指定的文字資訊。                                         |
|  UID  |                                         指定的使用者識別碼。                                          |
| UINFO |                                         指定的使用者資訊。                                         |
|  WKS  |                                         描述已知的服務。                                         |
| {說明 |                                                       ?}                                                        |

顯示的簡短摘要<strong>nslookup</strong>子命令
## <a name="remarks"></a>備註
- <strong>設定類型</strong>命令會執行相同的功能<strong>設定 querytype</strong>命令。
- 如需資源記錄類型的詳細資訊，請參閱 < 註解 (Rfc) 1035年的要求。
  ## <a name="additional-references"></a>其他參考資料
  <a href="command-line-syntax-key.md" data-raw-source="[Command-Line Syntax Key](command-line-syntax-key.md)">命令列語法重點</a>
  <a href="nslookup-set-type.md" data-raw-source="[nslookup set type](nslookup-set-type.md)">nslookup 設定類型</a>

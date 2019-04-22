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
ms.openlocfilehash: 5d698e6d4603afb332efeaf1cdc79eeeee37d66d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813119"
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
|值|描述|
|-----|--------|
|A|指定電腦的 IP 位址|
|任何|指定電腦的 IP 位址。|
|CNAME|指定別名的正式名稱。|
|GID|指定群組名稱的群組識別碼。|
|HINFO|指定電腦的 CPU 和作業系統的類型。|
|MB|指定的信箱的網域名稱。|
|MG|指定郵件的群組成員。|
|MINFO|指定信箱或郵件清單資訊。|
|MR|指定電子郵件重新命名網域名稱。|
|MX|指定郵件交換程式。|
|NS|指定的具名區域的 DNS 名稱伺服器。|
|PTR|指定的電腦名稱，如果查詢是 IP 位址;否則，請指定其他資訊的指標。|
|SOA|指定開始授權的 DNS 區域。|
|TXT|指定的文字資訊。|
|UID|指定的使用者識別碼。|
|UINFO|指定的使用者資訊。|
|WKS|描述已知的服務。|
{說明 | ?}
顯示的簡短摘要**nslookup**子命令
## <a name="remarks"></a>備註
-   **設定類型**命令會執行相同的功能**設定 querytype**命令。
-   如需資源記錄類型的詳細資訊，請參閱 < 註解 (Rfc) 1035年的要求。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定類型](nslookup-set-type.md)

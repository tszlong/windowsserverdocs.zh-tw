---
title: Sc delete
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b60127cf957a30d147c9992c74c01e37e5b8bf89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871919"
---
# <a name="sc-delete"></a>Sc delete



從登錄刪除一個服務子機碼。 如果服務正在執行，或另一個處理序已開啟的控制代碼給服務，服務會標示為刪除。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定由服務所在的遠端伺服器的名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如\\ \\myserver)。 若要在本機執行 SC.exe，省略這個參數。|
|\<ServiceName>|指定所傳回的服務名稱**getkeyname**作業。|
|?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

使用**新增或移除程式**上**控制台**刪除 DHCP、 DNS 或任何其他內建的作業系統服務。 請注意，**新增或移除程式**才不會移除服務，登錄子機碼，但它也會解除安裝服務，並刪除任何捷徑。

## <a name="BKMK_examples"></a>範例

若要刪除的服務子機碼**NewServ**從本機電腦上登錄中，輸入：
```
sc delete newserv
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
---
title: query
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d89ae8c7c526bce396b2583abc1728456f7bcc3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836821"
---
# <a name="query"></a>query

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示處理常式、會話和遠端桌面工作階段主機（RD 工作階段主機）伺服器的相關資訊。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
query process
query session
query termserver
query user
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[查詢進程](query-process.md)|顯示在 rd 工作階段主機伺服器上執行之處理常式的相關資訊。|
|[查詢會話](query-session.md)|顯示 rd 工作階段主機伺服器上會話的相關資訊。|
|[查詢 termserver](query-termserver.md)|顯示網路上所有 rd 工作階段主機伺服器的清單。|
|[查詢使用者](query-user.md)|顯示 rd 工作階段主機伺服器上使用者會話的相關資訊。|

## <a name="additional-references"></a>其他參考資料
- [命令列語法金鑰](command-line-syntax-key.md)
[遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)

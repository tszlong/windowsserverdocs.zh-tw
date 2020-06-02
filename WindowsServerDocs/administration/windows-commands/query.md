---
title: 查詢
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dcea0fa4ea91de56e81c51bf9fe87ec7e3a49fa
ms.sourcegitcommit: 4894649cc47dfa535306cc334871f81155198f76
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84254711"
---
# <a name="query"></a>查詢

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示處理常式、會話和遠端桌面工作階段主機（RD 工作階段主機）伺服器的相關資訊。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Microsoft Docs Windows Server Library 中的[Windows server 遠端桌面服務的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11))。

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
|[查詢處理序](query-process.md)|顯示在 rd 工作階段主機伺服器上執行之處理常式的相關資訊。|
|[query session](query-session.md)|顯示 rd 工作階段主機伺服器上會話的相關資訊。|
|[查詢 termserver](query-termserver.md)|顯示網路上所有 rd 工作階段主機伺服器的清單。|
|[query user](query-user.md)|顯示 rd 工作階段主機伺服器上使用者會話的相關資訊。|

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

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
ms.openlocfilehash: 90ef2cc14ef0131978956de8df029eaf04baabd3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722681"
---
# <a name="query"></a>查詢

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
|[查詢處理序](query-process.md)|顯示在 rd 工作階段主機伺服器上執行之處理常式的相關資訊。|
|[query session](query-session.md)|顯示 rd 工作階段主機伺服器上會話的相關資訊。|
|[查詢 termserver](query-termserver.md)|顯示網路上所有 rd 工作階段主機伺服器的清單。|
|[query user](query-user.md)|顯示 rd 工作階段主機伺服器上使用者會話的相關資訊。|

## <a name="additional-references"></a>其他參考
- [命令列語法金鑰](command-line-syntax-key.md)
[遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)

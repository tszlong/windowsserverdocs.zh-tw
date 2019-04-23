---
title: 查詢
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebe10bb78a6a901871a75e8533b3389c38060666
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863149"
---
# <a name="query"></a>查詢

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示處理程序、 工作階段和遠端桌面工作階段主機 （RD 工作階段主機） 伺服器的相關資訊。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
query process
query session
query termserver
query user
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[查詢處理序](query-process.md)|顯示在 rd 工作階段主機伺服器執行的處理序的相關資訊。|
|[查詢工作階段](query-session.md)|在 rd 工作階段主機伺服器上，會顯示有關工作階段的資訊。|
|[查詢 termserver](query-termserver.md)|顯示在網路上的所有 rd 工作階段主機伺服器的清單。|
|[查詢使用者](query-user.md)|在 rd 工作階段主機伺服器上，會顯示關於使用者工作階段的資訊。|

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

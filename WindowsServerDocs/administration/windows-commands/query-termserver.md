---
title: 查詢 termserver
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d63bd158dad74203aa7ee3fd4e43dffb97c4c873
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836921"
---
# <a name="query-termserver"></a>查詢 termserver

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示網路上所有遠端桌面工作階段主機（rd 工作階段主機）伺服器的清單。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>語法
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ### <a name="parameters"></a>參數
> 
> |    參數     |                                                                        描述                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               指定識別 rd 工作階段主機伺服器的名稱。                                               |
> | [/domain]：<Domain> | 指定要用來查詢終端機伺服器的網域。 如果您要查詢目前正在使用的網域，則不需要指定網域。 |
> |     /address     |                                                  顯示每部伺服器的網路和節點位址。                                                  |
> |    /continue     |                                              防止在每個資訊畫面顯示之後暫停。                                               |
> |        /?        |                                                            在命令提示字元顯示說明。                                                            |
> 
> ## <a name="remarks"></a>備註
> - [**查詢 termserver** ] 會在網路中搜尋所有連接的 Rd 工作階段主機伺服器，並傳回下列資訊：
>   - 伺服器的名稱
>   - 網路（以及使用/address 選項時的節點位址）
>     ## <a name="examples"></a><a name=BKMK_examples></a>典型
> - 若要顯示網路上所有 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver
>   ```
> - 若要顯示名為 Server3 的 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver Server3
>   ```
> - 若要顯示網域 CONTOSO 中所有 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - 若要顯示名為 Server3 的 rd 工作階段主機伺服器的網路和節點位址，請輸入：
>   ```
>   query termserver Server3 /address
>   ```
>   ## <a name="additional-references"></a>其他參考資料
>   - [命令列語法索引鍵](command-line-syntax-key.md)
>   [查詢](query.md)
>   [遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)

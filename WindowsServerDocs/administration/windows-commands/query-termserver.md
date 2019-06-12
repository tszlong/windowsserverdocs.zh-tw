---
title: 查詢 termserver
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14270092949baf37588059d592e6f92e694a1739
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442095"
---
# <a name="query-termserver"></a>查詢 termserver

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在網路上顯示所有的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器的清單。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
> ## <a name="syntax"></a>語法
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>參數
> 
> |    參數     |                                                                        描述                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               指定識別 rd 工作階段主機伺服器的名稱。                                               |
> | /domain:<Domain> | 指定要查詢終端機伺服器的網域。 您不需要指定網域，如果您要查詢中您目前使用的網域。 |
> |     /address     |                                                  會顯示每一部伺服器的網路和節點位址。                                                  |
> |    / 繼續     |                                              可防止暫停之後會顯示每個畫面的資訊。                                               |
> |        /?        |                                                            在命令提示字元顯示說明。                                                            |
> 
> ## <a name="remarks"></a>備註
> - **查詢 termserver**搜尋網路上的所有 rd 工作階段主機伺服器的連接，並傳回下列資訊：
>   - 伺服器的名稱
>   - 網路 （和節點位址，如果使用 /address 選項）
>     ## <a name="BKMK_examples"></a>範例
> - 若要顯示在網路上的所有 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver
>   ```
> - 若要顯示 server3 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver Server3
>   ```
> - 若要顯示在 CONTOSO 網域中的所有 rd 工作階段主機伺服器的相關資訊，請輸入：
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - 若要顯示 server3 rd 工作階段主機伺服器的網路和節點位址，請輸入：
>   ```
>   query termserver Server3 /address
>   ```
>   #### <a name="additional-references"></a>其他參考資料
>   [命令列語法重點](command-line-syntax-key.md)
>   [查詢](query.md)
>   [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

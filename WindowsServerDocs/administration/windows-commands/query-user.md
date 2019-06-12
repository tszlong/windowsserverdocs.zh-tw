---
title: 查詢使用者
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6e0d936e03e14e134c7bfd450a878fda1a796c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442007"
---
# <a name="query-user"></a>查詢使用者

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上，會顯示關於使用者工作階段的資訊。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
> ## <a name="syntax"></a>語法
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>參數
> 
> |      參數       |                                                     描述                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            指定您想要查詢之使用者的登入名稱。                             |
> |    <SessionName>     |                              指定您想要查詢之工作階段的名稱。                              |
> |     <SessionID>      |                               指定您想要查詢的工作階段識別碼。                               |
> | /server:<ServerName> | 指定您想要查詢的 rd 工作階段主機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。 |
> |          /?          |                                        在命令提示字元顯示說明。                                         |
> 
> ## <a name="remarks"></a>備註
> - 您可以使用此命令，以找出特定的使用者是否登入特定的 rd 工作階段主機伺服器。 **查詢使用者**傳回下列資訊：
>   -   使用者的名稱
>   -   在 rd 工作階段主機伺服器上的工作階段名稱
>   -   工作階段識別碼
>   -   工作階段 （作用中或已中斷連線） 狀態
>   -   閒置時間 （從最後一個按鍵或滑鼠移動後的工作階段的分鐘數）
>   -   使用者登入的時間與日期
> - 若要使用**查詢使用者**，您必須擁有完全控制權限，或查詢資訊 特殊存取權限。
> - 如果您使用**查詢使用者**但未指定 <*UserName*>，<*SessionName*>，或 <*SessionID*>，所有的清單會傳回登入伺服器的使用者。 或者，您也可以使用**查詢工作階段**來顯示伺服器上的所有工作階段的清單。
> - 當**查詢使用者**傳回資訊，大於 (>) 符號會顯示目前的工作階段之前。
> - **/Server**只有當您使用時，才是必要參數**查詢使用者**從遠端伺服器。
>   ## <a name="BKMK_examples"></a>範例
> - 若要顯示所有使用者的身分登入系統的相關資訊，請輸入：
>   ```
>   query user
>   ```
> - 若要在伺服器 SERver1 上顯示使用者 USER1 的相關資訊，請輸入：
>   ```
>   query user USER1 /server:SERver1
>   ```
>   #### <a name="additional-references"></a>其他參考資料
>   [命令列語法重點](command-line-syntax-key.md)
>   [查詢](query.md)
>   [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

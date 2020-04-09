---
title: query user
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6624c559bc85263da955f993ae7e4ad7e8b9ee2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836801"
---
# <a name="query-user"></a>query user

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機（rd 工作階段主機）伺服器上的使用者會話相關資訊。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>語法
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>參數
> 
> |      參數       |                                                     描述                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            指定您想要查詢之使用者的登入名稱。                             |
> |    <SessionName>     |                              指定您想要查詢之會話的名稱。                              |
> |     <SessionID>      |                               指定您想要查詢之會話的識別碼。                               |
> | /server:<ServerName> | 指定您想要查詢的 rd 工作階段主機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。 |
> |          /?          |                                        在命令提示字元顯示說明。                                         |
> 
> ## <a name="remarks"></a>備註
> - 您可以使用此命令來找出特定使用者是否登入特定的 rd 工作階段主機伺服器。 **查詢使用者**會傳回下列資訊：
>   -   使用者的名稱
>   -   Rd 工作階段主機伺服器上的會話名稱
>   -   會話識別碼
>   -   會話的狀態（作用中或已中斷連線）
>   -   閒置時間（自上一次按鍵或滑鼠移動到會話之後的分鐘數）
>   -   使用者登入的日期和時間
> - 若要使用**查詢使用者**，您必須具有 [完全控制] 許可權或 [查詢資訊] 特殊存取權限。
> - 如果您使用**查詢使用者**，但未指定 <*UserName*>、<*SessionName*> 或 <*SessionID*>，則會傳回所有登入伺服器的使用者清單。 或者，您也可以使用 [**查詢會話**] 顯示伺服器上所有會話的清單。
> - 當**查詢使用者**傳回信息時，會在目前的會話之前顯示大於（>）符號。
> - 只有當您使用遠端伺服器的**查詢使用者**時，才需要 **/server**參數。
>   ## <a name="examples"></a><a name=BKMK_examples></a>典型
> - 若要顯示所有登入系統之使用者的相關資訊，請輸入：
>   ```
>   query user
>   ```
> - 若要顯示伺服器 SERver1 上使用者 USER1 的相關資訊，請輸入：
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>其他參考資料
>   - [命令列語法索引鍵](command-line-syntax-key.md)
>   [查詢](query.md)
>   [遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)

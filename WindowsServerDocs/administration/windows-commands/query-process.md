---
title: 查詢處理序
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ac3e5a458d88d945e857cd1922783e5b0ff5cf0
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436283"
---
# <a name="query-process"></a>查詢處理序

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示在遠端桌面工作階段主機（rd 工作階段主機）伺服器上執行之處理常式的相關資訊。
您可以使用此命令來找出特定使用者正在執行的程式，以及哪些使用者正在執行特定程式。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>語法
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>參數
>
> |      參數       |                                                                 描述                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    列出所有會話的處理常式。                                                     |
> |     <ProcessID>      |                                   指定識別您想要查詢之進程的數位識別碼。                                   |
> |      <UserName>      |                                       指定您要列出其進程之使用者的名稱。                                       |
> |    <SessionName>     |                                     指定您想要列出其進程的會話名稱。                                      |
> |       /id<nn>       |                                      指定您要列出其進程之會話的識別碼。                                       |
> |    <ProgramName>     |                     指定您想要查詢其進程的程式名稱。 需要 .exe 副檔名。                     |
> | /server:<ServerName> | 指定要列出其進程的 rd 工作階段主機伺服器。 如果未指定，則會使用您目前登入的伺服器。 |
> |          /?          |                                                     在命令提示字元顯示說明。                                                     |
>
>#### <a name="remarks"></a>備註
> - 系統管理員具有所有**查詢處理**函式的完整存取權。
> - 如果您未指定 <*UserName*>、<*SessionName*>、 **/id：** < *nn*>、<*ProgramName*> 或 **\\** * 參數，**查詢進程**只會顯示屬於目前使用者的進程。
> - 如果指定了會話，它就必須識別使用中的會話。
> - **查詢處理**程式會傳回下列資訊：
>   -   擁有該進程的使用者
>   -   擁有進程的會話
>   -   會話的識別碼
>   -   進程的名稱
>   -   進程的識別碼
> - 當**查詢進程**傳回信息時，會在屬於目前會話的每個進程之前顯示大於（>）符號。
>   ## <a name="examples"></a>範例
> - 若要顯示所有會話正在使用之進程的相關資訊，請輸入：
>   ```
>   query process *
>   ```
> - 若要顯示會話識別碼2所使用之處理常式的相關資訊，請輸入：
>   ```
>   query process /ID:2
>   ```
>   ## <a name="additional-references"></a>其他參考
>   - [命令列語法索引鍵](command-line-syntax-key.md) 
>   [查詢](query.md) 
>   [遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)

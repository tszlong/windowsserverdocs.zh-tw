---
title: reset session
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 2f910dfc1c13b0e8555078acfb4e7ad830049592
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883667"
---
# <a name="reset-session"></a>reset session

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您在遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上，重設) 會話 (刪除。


> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11))。

## <a name="syntax"></a>語法
```
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<SessionName>|指定您想要重設的會話名稱。 若要判斷會話的名稱，請使用**查詢會話**命令。|
|\<SessionID>|指定要重設之會話的識別碼。|
|/server:\<ServerName>|指定包含您要重設之會話的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您一律可以重設自己的會話，但您必須擁有 [完全控制] 存取權限，才能重設其他使用者的會話。
-   請注意，在不發出警告的情況下重設使用者的會話可能會導致會話遺失資料。
-   只有在工作階段的功能有問題或似乎已停止回應時，才需要重設工作階段。
-   只有當您從遠端伺服器使用 [**重設會話**] 時，才需要 **/server**參數。

## <a name="examples"></a>範例
- 若要重設指定的 rdp-tcp # 6 會話，請輸入：
  ```
  reset session rdp-tcp#6
  ```
- 若要重設使用會話識別碼3的會話，請輸入：
  ```
  reset session 3
  ```

## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[遠端桌面服務 (終端機服務) 命令參考](remote-desktop-services-terminal-services-command-reference.md)

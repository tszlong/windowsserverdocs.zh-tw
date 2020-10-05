---
title: shadow
description: Shadow 命令的參考文章，可讓您從遠端控制遠端桌面工作階段主機伺服器上另一位使用者的作用中會話。
ms.topic: reference
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4689806b810fa46ccef3cd73b94c4d729c47f641
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718355"
---
# <a name="shadow"></a>shadow

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您從遠端控制遠端桌面工作階段主機伺服器上另一位使用者的作用中會話。

## <a name="syntax"></a>語法

```
shadow {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<sessionname>` | 指定您想要遠端控制的會話名稱。 |
| `<sessionID>` | 指定您想要遠端控制之會話的識別碼。 您可以使用 [ **查詢使用者** ] 來顯示會話清單及其會話識別碼。 |
| /server:`<servername>` | 指定包含您要遠端控制之會話的遠端桌面工作階段主機伺服器。 預設會使用目前的遠端桌面會話 Host4 伺服器。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您可以檢視或主動控制工作階段。 如果您選擇主動控制使用者的工作階段，即可將鍵盤和滑鼠動作輸入工作階段。

- 除了目前的會話) 之外，您還可以從遠端控制您自己的會話 (但您必須擁有 [完全控制] 許可權或 [遠端控制] 特殊存取權限，才能從遠端控制另一個會話。

- 您也可以使用遠端桌面服務管理員來起始遠端控制。

- 除非停用警告，否則在開始監視之前，伺服器會警告使用者即將從遠端控制工作階段。 等待使用者的回應時，您的工作階段可能看似凍結數秒鐘。 若要為使用者和會話設定遠端控制，請使用遠端桌面服務設定工具或遠端桌面服務擴充功能提供給本機使用者和群組，以及 active directory 使用者和電腦。

- 您的工作階段必須能夠支援遠端控制的工作階段所使用的視訊解析度，否則操作就會失敗。

- 主控台會話無法從遠端控制另一個會話，也不能由另一個會話遠端控制。

- 當您想要 (遮蔽) 來進行遠端控制時，請 `*` `*` 只在數位鍵台) 中使用 CTRL + (。

## <a name="examples"></a>範例

若要遮蔽 *會話 93*，請輸入：

```
shadow 93
```

若要遮蔽會話 *ACCTG01*，請輸入：

```
shadow ACCTG01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

---
title: shadow
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877109"
---
# <a name="shadow"></a>shadow

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓您從遠端控制使用中工作階段的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的另一位使用者。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\<SessionName>|指定您想要從遠端控制工作階段的名稱。|
|\<SessionID>|指定您想要從遠端控制工作階段的識別碼。 使用**查詢使用者**以顯示工作階段與他們的工作階段識別碼的清單。|
|/server:\<伺服器名稱 >|指定包含您想要從遠端控制工作階段的 rd 工作階段主機伺服器。 根據預設，會使用目前的 rd 工作階段 Host4 伺服器。|
|/v|顯示已執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您可以檢視或主動控制工作階段。 如果您選擇主動控制使用者工作階段，您可以輸入的鍵盤和滑鼠動作至工作階段。
-   您可以一律從遠端控制自己的工作階段 （除了目前的工作階段），但您必須擁有完全控制權限或從遠端控制另一個工作階段的遠端控制特殊存取權限。
-   您也可以使用遠端桌面服務管理員，以起始遠端控制。
-   開始監視之前，伺服器會警告使用者工作階段即將進行遠端控制，除非已停用此警告。 您的工作階段可能看似凍結數秒鐘，同時等候使用者回應。 若要設定使用者和工作階段的遠端控制，使用遠端桌面服務組態工具或遠端桌面服務延伸模組，以本機使用者和群組和 active directory 使用者和電腦。
-   您的工作階段必須能夠支援使用在您從遠端控制，或作業失敗的工作階段的視訊解析度。
-   主控台工作階段可以無法從遠端控制另一個工作階段，也不該進行遠端控制其他工作階段。
-   當您想要結束遠端控制 （遮蔽） 時，請按 CTRL + * (使用\*從數字鍵台只有)。

## <a name="BKMK_examples"></a>範例
-   至陰影階段 93，請輸入：
    ```
    shadow 93
    ```
-   若要遮蔽 ACCTG01 的工作階段，請輸入：
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

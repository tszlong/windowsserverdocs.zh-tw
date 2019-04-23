---
title: flattemp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fc14a6fe1a355f7c20c130fba3fb1f17e49b6f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872929"
---
# <a name="flattemp"></a>flattemp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

啟用或停用一般暫存資料夾。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|查詢目前的設定。|
|/enable|可讓一般的暫存資料夾。 使用者將共用的暫存資料夾，除非在使用者主資料夾所在的暫存資料夾。|
|/disable|停用一般的暫存資料夾。 每個使用者暫存資料夾會位於個別的資料夾 （由使用者 s 工作階段識別碼）。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Flattemp**您已在執行 Windows Server 2008 R2 的電腦上執行 Windows Server 2008 或 rd 工作階段主機角色服務的電腦上安裝終端機伺服器角色服務時，才可用命令。
-   您必須擁有系統管理認證才能執行**flattemp**。
-   每個使用者都會有一個唯一的暫存資料夾之後，請使用**flattemp /enable**啟用一般暫存資料夾。
-   建立暫存資料夾 （通常由指向在 TEMP 和 TMP 環境變數） 的多個使用者的預設方法是建立在子資料夾**\Temp**資料夾中的，使用 logonID 做為子資料夾名稱。 例如，如果 TEMP 環境變數指向 C:\Temp，指派給使用者 logonID 4 的暫存資料夾是 C:\Temp\4。 使用**flattemp**，您可以直接指向 \Temp 資料夾，並防止形成子資料夾。 當您想要包含在主資料夾，不論是在 rd 工作階段主機伺服器本機磁碟機或共用的網路磁碟機上的使用者暫存資料夾時，這非常有用。 您應該使用**flattemp /enable**命令只有每位使用者有不同的暫存資料夾時。
-   如果使用者的暫存資料夾位於網路磁碟機，您可能會遇到應用程式錯誤。 會發生這種情況時暫時無法存取網路上的共用的網路磁碟機。 應用程式的暫存檔案都無法存取或未執行同步處理，因為它會回應，如果磁碟已停止。 建議您不要將臨時資料夾移至網路磁碟機。 預設會保留在本機硬碟上的暫存資料夾。 如果您遇到未預期的行為或與特定應用程式的磁碟損毀錯誤時，穩定網路，或將臨時資料夾移回本機硬碟。
-   如果您使用不同的暫存資料夾個別工作階段，停用**flattemp**設定都會被忽略。 遠端桌面服務組態工具中設定此選項。

## <a name="BKMK_examples"></a>範例
-   若要顯示一般的暫存資料夾的目前設定，請輸入：
    ```
    flattemp /query
    ```
-   若要啟用一般暫存資料夾，請輸入：
    ```
    flattemp /enable
    ```
-   若要停用一般暫存資料夾，請輸入：
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)

[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

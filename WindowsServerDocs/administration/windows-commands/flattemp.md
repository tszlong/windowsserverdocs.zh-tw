---
title: flattemp
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0969209044784f87c917d90af257c5b3b523af16
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720105"
---
# <a name="flattemp"></a>flattemp

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用或停用單層暫存資料夾。


> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|查詢目前的設定。|
|/enable|啟用單層暫存資料夾。 除非暫存資料夾位於使用者的主資料夾中，否則使用者會共用暫存資料夾。|
|/disable|停用單層暫存資料夾。 每個使用者的暫存資料夾都會位於另一個資料夾（由使用者的會話識別碼決定）。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   只有當您已在執行 Windows Server 2008 的電腦上安裝終端機伺服器角色服務，或是在執行 Windows Server 2008 R2 的電腦上安裝了 [rd 工作階段主機] 角色服務時，才可以使用**flattemp**命令。
-   您必須具有系統管理認證，才能執行**flattemp**。
-   當每位使用者都有唯一的暫存資料夾之後，請使用**flattemp/enable**來啟用一般暫存資料夾。
-   為多個使用者建立暫存資料夾的預設方法（通常由 TEMP 和 TMP 環境變數指向）是使用 logonID 做為子資料夾名稱，在 [ **\temp** ] 資料夾中建立子資料夾。 例如，如果 TEMP 環境變數指向 C：\Temp，則指派給使用者 logonID 4 的暫存資料夾會是 C:\Temp\4。 使用**flattemp**，您可以直接指向 [\temp] 資料夾，並防止子資料夾形成。 當您想要讓使用者暫存資料夾包含在主資料夾中（不論是在 rd 工作階段主機伺服器本機磁片磁碟機或共用網路磁碟機機上）時，這會很有用。 只有在每個使用者都有個別的暫存資料夾時，您才應該使用**flattemp/enable**命令。
-   如果使用者的暫存資料夾是在網路磁碟機機上，您可能會遇到應用程式錯誤。 當網路上的共用網路磁碟機機暫時無法存取時，就會發生這種情況。 因為應用程式的暫存檔案無法存取或同步處理，所以它會回應磁片是否已停止。 不建議將暫存資料夾移動到網路磁碟機機。 預設為將暫存資料夾保留在本機硬碟上。 如果您遇到非預期的行為或特定應用程式的磁片損毀錯誤，請將您的網路穩定，或將暫存資料夾移回本機硬碟。
-   如果您停用每個會話使用個別的暫存資料夾，則會忽略**flattemp**設定。 此選項是在遠端桌面服務設定工具中設定。

## <a name="examples"></a>範例
-   若要顯示一般暫存資料夾的目前設定，請輸入：
    ```
    flattemp /query
    ```
-   若要啟用單層暫存資料夾，請輸入：
    ```
    flattemp /enable
    ```
-   若要停用單層暫存資料夾，請輸入：
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)

[遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

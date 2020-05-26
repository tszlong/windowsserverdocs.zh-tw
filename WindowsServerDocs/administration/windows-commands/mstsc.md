---
title: mstsc
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bee9dabe0e9344c7870e53ed34e4c9dd057cfc42
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820838"
---
# <a name="mstsc"></a>mstsc

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立遠端桌面工作階段主機（rd 工作階段主機）伺服器或其他遠端電腦的連線、編輯現有的遠端桌面連線（.rdp）設定檔，並將使用用戶端連接管理員建立的舊版連接檔案遷移至新的 .rdp 連接檔案。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

### <a name="parameters"></a>參數

|        參數        |                                                         說明                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   指定連接的 .rdp 檔案名。                                    |
|  /v： <伺服器 \> [： <埠 \> ] |                指定遠端電腦，以及您想要連接的埠號碼（選擇性）。                 |
|         /admin          |                                   將您連接到管理伺服器的會話。                                   |
|           /f            |                                    以全螢幕模式啟動遠端桌面連線。                                    |
|       /w<Width>        |                                      指定遠端桌面視窗的寬度。                                      |
|       /h<Height>       |                                     指定 [遠端桌面] 視窗的高度。                                      |
|         /public         |                  以公用模式執行遠端桌面。 在公用模式中，不會快取密碼和點陣圖。                  |
|          /span          | 將遠端桌面寬度和高度與本機虛擬桌面比對，視需要跨多個監視器。 |
| /edit<Connection File> |                                         開啟指定的 .rdp 檔案進行編輯。                                          |
|        /migrate         |       將使用用戶端連接管理員所建立的舊版連接檔案，遷移至新的 .rdp 連接檔案。       |
|           /?            |                                            在命令提示字元顯示說明。                                             |

## <a name="remarks"></a>備註
-   預設會將每個使用者的 .rdp 儲存為使用者的 [檔] 資料夾中的隱藏檔案。 使用者建立的 .rdp 檔案預設會儲存在使用者的 [檔] 資料夾中，但可以儲存在任何位置。
-   若要跨越監視器，監視器必須使用相同的解析度，而且必須水準對齊（也就是並存）。 目前不支援跨越用戶端系統上彼此垂直的多個監視器顯示桌面內容。

## <a name="examples"></a>範例
-   若要以全螢幕模式連接到會話，請輸入：
    ```
    mstsc /f
    ```
-   若要開啟名為 filename .rdp 的檔案以進行編輯，請輸入：
    ```
    mstsc /edit filename.rdp
    ```

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
-   [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

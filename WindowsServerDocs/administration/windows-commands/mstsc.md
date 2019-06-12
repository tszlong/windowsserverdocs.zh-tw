---
title: mstsc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6f89c1e3b0d36f14dbd55f9e6994c788305b30d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437181"
---
# <a name="mstsc"></a>mstsc

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立遠端桌面工作階段主機 (rd 工作階段主機) 伺服器或其他遠端電腦的連線、 編輯現有的遠端桌面連線 (.rdp) 組態檔，並移轉用戶端連線管理員所建立的舊版連線檔案新的.rdp 連線檔案。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>參數

|        參數        |                                                         描述                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   指定的.rdp 連線檔案的名稱。                                    |
|   /v:<Server[:<Port>]   |                指定在遠端電腦和 （選擇性） 您要連接的連接埠號碼。                 |
|         /admin          |                                   連線到管理伺服器的工作階段。                                   |
|           /f            |                                    以全螢幕模式中啟動遠端桌面連線。                                    |
|       /w:<Width>        |                                      指定遠端桌面 視窗的寬度。                                      |
|       /h:<Height>       |                                     指定的遠端桌面 視窗的高度。                                      |
|         /public         |                  以公用模式執行遠端桌面。 在公用模式中，密碼和點陣圖不會快取。                  |
|          /span          | 遠端桌面寬度和高度與本機虛擬桌面，橫跨多個監視器，如有必要，便會比對。 |
| /edit <Connection File> |                                         開啟指定的.rdp 檔案，以進行編輯。                                          |
|        /migrate         |       將移轉至新的.rdp 連線檔案建立與用戶端連線管理員的舊版連線檔案。       |
|           /?            |                                            在命令提示字元顯示說明。                                             |

## <a name="remarks"></a>備註
-   為 Default.rdp 會儲存每個使用者，為使用者的文件資料夾中的隱藏檔案。 使用者建立.rdp 檔會儲存預設會在使用者的文件 資料夾，但可以儲存在任何位置。
-   若要跨監視器，監視必須使用相同的解析度和必須水平對齊 （也就是說，並列）。 目前不支援跨多個監視器，以垂直方式在用戶端系統。

## <a name="BKMK_examples"></a>範例
-   若要連接至全螢幕模式中的工作階段，請輸入：
    ```
    mstsc /f
    ```
-   若要開啟名為 filename.rdp 進行編輯的檔案，請輸入：
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)

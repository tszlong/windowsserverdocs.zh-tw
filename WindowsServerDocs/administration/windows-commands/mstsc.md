---
title: mstsc
description: Mstsc 命令的參考文章，可建立遠端桌面工作階段主機伺服器或其他遠端電腦的連線、編輯現有的遠端桌面連線 ( .rdp) 設定檔，以及將使用用戶端連線管理員建立的舊版連接檔案遷移至新的 .rdp 連接檔案。
ms.topic: reference
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 684ab29a9e1ded85443a2ec2d05ad4f55ec2cd5c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025212"
---
# <a name="mstsc"></a>mstsc

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會建立遠端桌面工作階段主機伺服器或其他遠端電腦的連線、編輯現有的遠端桌面連線 ( .rdp) 設定檔，以及將使用用戶端連線管理員建立的舊版連接檔案遷移至新的 .rdp 連接檔案。

## <a name="syntax"></a>語法

```
mstsc.exe [<connectionfile>] [/v:<server>[:<port>]] [/admin] [/f] [/w:<width> /h:<height>] [/public] [/span]
mstsc.exe /edit <connectionfile>
mstsc.exe /migrate
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ------------|
| `<connectionfile>` | 指定連接的 .rdp 檔名稱。 |
| /v:`<server>[:<port>]` | 指定遠端電腦，並選擇性地指定您要連接的埠號碼。 |
| /admin | 將您連接到管理伺服器的會話。 |
| /f | 以全螢幕模式啟動 [遠端桌面連線]。 |
| /w`<width>` | 指定遠端桌面視窗的寬度。 |
| /h`<height>` | 指定 [遠端桌面] 視窗的高度。 |
| /public | 以公用模式執行遠端桌面。 在公用模式中，不會快取密碼和點陣圖。 |
| /span | 若有需要，請將遠端桌面寬度和高度與本機虛擬桌面比對，以跨越多個監視器。 |
| /edit `<connectionfile>` | 開啟指定的 .rdp 檔案進行編輯。 |
| /migrate | 將使用用戶端連線管理員建立的舊版連接檔案遷移至新的 .rdp 連接檔案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 預設會將每個使用者的 rdp 儲存為使用者的 [檔 **] 資料夾中** 的隱藏檔案。

- 使用者建立的 .rdp 檔案預設會儲存在使用者的 **[檔] 資料夾中** ，但可儲存在任何位置。

- 若要跨越監視器，監視器必須使用相同的解析度，且必須水準對齊， (也就是並行) 。 目前不支援跨越用戶端系統上彼此垂直的多個監視器顯示桌面內容。

### <a name="examples"></a>範例

若要以全螢幕模式連接到會話，請輸入：

```
mstsc /f
```

若要開啟名為 *.rdp* 的檔案進行編輯，請輸入：

```
mstsc /edit filename.rdp
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

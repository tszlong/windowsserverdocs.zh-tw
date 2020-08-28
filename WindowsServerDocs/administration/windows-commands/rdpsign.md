---
title: rdpsign
description: '>rdpsign.exe 命令的參考文章，可讓您以數位方式簽署遠端桌面通訊協定 ( .rdp) 檔。'
ms.topic: reference
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ecd80969f42a440bfd583223779fe67c27c5c310
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037146"
---
# <a name="rdpsign"></a>rdpsign

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您以數位方式簽署遠端桌面通訊協定 ( .rdp) 檔。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /sha1 `<hash>` | 指定憑證指紋，也就是憑證存放區中所包含之簽署憑證的安全雜湊演算法 1 (SHA1) 雜湊。 用於 Windows Server 2012 R2 和更舊版本。 |
| /sha256 `<hash>` | 指定指紋，也就是憑證存放區中包含的簽署憑證的安全雜湊演算法 256 (SHA256) 雜湊。 取代 Windows Server 2016 和更新版本中的/sha1。 |
| /q | 無訊息模式。 如果命令失敗，則不會產生任何輸出，以及最基本的輸出。 |
| /v | 詳細資訊模式。 顯示所有警告、訊息和狀態。 |
| /l | 測試簽署和輸出結果，而不實際取代任何輸入檔。 |
| `<file_name.rdp>` | .Rdp 檔的名稱。 您必須使用完整的檔案名，指定 .rdp 檔 (或) 簽署的檔。 不接受萬用字元。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- SHA1 或 SHA256 憑證指紋應代表受信任的 .rdp 檔發行者。 若要取得憑證指紋，請開啟 [ **憑證** ] 嵌入式管理單元，按兩下您想要使用的憑證 (在本機電腦的 [憑證存放區] 或 [個人憑證存儲]) 中，按一下 [ **詳細資料** ] 索引標籤，然後在 [ **欄位** ] 清單中，按一下 [ **指紋**]。

    > [!NOTE]
    > 當您複製與 rdpsign.exe 工具搭配使用的指紋時，必須移除任何空格。

- 簽署的輸出檔會覆寫輸入檔。

- 如果指定了多個檔案，而且無法讀取或寫入任何 .rdp 檔案，則工具會繼續進行下一個檔案。

### <a name="examples"></a>範例

若要簽署名為 *file1*的 .rdp 檔案，請流覽至您儲存 .rdp 檔的資料夾，然後輸入：

```
rdpsign /sha1 hash file1.rdp
```

> [!NOTE]
> *雜湊*值代表 SHA1 憑證指紋，不含任何空格。

若要測試 .rdp 檔的數位簽章是否會成功，而不需要實際簽署檔案，請輸入：

```
rdpsign /sha1 hash /l file1.rdp
```

若要簽署名為、 *file1*、 *file2 .rdp*和 *file3*的多個 .rdp 檔案，請輸入 (包括檔案名) 之間的空格：

```
rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
```

## <a name="see-also"></a>另請參閱

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)

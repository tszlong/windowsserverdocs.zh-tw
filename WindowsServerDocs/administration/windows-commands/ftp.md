---
title: ftp
description: Ftp 命令的參考文章，此命令會在執行檔案傳輸通訊協定 (ftp) server 服務的電腦之間傳輸檔案。
ms.topic: reference
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f5cb70e6b42e390f8e279152e736b0226e74f9d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038899"
---
# <a name="ftp"></a>ftp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從執行檔案傳輸通訊協定 (ftp) server 服務的電腦來回傳輸檔案。 此命令可透過處理 ASCII 文字檔，以互動方式或在批次模式中使用。

## <a name="syntax"></a>語法

```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<filename>] [-a] [-A] [-x:<sendbuffer>] [-r:<recvbuffer>] [-b:<asyncbuffers>][-w:<windowssize>][<host>] [-?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ----------| ----------- |
| -v | 隱藏遠端伺服器回應的顯示。 |
| -d | 啟用偵測，並顯示 FTP 用戶端與 FTP 伺服器之間傳遞的所有命令。 |
| -i | 在多個檔案傳輸期間停用互動式提示。 |
| -n | 在初始連接時隱藏自動登入。 |
| -g | 停用檔案名萬用字元。  **Glob** 允許使用星號 ( * ) 和問號 (？ ) 做為本機檔案和路徑名稱中的萬用字元。 |
| ！`<filename>` | 指定包含 **ftp** 命令的文字檔。 這些命令會在 **ftp** 啟動後自動執行。 此參數不允許空格。 使用這個參數，而不是重新導向 (`<`) 。 **注意：** 在 Windows 8 和 Windows Server 2012 或更新版本的作業系統中，必須以 UTF-8 撰寫文字檔。 |
| -a | 指定在系結 ftp 資料連線時可以使用任何本機介面。 |
| -A | 以匿名方式登入 ftp 伺服器。 |
| 版`<sendbuffer> `| 覆寫預設的 8192 SO_SNDBUF 大小。 |
| 桌上型電腦主機板`<recvbuffer>` | 覆寫預設的 8192 SO_RCVBUF 大小。 |
| b`<asyncbuffers>` | 覆寫預設的非同步緩衝區計數3。 |
| w`<windowssize>` | 指定傳輸緩衝區的大小。 預設視窗大小為4096個位元組。 |
| `<host>` | 指定要連接之 ftp 伺服器的電腦名稱稱、IP 位址或 IPv6 位址。 主機名稱或位址（若有指定）必須是該行的最後一個參數。 |
| -? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Ftp**命令列參數會區分大小寫。

- 只有在 [網路連線] 的網路介面卡內容中，將 [ ** (tcp/ip) ** 通訊協定安裝為元件時，才能使用此命令。

- **Ftp**命令可透過互動方式使用。 啟動之後， **ftp** 會建立子環境，您可以在其中使用 **ftp** 命令。 您可以輸入 **quit** 命令以返回命令提示字元。 當 **ftp** 子環境正在執行時，命令提示字元會指出此環境 `ftp >` 。 如需詳細資訊，請參閱 **ftp** 命令。

- 安裝 IPv6 通訊協定時， **ftp** 命令支援使用 ipv6。

### <a name="examples"></a>範例

若要登入名為的 ftp 伺服器 `ftp.example.microsoft.com` ，請輸入：

```
ftp ftp.example.microsoft.com
```

若要登入名為的 ftp 伺服器， `ftp.example.microsoft.com` 並執行包含在名為*resync.txt*之檔案中的**ftp**命令，請輸入：

```
ftp -s:resync.txt ftp.example.microsoft.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))

- [IP 第6版](/previous-versions/windows/it-pro/windows-server-2003/cc738636(v=ws.10))

- [IPv6 應用程式](/previous-versions/windows/it-pro/windows-server-2003/cc782509(v=ws.10))

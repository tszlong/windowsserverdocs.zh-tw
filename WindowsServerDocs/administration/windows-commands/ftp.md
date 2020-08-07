---
title: ftp
description: Ftp 命令的參考文章，它會在執行檔案傳輸通訊協定 (ftp) server 服務的電腦上來回傳輸檔案。
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 369a41cf6ad803a4fce939da58228997410cf177
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888789"
---
# <a name="ftp"></a>ftp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在執行檔案傳輸通訊協定 (ftp) server 服務的電腦之間傳輸檔案。 此命令可透過處理 ASCII 文字檔，以互動方式或在批次模式中使用。

## <a name="syntax"></a>語法

```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<filename>] [-a] [-A] [-x:<sendbuffer>] [-r:<recvbuffer>] [-b:<asyncbuffers>][-w:<windowssize>][<host>] [-?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ----------| ----------- |
| -v | 隱藏遠端伺服器回應的顯示。 |
| -d | 啟用「偵測」，顯示在 FTP 用戶端與 FTP 伺服器之間傳遞的所有命令。 |
| -i | 停用多個檔案傳輸期間的互動式提示。 |
| -n | 在初始連接時抑制自動登入。 |
| -g | 停用檔案名萬用字元。  **Glob**允許使用星號 ( * ) 和問號 (？ ) 作為本機檔案和路徑名稱中的萬用字元。 |
| 今日`<filename>` | 指定包含**ftp**命令的文字檔。 這些命令會在**ftp**啟動後自動執行。 此參數不允許空格。 請使用此參數，而不是 () 的重新導向 `<` 。 **注意：** 在 Windows 8 和 Windows Server 2012 或更新版本的作業系統中，文字檔必須以 UTF-8 撰寫。 |
| -a | 指定在系結 ftp 資料連線時，可以使用任何本機介面。 |
| -A | 以匿名方式登入 ftp 伺服器。 |
| x`<sendbuffer> `| 覆寫8192的預設 SO_SNDBUF 大小。 |
| r`<recvbuffer>` | 覆寫8192的預設 SO_RCVBUF 大小。 |
| 位元組`<asyncbuffers>` | 覆寫預設的非同步緩衝區計數3。 |
| 寬`<windowssize>` | 指定傳輸緩衝區的大小。 預設視窗大小為4096個位元組。 |
| `<host>` | 指定要連接之 ftp 伺服器的電腦名稱稱、IP 位址或 IPv6 位址。 主機名稱或位址（如果有指定）必須是該行的最後一個參數。 |
| -? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Ftp**命令列參數會區分大小寫。

- 只有當**網際網路通訊協定 (tcp/ip) **通訊協定在網路連線的網路介面卡內容中安裝為元件時，才可使用此命令。

- **Ftp**命令可以互動方式使用。 啟動之後， **ftp**會建立一個子環境，您可以在其中使用**ftp**命令。 您可以輸入**quit**命令來返回命令提示字元。 當**ftp**子環境正在執行時，命令提示字元會指示它 `ftp >` 。 如需詳細資訊，請參閱**ftp**命令。

- 安裝 IPv6 通訊協定時， **ftp**命令支援使用 ipv6。

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

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))

- [IP 第6版](/previous-versions/windows/it-pro/windows-server-2003/cc738636(v=ws.10))

- [IPv6 應用程式](/previous-versions/windows/it-pro/windows-server-2003/cc782509(v=ws.10))

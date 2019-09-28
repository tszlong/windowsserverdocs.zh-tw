---
title: ftp
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b64e6831fcf8930c62b2a3f04022dc49d5297683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375856"
---
# <a name="ftp"></a>ftp

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在執行檔案傳輸通訊協定（ftp）伺服器服務的電腦之間傳輸檔案。 您可以藉由處理 ASCII 文字檔，以互動方式或在批次模式中使用**ftp** 。 
## <a name="syntax"></a>語法
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>參數

|     參數     |                                                                                                                                                      描述                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    隱藏遠端伺服器回應的顯示。                                                                                                                                     |
|        -d.ddd...e         |                                                                                                               啟用「偵測」，顯示在 FTP 用戶端與 FTP 伺服器之間傳遞的所有命令。                                                                                                                |
|        -i         |                                                                                                                            停用多個檔案傳輸期間的互動式提示。                                                                                                                             |
|        -n         |                                                                                                                                    在初始連接時抑制自動登入。                                                                                                                                     |
|        -g         |                                         停用檔案名萬用字元。  **Glob**允許使用星號（\*）和問號（？）做為本機檔案和路徑名稱中的萬用字元。 如需詳細資訊，請參閱[其他參考](ftp.md#BKMK_additionalRef)資料。                                          |
|   -s： <FileName>   | 指定包含**ftp**命令的文字檔。 這些命令會在**ftp**啟動後自動執行。 此參數不允許空格。 使用此參數，而不是重新**導向（<** ）。 **注意：** 在 Windows 8 和 Windows Server 2012 或更新版本的作業系統中，文字檔必須以 UTF-8 撰寫。 |
|        -a         |                                                                                                                 指定在系結 ftp 資料連線時，可以使用任何本機介面。                                                                                                                  |
|        -A         |                                                                                                                                        以匿名方式登入 ftp 伺服器。                                                                                                                                         |
|  -x： <SendBuffer>  |                                                                                                                                     覆寫8192的預設 SO_SNDBUF 大小。                                                                                                                                     |
|  -r： <RecvBuffer>  |                                                                                                                                     覆寫8192的預設 SO_RCVBUF 大小。                                                                                                                                     |
| -b： <AsyncBuffers> |                                                                                                                                    覆寫預設的非同步緩衝區計數3。                                                                                                                                     |
| -w： <WindowsSize>  |                                                                                                                   指定傳輸緩衝區的大小。 預設視窗大小為4096個位元組。                                                                                                                   |
|        -?         |                                                                                                                                         在命令提示字元顯示說明。                                                                                                                                          |
|      <host>       |                                                                    指定要連接之 ftp 伺服器的電腦名稱稱、IP 位址或 IPv6 位址。 主機名稱或位址（如果有指定）必須是該行的最後一個參數。                                                                    |

## <a name="remarks"></a>備註
- 如需有關 Windows Server 2003 上**ftp**命令的詳細資訊，請參閱[ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx)。
- **ftp**命令列參數會區分大小寫。
- 只有當**網際網路通訊協定（tcp/ip）** 通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。
- **ftp**可以互動方式使用。 啟動之後， **ftp**會建立一個子環境，您可以在其中使用**ftp**命令。 您可以輸入**quit**命令來返回命令提示字元。 當**ftp**子環境正在執行時， **ftp >** 命令提示字元會指出它。 如需詳細資訊，請參閱**ftp**命令。
- 安裝 IPv6 通訊協定時， **ftp**支援使用 ipv6。 如需詳細資訊，請參閱[其他參考](ftp.md#BKMK_additionalRef)資料。
  ## <a name="BKMK_Examples"></a>典型
  若要登入名為 ftp.example.microsoft.com 的 ftp 伺服器，請輸入：
  ```
  ftp ftp.example.microsoft.com
  ```
  若要登入名為 ftp.example.microsoft.com 的 ftp 伺服器，並執行包含在名為 resync 之檔案中的**ftp**命令，請輸入：
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="BKMK_additionalRef"></a>其他參考
- [IP 第6版](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [IPv6 應用程式](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- [命令列語法關鍵](command-line-syntax-key.md)

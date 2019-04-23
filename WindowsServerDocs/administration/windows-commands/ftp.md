---
title: ftp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 76b8a2ee7ce81a6db75e0d375d017746e4361df1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887379"
---
# <a name="ftp"></a>ftp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

傳輸檔案與執行檔案傳輸通訊協定 (ftp) 伺服器服務的電腦。 **ftp**可供以互動方式或以批次模式處理 ASCII 文字檔。 
## <a name="syntax"></a>語法
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-v|隱藏顯示遠端伺服器的回應。|
|-d|啟用偵錯，顯示所有 FTP 用戶端與 FTP 伺服器之間傳遞的命令。|
|-i|停用在多個檔案傳輸期間的互動式提示。|
|-n|隱藏初始連線時自動登入。|
|-g|停用檔案名稱萬用字元。  **Glob**作為萬用字元，在本機的檔案和路徑名稱中允許使用星號 （*） 和問號 （？）。 如需詳細資訊，請參閱 <<c0> [ 其他參考](ftp.md#BKMK_additionalRef)。|
|-s:。<FileName>|指定的文字檔案，其中包含**ftp**命令。 這些命令會執行自動之後**ftp**啟動。 此參數可讓不含空格。 使用此參數，而不是重新導向 (**<**)。 **注意：** 在 Windows 8 和 Windows Server 2012 或更新版本的作業系統，您必須以 utf-8，撰寫文字檔案。|
|-a|指定繫結 ftp 資料連線時，可以使用任何本機介面。|
|-A|登入 ftp 伺服器為匿名。|
|-x:。<SendBuffer>|覆寫 8192 的預設 SO_SNDBUF 大小。|
|-:。<RecvBuffer>|覆寫 8192 的預設 SO_RCVBUF 大小。|
|-b:<AsyncBuffers>|覆寫預設非同步緩衝區計數為 3。|
|-w:。<WindowsSize>|指定傳送緩衝區的大小。 預設的視窗大小為 4096 個位元組。|
|-?|在命令提示字元顯示說明。|
|<host>|指定電腦名稱、 IP 位址或要連接之 ftp 伺服器的 IPv6 位址。 主機名稱或位址，如果指定，就必須是該行的最後一個參數。|
## <a name="remarks"></a>備註
-   如需詳細資訊**ftp**命令，在 Windows Server 2003，請參閱[ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx)。
-   **ftp**命令列參數會區分大小寫。
-   使用此命令，只有當**網際網路通訊協定 (TCP/IP)** 通訊協定安裝為在 網路連線的網路介面卡的內容中的元件。
-   **ftp**可以互動方式使用。 啟動之後， **ftp**會建立子環境，您可以使用**ftp**命令。 您可以返回命令提示字元中輸入**結束**命令。 當**ftp**子環境執行，其中會顯示**ftp >** 命令提示字元。 如需詳細資訊，請參閱**ftp**命令。
-   **ftp**安裝 IPv6 通訊協定時，支援使用 IPv6。 如需詳細資訊，請參閱 <<c0> [ 其他參考](ftp.md#BKMK_additionalRef)。
## <a name="BKMK_Examples"></a>範例
若要登入名為 ftp.example.microsoft.com 的 ftp 伺服器，請輸入：
```
ftp ftp.example.microsoft.com
```
登入 ftp 伺服器名為 ftp.example.microsoft.com 和 執行 **ftp**在名為 resync.txt，型別所包含的命令：
```
ftp -s:resync.txt ftp.example.microsoft.com
```
## <a name="BKMK_additionalRef"></a>其他參考資料
-   [IP 第 6 版](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
-   [IPv6 的應用程式](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
-   [命令列語法關鍵](command-line-syntax-key.md)

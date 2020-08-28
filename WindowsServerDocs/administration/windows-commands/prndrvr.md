---
title: prndrvr
description: Prndrvr 命令的參考文章，可新增、刪除和列出印表機驅動程式。
ms.topic: reference
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36ad4a4206e26065dfad9ff2d970da11e4efaa66
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038699"
---
# <a name="prndrvr"></a>prndrvr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

新增、刪除及列出印表機驅動程式。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入 **cscript** ，後面接著 prndrvr 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如： `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr` 。

使用時不含參數， **prndrvr** 會顯示命令列說明。

## <a name="syntax"></a>語法

```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] [-e <environment>] [-s <Servername>] [-u <Username>] [-w <password>] [-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -a | 安裝驅動程式。 |
| -d | 刪除驅動程式。 |
| -l | 列出安裝在 **-s** 參數所指定之伺服器上的所有印表機驅動程式。 如果您未指定伺服器，Windows 會列出安裝在本機電腦上的印表機驅動程式。 |
| -X | 刪除 **-s** 參數所指定之伺服器上的邏輯印表機未使用的所有印表機驅動程式和其他印表機驅動程式。 如果您未指定要從清單中移除的伺服器，Windows 會刪除本機電腦上所有未使用的印表機驅動程式。 |
| -m `<model_name>` | 依名稱指定您要安裝的驅動程式)  (。 驅動程式通常是針對支援的印表機型號命名。 如需詳細資訊，請參閱印表機檔。 |
| `-v {0|1|2|3}` | 指定您想要安裝的驅動程式版本。 請參閱 **-e**參數的描述，以取得哪些環境可用版本的相關資訊。 如果您未指定版本，則會安裝適用于您要安裝驅動程式之電腦上所執行之 Windows 版本的驅動程式版本。 |
| -e `<environment>` | 指定您要安裝之驅動程式的環境。 如果您未指定環境，則會使用您要安裝驅動程式的電腦環境。 支援的環境參數為： **Windows NT x86**、 **Windows X64** 或 **windows IA64**。 |
| -s `<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -u `<Username>` -w `<password>` | 指定有權連線到裝載您要管理之印表機之電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與其他使用者。 如果您未指定帳戶，則必須以具有這些許可權的帳戶登入，才能使用命令。 |
| -h `<path>` | 指定驅動程式檔案的路徑。 如果您未指定路徑，則會使用已安裝 Windows 的位置路徑。 |
| -i `<filename.inf>` | 指定您要安裝之驅動程式的完整路徑和檔案名。 如果您未指定檔案名，腳本會使用 Windows 目錄之 inf 子目錄中的其中一個收件匣印表機 .inf 檔案。<p>如果未指定驅動程式路徑，腳本會在 driver.cab 檔案中搜尋驅動程式檔案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字 (例如「電腦名稱稱」 ) 。

- **-X**參數會刪除其他安裝的印表機驅動程式 (驅動程式，以便在執行 Windows) 替代版本的用戶端上使用，即使主要驅動程式正在使用中。 如果已安裝傳真元件，此選項也會刪除傳真驅動程式。 如果主要傳真驅動程式不在使用中，則會刪除它 (也就是，如果沒有使用它的佇列) 。 如果刪除主要傳真驅動程式，重新啟用傳真的唯一方法就是重新安裝傳真元件。

### <a name="examples"></a>範例

若要列出本機 printServer1 伺服器上的所有驅動程式 \\ ，請輸入：

```
cscript prndrvr -l -s
```

若要使用儲存在 c：\temp 資料夾中之驅動程式的 c:\temp\Laserprinter1.inf 驅動程式資訊檔案，為雷射印表機型號1的印表機新增第3版 Windows x64 印表機驅動程式，請輸入：

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

若要刪除第3版 Windows x64 印表機驅動程式進行雷射印表機型號1，請輸入：

```
cscript prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)

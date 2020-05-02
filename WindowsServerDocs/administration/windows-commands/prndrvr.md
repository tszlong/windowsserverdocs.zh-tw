---
title: prndrvr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c47a49146b1f20fb327d93c8cd00969cbe0a2304
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722832"
---
# <a name="prndrvr"></a>prndrvr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用**prndrvr**命令來新增、刪除和列出印表機驅動程式。

## <a name="syntax"></a>語法
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|-a|安裝驅動程式。|
|-d|刪除驅動程式。|
|-l|列出安裝在 **-s**參數所指定之伺服器上的所有印表機驅動程式。 如果您未指定伺服器，Windows 會列出安裝在本機電腦上的印表機驅動程式。|
|-X|刪除 **-s**參數所指定之伺服器上的邏輯印表機未使用的所有印表機驅動程式和其他印表機驅動程式。 如果您未指定要從清單中移除的伺服器，Windows 會刪除本機電腦上所有未使用的印表機驅動程式。|
|-m \<DrivermodelName\>|指定您想要安裝的驅動程式（依名稱）。 驅動程式通常是以其支援的印表機型號來命名。 如需詳細資訊，請參閱印表機檔。|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|指定您想要安裝的驅動程式版本。 如需哪些版本適用于哪些環境的詳細資訊，請參閱 **-e**參數的描述。 如果您未指定版本，則會安裝適用于您要安裝驅動程式之電腦上所執行之 Windows 版本的驅動程式版本。<p>-version **0**支援 windows 95、windows 98 和 windows Millennium edition。<br />-版本**1**支援 Windows NT 3.51。<br />-**第 2**版支援 Windows NT 4.0。<br />-**第 3**版支援 windows Vista、windows XP、windows 2000 和 windows Server 2003 作業系統。 請注意，這是 Windows Vista 唯一支援的印表機驅動程式版本。|
|-e \<環境>|指定您想要安裝之驅動程式的環境。 如果您未指定環境，則會使用您要安裝驅動程式之電腦的環境。 支援的環境參數包括：<p>-   **Windows NT x86**<br />-   **Windows x64**<br />-   **Windows IA64**|
|-s \<ServerName>|指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。|
|-u \<使用者名稱>- \<w 密碼>|指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。|
|-h \<路徑>|指定驅動程式檔案的路徑。 如果您未指定路徑，則會使用安裝 Windows 之位置的路徑。|
|-i \<Filename .inf>|指定您想要安裝之驅動程式的完整路徑和檔案名。 如果您未指定檔案名，腳本會使用 Windows 目錄之 inf 子目錄中的其中一個 [收件匣] 印表機 .inf 檔案。<p>如果未指定驅動程式路徑，腳本會搜尋驅動程式 .cab 檔案中的驅動程式檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
- **Prndrvr**命令是位於%windir%\system32\ printing_Admin_Scripts\\ <language>目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著 prndrvr 檔案的完整路徑，或將目錄變更為適當的資料夾。

  例如：
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- 如果您提供的資訊包含空格，請使用引號括住文字（例如， `computer Name`）。
- -X 選項會刪除所有額外的印表機驅動程式（安裝在執行其他 Windows 版本的用戶端上使用的驅動程式），即使主要驅動程式正在使用中也一樣。 如果已安裝傳真元件，此選項也會刪除傳真驅動程式。 如果主要傳真驅動程式不在使用中，則會將其刪除（也就是沒有佇列使用它）。 如果主要傳真驅動程式遭到刪除，重新啟用傳真的唯一方法就是重新安裝傳真元件。
- 使用時不含參數， **prndrvr**會顯示**prndrvr**命令的命令列說明。

## <a name="examples"></a>範例

若要列出 \PrintServer1 伺服器上\\的所有驅動程式，請輸入：
```
cscript Prndrvr -l -s
```

若要使用 C:\temp\Laserprinter1.inf 驅動程式資訊檔案，針對儲存在 C：\temp 資料夾類型中的驅動程式，新增第3版的 Windows x64 印表機驅動程式（適用于雷射印表機型號1）：
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

若要刪除第3版的 Windows NT x86 印表機驅動程式，以取得雷射印表機型號1，請輸入：
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows NT x86 
```

## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
[列印命令參考](print-command-reference.md)

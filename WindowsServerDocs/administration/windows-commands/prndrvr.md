---
title: prndrvr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436217"
---
# <a name="prndrvr"></a>prndrvr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用**prndrvr**命令來新增、 刪除和列出印表機驅動程式。

## <a name="syntax"></a>語法
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|-a|安裝的驅動程式。|
|-d|刪除驅動程式。|
|-l|列出所有安裝在所指定的伺服器上的印表機驅動程式 **-s**參數。 如果您未指定 server，Windows 就會列出本機電腦上安裝的印表機驅動程式。|
|-x|刪除所有的印表機驅動程式和其他的印表機驅動程式不在使用上所指定之伺服器的邏輯印表機 **-s**參數。 如果您未指定要從清單中移除的伺服器，Windows 就會刪除本機電腦上的所有未使用的印表機驅動程式。|
|-m \<DrivermodelName\>|指定您想要安裝的驅動程式 （依名稱）。 驅動程式通常會命名其所支援的印表機機型。 請參閱印表機說明文件，如需詳細資訊。|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|指定您想要安裝的驅動程式版本。 請參閱 popis **-e**參數所在的環境中可用的版本的資訊。 如果您未指定版本，會安裝適當版本的 Windows 電腦上執行您要在其中安裝驅動程式的驅動程式版本。<br /><br />-版本**0**支援 Windows 95、 Windows 98 和 Windows Millennium edition。<br />-版本**1**支援 Windows NT 3.51。<br />-版本**2**支援 Windows NT 4.0。<br />-版本**3**支援 Windows Vista、 Windows XP、 Windows 2000 和 Windows Server 2003 作業系統的電腦。 請注意，這是 Windows Vista 支援的唯一印表機驅動程式版本。|
|-e\<環境 >|指定您想要安裝驅動程式的環境。 如果您未指定的環境，則會使用您要在其中安裝驅動程式的電腦的環境。 支援的環境參數如下：<br /><br />-    **"Windows NT x86"**<br />-    **"Windows x64"**<br />-    **"Windows IA64"**|
|-s\<伺服器名稱 >|指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。|
|-u \<UserName> -w \<Password>|指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。|
|-h \<path>|指定驅動程式檔案的路徑。 如果您不指定路徑，則會使用 Windows 安裝的所在位置的路徑。|
|-i \<Filename.inf >|指定您想要安裝的驅動程式的完整路徑和檔案名稱。 如果您未指定檔案名稱，指令碼會使用其中一個收件匣印表機.inf 檔案的 Windows 目錄的 inf 子目錄中。<br /><br />如果未指定的驅動程式路徑，指令碼會搜尋 driver.cab 檔案中的驅動程式檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
- **Prndrvr**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 prndrvr 檔案或將目錄變更為適當的資料夾完整路徑。

  例如:
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- 如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。
- -X 選項會刪除所有其他的印表機驅動程式 （驅動程式安裝用於用戶端執行其他版本的 Windows），即使主要的驅動程式正在使用中。 如果已安裝傳真元件，則此選項也會刪除傳真的驅動程式。 如果不是使用中 （亦即，如果沒有任何使用它的佇列），則會刪除主要的傳真驅動程式。 如果刪除主要的傳真驅動程式時，若要重新啟用傳真的唯一方法是重新安裝傳真元件。
- 不含參數， **prndrvr**會顯示的命令列說明**prndrvr**命令。

## <a name="BKMK_examples"></a>範例

若要列出所有的驅動程式上\\\printServer1 伺服器中，輸入：
```
cscript Prndrvr -l -s
```

若要新增印表機驅動程式儲存在 C:\temp 資料夾類型使用 C:\temp\Laserprinter1.inf 驅動程式資訊檔的 「 雷射印表機模型 1 」 模型的第 3 版 Windows x64 印表機驅動程式：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

若要刪除 「 雷射印表機模型 1 」 的第 3 版 Windows NT x86 印表機驅動程式，請輸入：
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)

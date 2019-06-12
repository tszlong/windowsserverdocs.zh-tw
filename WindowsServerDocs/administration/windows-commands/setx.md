---
title: setx
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b2caceed6962bef22e7d546fa3b4469c9682b39
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441251"
---
# <a name="setx"></a>setx



建立或修改環境變數中的使用者或系統環境中，而不需要程式設計，或指令碼。 **Setx**命令也會擷取登錄機碼的值，並將其寫入到文字檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>參數

|         參數          |                                                                                                                                              描述                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s\<電腦 >       |                                                                                  指定的名稱或遠端電腦的 IP 位址。 請勿使用反斜線。 預設值是本機電腦的名稱。                                                                                  |
| /u [\<網域 >\]<User name> |                                                                                           使用指定的使用者帳戶的認證執行指令碼。 預設值是系統權限。                                                                                            |
|      /p [\<Password>]      |                                                                                                         指定在指定的使用者帳戶的密碼 **/u**參數。                                                                                                         |
|        \<變數 >         |                                                                                                                 指定您想要設定環境變數的名稱。                                                                                                                  |
|          \<值 >          |                                                                                                                指定您要設定環境變數的值。                                                                                                                 |
|         /k \<Path>         | 指定的變數會根據設定登錄機碼中的資訊。 P*ath*使用下列語法：</br>`\\<HIVE>\<KEY>\...\<Value>`</br>例如，您可以指定下列路徑：</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f\<檔案名稱 >       |                                                                                                                               指定您想要使用的檔案。                                                                                                                                |
|        / a \<X >，<Y>         |                                                                                                                    指定絕對座標和位移，做為搜尋參數。                                                                                                                    |
|   /r \<X >，<Y> "<String>」   |                                                                                                            指定相對座標和時差**字串**做為搜尋參數。                                                                                                            |
|             /m             |                                                                                                指定系統環境中設定變數。 預設值是本機的環境。                                                                                                 |
|             /x             |                                                                                                       顯示檔案的座標，請略過 **/a**， **/r**，並 **/d**命令列選項。                                                                                                        |
|      /d\<分隔符號 >      |                    指定分隔符號，例如" **，** 「 或 」 **\\** "要使用的四個內建分隔符號除了 — 空間、 TAB、 ENTER 和換行字元。 有效的分隔符號包括任何 ASCII 字元。 分隔符號的最大數目是 15，包括內建的分隔符號。                    |
|             /?             |                                                                                                                                 在命令提示字元顯示說明。                                                                                                                                  |

## <a name="remarks"></a>備註

-   **Setx**命令是類似於 UNIX 公用程式 SETENV。
-   **Setx**提供直接和永久設定系統環境的值僅命令列或以程式設計方式的方法。 系統環境變數是可透過手動設定**控制台中**或透過登錄編輯程式。 **設定**命令，也就是內部的命令直譯器 (Cmd.exe)，設定目前的主控台視窗只的使用者環境變數。
-   您可以使用**setx**命令，以從下列三個來源 （模式） 設定值的使用者和系統環境變數：命令列模式、 登錄模式或檔案模式。
-   **Setx**寫入登錄中的主要環境變數。 設定使用的變數**setx**變數不提供未來的命令視窗，在目前的 [命令] 視窗。
-   **HKEY_CURRENT_USER**並**HKEY_LOCAL_MACHINE**是唯一的支援的 hive。 REG_DWORD、 REG_EXPAND_SZ、 REG_SZ 和 REG_MULTI_SZ 有效期**RegKey**資料型別。
-   當您取得的存取權**REG_MULTI_SZ**值在登錄中，只有第一個項目會擷取並使用。
-   您無法使用**setx**命令來移除已新增至本機或系統環境的值。 您可以使用**設定**變數名稱，且沒有值可從本機環境中移除對應的值。
-   擷取並用於十六進位模式 REG_DWORD 登錄值。
-   檔案模式支援剖析歸位字元和換行字元 (CRLF) 文字檔案。

## <a name="BKMK_examples"></a>範例

若要設定電腦的環境變數值 Brand1 在本機環境中，輸入：
```
setx MACHINE Brand1
```
若要設定電腦的環境變數值 Brand1 電腦系統環境中，輸入：
```
setx MACHINE "Brand1 Computer" /m
```
若要設定本機的環境，以使用在 PATH 環境變數中定義的搜尋路徑 MYPATH 環境變數，請輸入：
```
setx MYPATH %PATH%
```
若要在本機的環境，以使用取代後的 PATH 環境變數中所定義的搜尋路徑中設定 MYPATH 環境變數 **~** 具有 **%** ，型別：
```
setx MYPATH ~PATH~ 
```
若要設定機器的環境變數來 Brand1 名為 Computer1 的遠端電腦上的本機環境中，輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
若要設定本機的環境，以使用名為 Computer1 的遠端電腦上的 PATH 環境變數中定義的搜尋路徑 MYPATH 環境變數，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
若要在本機環境中找到的值來設定 TZONE 環境變數**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**登錄的索引鍵，類型：
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
若要設定 TZONE 環境變數中找到的值命名 Computer1 的遠端電腦的本機環境中**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName**登錄索引鍵，類型：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
若要設定系統環境中找到的值中的組建環境變數**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**登錄的索引鍵，類型：
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
若要設定組建的環境變數中找到的值命名 Computer1 的遠端電腦的系統環境中**HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber**登錄機碼類型：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
若要顯示具名 Ipconfig.out，內容的對應座標，以及檔案的內容中，輸入：
```
setx /f ipconfig.out /x
```
若要設定 IPADDR 環境變數檔案 Ipconfig.out 中，請參閱 5,11 的座標值的本機環境中，輸入：
```
setx IPADDR /f ipconfig.out /a 5,11
```
若要找到在座標 5,3 分隔符號 Ipconfig.out 檔案中的值在本機環境中設定 OCTET1 環境變數 **"#$\*。 」** ，型別：
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
若要設定 IPGATEWAY 環境變數檔案 Ipconfig.out 中，請參閱座標 0,7 相對於 「 閘道 」 的座標值的本機環境中，輸入：
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
若要顯示名為 Ipconfig.out 檔案的內容，以及內容的對應座標 — 在名為 Computer1 的電腦，輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
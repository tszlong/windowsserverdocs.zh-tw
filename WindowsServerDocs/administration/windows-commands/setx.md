---
title: setx
description: Setx 的參考文章，它會在使用者或系統內容中建立或修改環境變數，而不需要程式設計或編寫腳本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69dcbca54419acb9ede0924e3e835bdfaf0633c1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935897"
---
# <a name="setx"></a>setx

在使用者或系統內容中建立或修改環境變數，而不需要程式設計或編寫腳本。 **Setx**命令也會抓取登錄機碼的值，並將它們寫入文字檔。



## <a name="syntax"></a>語法

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> <String>} [/m] | /x} [/d <Delimiters>]
```

### <a name="parameters"></a>參數

|         參數          |                                                                                                                                              說明                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s\<Computer>       |                                                                                  指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設值是本機電腦的名稱。                                                                                  |
| 那麼\<Domain>\]<User name> |                                                                                           使用指定之使用者帳戶的認證執行腳本。 預設值是系統許可權。                                                                                            |
|      /p [ \<Password> ]      |                                                                                                         指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                         |
|        \<Variable>         |                                                                                                                 指定您想要設定的環境變數名稱。                                                                                                                  |
|          \<Value>          |                                                                                                                指定您要設定環境變數的值。                                                                                                                 |
|         /k\<Path>         | 指定根據登錄機碼中的資訊來設定變數。 P*路徑 a)* 會使用下列語法：</br>`\\<HIVE>\<KEY>\...\<Value>`</br>例如，您可以指定下列路徑：</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<File name>       |                                                                                                                               指定您想要使用的檔案。                                                                                                                                |
|        /a \<X> 、<Y>         |                                                                                                                    指定絕對座標和位移作為搜尋參數。                                                                                                                    |
|   /r \<X> 、 <Y><String>   |                                                                                                            指定**字串**的相對座標和位移作為搜尋參數。                                                                                                            |
|             /m             |                                                                                                指定在系統內容中設定變數。 預設設定是本機環境。                                                                                                 |
|             /x             |                                                                                                       顯示檔案座標，忽略 **/a**、 **/r**和 **/d**命令列選項。                                                                                                        |
|      /d\<Delimiters>      |                    指定分隔符號（例如 **），** 或 **\\** 除了四個內建分隔符號（空格、TAB、ENTER 和換行字元）以外使用。 有效的分隔符號包含任何 ASCII 字元。 分隔符號的最大數目為15，包括內建的分隔符號。                    |
|             /?             |                                                                                                                                 在命令提示字元顯示說明。                                                                                                                                  |

## <a name="remarks"></a>備註

-   **Setx**命令類似于 UNIX 公用程式 SETENV。
-   **Setx**提供唯一的命令列或程式設計方式，直接並永久設定系統內容值。 系統內容變數可透過 [**控制台**] 或 [登錄編輯程式] 手動設定。 **Set**命令是命令直譯器的內部（Cmd.exe），只會為目前的主控台視窗設定使用者環境變數。
-   您可以使用**setx**命令，從三個來源（模式）的其中一個設定使用者和系統內容變數的值：命令列模式、登錄模式或檔案模式。
-   **Setx**會將變數寫入登錄中的主要環境。 使用**setx**變數設定的變數僅適用于未來的命令視窗，而不是在目前的命令視窗中。
-   **HKEY_CURRENT_USER**和**HKEY_LOCAL_MACHINE**是唯一支援的 hive。 REG_DWORD、REG_EXPAND_SZ、REG_SZ 和 REG_MULTI_SZ 是有效的**RegKey**資料類型。
-   當您取得登錄中**REG_MULTI_SZ**值的存取權時，只會解壓縮和使用第一個專案。
-   您無法使用**setx**命令來移除已新增至本機或系統內容的值。 您可以使用**set**搭配變數名稱，沒有值可從本機環境中移除對應的值。
-   REG_DWORD 登錄值會解壓縮並在十六進位模式中使用。
-   [檔案模式] 僅支援剖析 [回車] 和 [換行字元（CRLF）] 文字檔。

## <a name="examples"></a>範例

若要將本機環境中的電腦環境變數設定為 Brand1 值，請輸入：
```
setx MACHINE Brand1
```
若要將系統內容中的電腦環境變數設定為 Brand1 電腦的值，請輸入：
```
setx MACHINE Brand1 Computer /m
```
若要將本機環境中的 MYPATH 環境變數設定為使用 PATH 環境變數中定義的搜尋路徑，請輸入：
```
setx MYPATH %PATH%
```
若要在本機環境中設定 MYPATH 環境變數，以在以取代 with 之後使用 PATH 環境變數中定義的搜尋路徑 **~** **%** ，請輸入：
```
setx MYPATH ~PATH~
```
若要在本機環境中將電腦環境變數設定為在名為 Computer1 的遠端電腦上 Brand1，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
若要在本機環境中設定 MYPATH 環境變數，以使用在名為 Computer1 的遠端電腦上的 PATH 環境變數中定義的搜尋路徑，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
若要將本機環境中的 TZONE 環境變數設定為**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname**登錄機碼中找到的值，請輸入：
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```
若要在名為 Computer1 的遠端電腦本機環境中，將 TZONE 環境變數設定為**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname**登錄機碼中找到的值，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```
若要將系統內容中的組建環境變數設定為**HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber**登錄機碼中找到的值，請輸入：
```
setx BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber /m
```
若要在名為 Computer1 的遠端電腦系統內容中，將組建環境變數設定為在**HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber**登錄機碼中找到的值，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber /m
```
若要顯示名為 Ipconfig. out 的檔案內容，以及內容的對應座標，請輸入：
```
setx /f ipconfig.out /x
```
若要將本機環境中的 IPADDR 環境變數設定為在檔案 Ipconfig. out 的座標5、11中找到的值，請輸入：
```
setx IPADDR /f ipconfig.out /a 5,11
```
若要將本機環境中的 OCTET1 環境變數設定為在座標5中找到的值，請在檔案 Ipconfig. out 加上分隔符號** #$ \* 。** 輸入：
```
setx OCTET1 /f ipconfig.out /a 5,3 /d #$*.
```
若要將本機環境中的 IPGATEWAY 環境變數設定為在與檔案 Ipconfig 中的閘道座標相對的座標0、7上找到的值，請輸入：
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway
```
若要在名為 Computer1 的電腦上顯示名為 Ipconfig. out 的檔案內容，以及內容的對應座標，請輸入：
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
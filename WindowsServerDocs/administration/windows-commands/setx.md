---
title: setx
description: Setx 命令的參考文章，此命令會在使用者或系統內容中建立或修改環境變數，而不需要程式設計或編寫腳本。
ms.topic: reference
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 417a00dbb037c298784df81f9c5ed5e9c28485f8
ms.sourcegitcommit: 881a5bd40026288afbcee5fdbf602fd55f833d47
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586438"
---
# <a name="setx"></a>setx

在使用者或系統內容中建立或修改環境變數，而不需要程式設計或編寫腳本。 **Setx**命令也會抓取登錄機碼的值，並將它們寫入文字檔。

> [!NOTE]
> 此命令提供唯一的命令列或程式設計方式，直接和永久地設定系統內容值。 系統內容變數可透過 **主控台** 或透過登錄編輯程式，以手動方式設定。 **Set**命令（ ( # A0) 命令直譯器內部）只會設定目前主控台視窗的使用者環境變數。

## <a name="syntax"></a>語法

```
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] <variable> <value> [/m]
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] <variable>] /k <path> [/m]
setx [/s <computer> [/u [<domain>\]<user name> [/p [<password>]]]] /f <filename> {[<variable>] {/a <X>,<Y> | /r <X>,<Y> <String>} [/m] | /x} [/d <delimiters>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設值是本機電腦的名稱。 |
| u `[<domain>\]<user name>` | 使用指定之使用者帳戶的認證來執行腳本。 預設值為系統許可權。 |
| /p [ `<password>` ]| 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| `<variable>` | 指定您想要設定之環境變數的名稱。 |
| `<value>` | 指定您要設定環境變數的值。 |
| /k `<path>` | 指定根據登錄機碼中的資訊設定變數。 *路徑*使用下列語法： `\\<HIVE>\<KEY>\...\<Value>` 。 例如，您可以指定下列路徑： `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
| /f `<filename>` | 指定您想要使用的檔案。 |
| /a `<X>,<Y>` | 將絕對座標和位移指定為搜尋參數。 |
| /r `<X>,<Y> <String>` | 指定從 **字串** 到搜尋參數的相對座標和位移。 |
| /m | 指定在系統內容中設定變數。 預設設定為本機環境。 |
| /x | 顯示檔案座標，忽略 **/a**、 **/r**和 **/d** 命令列選項。 |
| /d `<delimiters>` | 指定 **,** **\\** 除了四個內建分隔符號（空格、定位字元、ENTER 和換行字元）之外，所要使用的分隔符號。 有效的分隔符號包含任何 ASCII 字元。 分隔符號的最大數目為15，包括內建分隔符號。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令類似于 UNIX 公用程式 SETENV。

- 您可以使用此命令，從三個來源的其中一個 (模式設定使用者和系統內容變數的值) ：命令列模式、登錄模式或檔案模式。

- 此命令會將變數寫入登錄中的主要環境。 使用 **setx** 變數設定的變數僅適用于未來的命令視窗，不能在目前的命令視窗中使用。

- **HKEY_CURRENT_USER** 和 **HKEY_LOCAL_MACHINE** 是唯一支援的 hive。 REG_DWORD、REG_EXPAND_SZ、REG_SZ 和 REG_MULTI_SZ 都是有效的 **RegKey** 資料類型。

- 如果您在登錄中取得 **REG_MULTI_SZ** 值的存取權，則只會解壓縮並使用第一個專案。

- 您無法使用此命令來移除新增至本機或系統內容的值。 您可以使用此命令搭配變數名稱，而不使用值來移除本機環境中的對應值。

- REG_DWORD 登錄值會以十六進位模式進行解壓縮和使用。

- 檔案模式僅支援剖析換行字元和換行 (CRLF) 文字檔。

- 在現有的變數上執行這個命令會移除任何變數參考，並使用展開的值。

  比方說，如果變數% PATH% 有% JAVADIR% 的參考，而% PATH% 是使用 **setx**操作的，則% JAVADIR% 會展開，並將其值直接指派給目標變數% PATH%。 這表示% JAVADIR% 的未來更新 **將不** 會反映在% PATH% 變數中。

- 請注意，使用 **setx**將內容指派給變數時，有1024個字元的限制。

  這表示，如果您超過1024個字元，則會裁剪內容，而且裁剪的文字會套用至目標變數。 如果此裁剪文字套用至現有的變數，可能會導致目標變數先前保留的資料遺失。

## <a name="examples"></a>範例

若要將本機環境中的 *機器* 環境變數設定為值 *Brand1*，請輸入：

```
setx MACHINE Brand1
```

若要將系統內容中的 *機器* 環境變數設定為值 *Brand1 電腦*，請輸入：

```
setx MACHINE Brand1 Computer /m
```

若要在本機環境中設定 *MYPATH* 環境變數以使用 *path* 環境變數中定義的搜尋路徑，請輸入：

```
setx MYPATH %PATH%
```

若要在將取代為之後，將本機環境中的 *MYPATH* 環境變數設定為使用 *path* 環境變數中定義的搜尋路徑 **~** **%** ，請輸入：

```
setx MYPATH ~PATH~
```

若要在名為*computer1*的遠端電腦上將本機環境中的*電腦*環境變數設定為*Brand1* ，請輸入：

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```

若要在本機環境中設定*MYPATH*環境變數，以使用名為*computer1*之遠端電腦上*path*環境變數中定義的搜尋路徑，請輸入：

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```

若要將本機環境中的 *TZONE* 環境變數設定為在 **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** 登錄機碼中找到的值，請輸入：

```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```

若要將名為*computer1*之遠端電腦的本機環境中的*TZONE*環境變數設定為**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname**登錄機碼中找到的值，請輸入：

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName
```

若要將系統內容中的 *組建* 環境變數設定為 **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** 登錄機碼中找到的值，請輸入：

```
setx BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber /m
```

若要在名為 Computer1 的遠端電腦系統內容中，將組建環境變數設為在 **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** 登錄機碼中找到的值，請輸入：

```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber /m
```

若要顯示名為 *Ipconfig*的檔案內容，以及內容的對應座標，請輸入：

```
setx /f ipconfig.out /x
```

若要在本機環境中，將 *IPADDR* 環境變數設定為在座標 *5、11（* 在 *Ipconfig. out* 檔案中）找到的值，請輸入：

```
setx IPADDR /f ipconfig.out /a 5,11
```

若要將本機環境中的*OCTET1*環境變數設定為在座標5找到的值 *，請*在具有分隔符號 **# $ *** 的*Ipconfig. out*檔案中，使用3。請輸入：

```
setx OCTET1 /f ipconfig.out /a 5,3 /d #$*.
```

若要將本機環境中的*IPGATEWAY*環境變數設定為在座標*0、7* （相對於*Ipconfig. Out*檔案中的*閘道*座標）中找到的值，請輸入：

```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway
```

若要在名為*computer1*的電腦上顯示*Ipconfig*檔案的內容以及內容的對應座標，請輸入：

```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: waitfor
description: Waitfor 的參考主題，它會傳送或等候系統上的信號。 **Waitfor**是用來同步處理網路上的電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1264fa3bffde303577bd56a0f1f68a6d7b2d98c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720217"
---
# <a name="waitfor"></a>waitfor



在系統上傳送或等候信號。 **Waitfor**是用來同步處理網路上的電腦。



## <a name="syntax"></a>語法

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>參數

|       參數       |                                                                                         描述                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<Computer>     | 指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。 這個參數會套用至命令中指定的所有檔案和資料夾。 |
| /u [\<Domain>\]<User> |                              使用指定之使用者帳戶的認證來執行腳本。 根據預設， **waitfor**會使用目前使用者的認證。                               |
|   /p [\<Password>]    |                                                    指定 **/u**參數中指定之使用者帳戶的密碼。                                                     |
|          /si          |                                                                        透過網路傳送指定的信號。                                                                        |
|     /t \<Timeout>     |                                              指定等候信號的秒數。 根據預設， **waitfor**會無限期地等候。                                               |
|     \<SignalName>     |                                                指定**waitfor**等候或傳送的信號。 *SignalName*不區分大小寫。                                                 |
|          /?           |                                                                             在命令提示字元顯示說明。                                                                             |

## <a name="remarks"></a>備註

-   信號名稱不能超過225個字元。 有效的字元包括 a-z、a-z、0-9 和 ASCII 擴充字元集（128-255）。
-   如果您不使用 **/s**，則會將信號廣播到網域中的所有系統。 如果您使用 **/s**，信號只會傳送到指定的系統。
-   您可以在單一電腦上執行多個**waitfor**實例，但**waitfor**的每個實例都必須等候不同的信號。 只有一個**waitfor**實例可以等候指定電腦上的指定信號。
-   您可以使用 **/si**命令列選項，以手動方式啟動信號。
-   **Waitfor**只會在執行 windows Server 2003 作業系統的 windows XP 和伺服器上執行，但它可以傳送信號給任何執行 windows 作業系統的電腦。
-   只有當電腦與傳送信號的電腦位於相同網域時，才會收到信號。
-   當您測試軟體組建時，可以使用**waitfor** 。 例如，編譯電腦可以在編譯成功完成之後，傳送信號給執行**waitfor**的數部電腦。 收到信號時，包含**waitfor**的批次檔可指示電腦立即開始安裝軟體，或在已編譯的組建上執行測試。

## <a name="examples"></a>範例

若要等到收到 espresso\build007 信號，請輸入：
```
waitfor espresso\build007
```
根據預設， **waitfor**會無限期地等候信號。

若要等候10秒，讓 espresso\compile007 信號在超時前收到，請輸入：
```
waitfor /t 10 espresso\build007
```
若要手動啟動 espresso\build007 信號，請輸入：
```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
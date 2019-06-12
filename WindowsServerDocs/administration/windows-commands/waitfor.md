---
title: waitfor
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ced4a9ef0dd7dac5f6c4fc6f171d99fa516c07
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440317"
---
# <a name="waitfor"></a>waitfor



傳送，或等候信號，在系統上。 **Waitfor**用來同步處理跨網路的電腦。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>參數

|       參數       |                                                                                         描述                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s\<電腦 >     | 指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。 此參數適用於所有檔案和命令中指定的資料夾。 |
| /u [\<網域 >\]<User> |                              執行指令碼，使用指定的使用者帳戶的認證。 根據預設， **waitfor**會使用目前使用者的認證。                               |
|   /p [\<Password>]    |                                                    指定在指定的使用者帳戶的密碼 **/u**參數。                                                     |
|          /si          |                                                                        透過網路傳送指定的訊號。                                                                        |
|     /t \<Timeout>     |                                              指定等待訊號的秒數。 根據預設， **waitfor**會無限期等待。                                               |
|     \<SignalName>     |                                                指定的訊號， **waitfor**等候，或傳送。 *SignalName*不區分大小寫。                                                 |
|          /?           |                                                                             在命令提示字元顯示說明。                                                                             |

## <a name="remarks"></a>備註

-   訊號名稱不能超過 225 個字元。 有效字元包括 a-z、 A-Z、 0-9 和 ASCII 擴充字元集 (128 到 255)。
-   如果您不要使用 **/s**，信號廣播到網域中的所有系統。 如果您使用 **/s**，訊號只會傳送到指定的系統。
-   您可以執行多個執行個體**waitfor**在單一電腦，但每個執行個體上**waitfor**必須等待一個不同的訊號。 只有一個執行個體**waitfor**可以等候指定的電腦上的指定訊號。
-   您可以使用，以手動方式啟用信號 **/si**命令列選項。
-   **Waitfor**只在 Windows XP 和執行 Windows Server 2003 作業系統的伺服器執行，但它可以將訊號傳送至任何執行 Windows 作業系統的電腦。
-   如果它們是在傳送訊號的電腦相同的網域，電腦可以只會收到訊號。
-   您可以使用**waitfor**當您測試軟體的組建。 比方說，編譯的電腦時，可以執行數個電腦傳送信號**waitfor**編譯已順利完成之後。 在收到訊號時，批次檔，包括**waitfor**可以指示要立即開始安裝軟體，或已編譯的組建上執行測試的電腦。

## <a name="BKMK_examples"></a>範例

若要等候直到收到"espresso\build007 」 訊號，請輸入：
```
waitfor espresso\build007
```
根據預設， **waitfor**無限期地等候信號。

若要等候 10 秒的接收逾時之前的"espresso\compile007 」 訊號，請輸入：
```
waitfor /t 10 espresso\build007
```
若要以手動方式啟動 「 espresso\build007 」 訊號，請輸入：
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
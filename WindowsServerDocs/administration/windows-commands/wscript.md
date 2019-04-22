---
title: wscript
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821299"
---
# <a name="wscript"></a>wscript



Windows Script Host 提供使用者可以在不同的語言，使用各種不同的物件模型來執行工作的指令碼執行所在的環境。

## <a name="syntax"></a>語法

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|scriptname|指定指令碼檔案的路徑和檔案名稱。|
|/b|指定批次模式下，不會顯示警示、 指令碼錯誤或輸入的提示。 這是相對於 **/i**。|
|/d|啟動偵錯工具。|
|/e|指定用來執行指令碼引擎。 這可讓您執行使用自訂的副檔名的指令碼。 如果沒有 /e 參數中，您只能在執行指令碼使用已註冊的檔案名稱副檔名。 例如，如果您嘗試執行此命令：<br>```cscript test.admin```<br>您會收到這個錯誤訊息：輸入的錯誤：沒有副檔名的指令碼引擎 」。 系統管理員。 」<br>使用非標準的檔案名稱副檔名的優點之一是它可防止不小心按兩下指令碼，並執行您真的不想執行。 <br>這不會建立永久關聯.admin 副檔名和 VBScript。 每次執行的指令碼使用.admin 副檔名，您必須使用 /e 參數。|
|/h:cscript|註冊**cscript.exe**做為執行指令碼的預設指令碼主機。|
|/h:wscript|註冊**wscript.exe**做為執行指令碼的預設指令碼主機。 這是預設值時 **/h**省略選項。|
|/i|指定會顯示警示、 指令碼錯誤，以及輸入的提示中的互動模式。</br>這是預設值和相反**b**。|
|/job:\<identifier>|執行所識別的作業*識別碼*中 **.wsf**指令碼檔案。|
|/logo|指定指令碼執行之前，將 Windows Script Host 橫幅會顯示在主控台中。</br>這是預設值和相反 **/nologo**。|
|/nologo|指定指令碼執行之前不會顯示 Windows Script Host 橫幅。 這是相對於 **/logo**。|
|/s|儲存目前的命令提示字元選項，為目前的使用者。|
|/t:\<數字 >|指定指令碼可以執行 （以秒為單位） 的時間上限。 您可以指定 32,767 秒。</br>預設值是沒有時間限制。|
|/x|在 偵錯工具中啟動指令碼。|
|ScriptArguments|指定的引數傳遞至指令碼。 每個指令碼引數必須加上斜線 （/）。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   執行此工作不需要具有系統管理認證。 因此，基於最佳安全做法，請考慮以不具系統管理認證的使用者身分執行此工作。
-   若要開啟命令提示字元，請在 [開始] 畫面輸入 **cmd**，然後按一下 [命令提示字元]。
-   每個參數是選擇性的;不過，您無法指定指令碼引數，而不需要指定指令碼。 如果您未指定指令碼或任何指令碼引數， **wscript.exe**會顯示**Windows 指令碼主控件設定**對話方塊中，您可以使用該設定全域指令碼屬性中的所有指令碼**wscript.exe**本機電腦上執行。
-   **/T**參數可防止過度執行指令碼，藉由設定計時器。 時的時間超過指定的值， **wscript**中斷指令碼引擎並結束程序。
-   Windows 指令碼檔案通常具有下列副檔名的其中一個： **.wsf**， **.vbs**， **.js**。
-   如果您按兩下副檔名沒有相關聯，指令碼檔案**開啟** 對話方塊隨即出現。 選取  **wscript**或是**cscript**，然後選取**永遠用來開啟此檔案類型的 這個程式**。 這會註冊**wscript.exe**或是**cscript.exe**做為此檔案類型的檔案的預設指令碼主機。
-   您可以設定個別的指令碼的屬性。 請參閱[Windows Script Host 概觀](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx)如需詳細資訊。
-   可以使用 Windows Script Host **.wsf**指令碼檔案。 每個 **.wsf**檔案可以使用多個指令碼引擎並執行多項作業。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)

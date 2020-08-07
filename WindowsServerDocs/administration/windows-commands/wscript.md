---
title: wscript
description: Wscript.echo 的參考文章，其提供的環境可讓使用者以各種不同的語言執行腳本，以使用各種不同的物件模型來執行工作。
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 4d3ab5d04423a093b280b8468c7e85aad3519dcb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896447"
---
# <a name="wscript"></a>wscript



Windows Script Host 提供一個環境，讓使用者可以使用各種不同的語言來執行腳本，以執行工作。

## <a name="syntax"></a>語法

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|scriptname|指定腳本檔案的路徑和檔案名。|
|/b|指定不會顯示警示、腳本錯誤或輸入提示的批次模式。 這與 **/i**相反。|
|/d|啟動偵錯工具。|
|/e|指定用來執行腳本的引擎。 這可讓您執行使用自訂副檔名的腳本。 如果沒有/e 參數，您只能執行使用已註冊之副檔名的腳本。 例如，如果您嘗試執行此命令：<br>```cscript test.admin```<br>您會收到此錯誤訊息：輸入錯誤：副檔名不是腳本引擎。系統管理員。<br>使用非標準的副檔名的其中一個優點是，它可以防止不小心按兩下腳本，並執行您真的不想要執行的東西。 <br>這不會建立 admin 副檔名和 VBScript 之間的永久關聯。 每次您執行使用 admin 副檔名的腳本時，都必須使用/e 參數。|
|/h： cscript|註冊**cscript.exe**做為執行腳本的預設腳本主機。|
|/h： wscript.echo|註冊**wscript.exe**做為執行腳本的預設腳本主機。 這是省略 **/h**選項時的預設值。|
|/i|指定互動模式，以顯示警示、腳本錯誤和輸入提示。</br>這是預設值，與 **/b**相反。|
|/工作 x\<identifier>|執行**manage-bde.wsf**腳本檔案中的*識別碼*所識別的作業。|
|/logo|指定在執行腳本之前，主控台中顯示 Windows 腳本主機橫幅。</br>這是預設值，與 **/nologo**相反。|
|/nologo|指定在腳本執行之前，不顯示 Windows Script 主機橫幅。 這與 **/logo**相反。|
|/s|儲存目前使用者的目前命令提示字元選項。|
|/t:\<number>|指定腳本可執行檔時間上限 (以秒為單位) 。 您最多可以指定32767秒。</br>預設值為 [無時間限制]。|
|/x|啟動偵錯工具中的腳本。|
|ScriptArguments|指定傳遞至腳本的引數。 每個腳本引數前面必須加上斜線 (/) 。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   執行此工作不必有系統管理認證， 因此基於安全性最佳做法，請考慮以不具系統管理認證的使用者身分執行此工作。
-   若要開啟命令提示字元，請在 [開始]**** 畫面輸入 **cmd**，然後按一下 [命令提示字元]****。
-   每個參數都是選擇性的;不過，您不能指定腳本引數，而不指定腳本。 如果您未指定腳本或任何腳本引數， **wscript.exe**會顯示 [ **Windows script Host 設定**] 對話方塊，您可以用來設定**wscript.exe**在本機電腦上執行之所有腳本的全域腳本屬性。
-   **/T**參數會藉由設定計時器來防止過多的腳本執行。 當時間超過指定的值時， **wscript.echo**會中斷腳本引擎並結束進程。
-   Windows 腳本檔案通常會有下列其中一個副檔名： **manage-bde.wsf**、 **.vbs**、 **.js**。
-   如果您按兩下副檔名沒有關聯的腳本檔案，[**開啟方式**] 對話方塊隨即出現。 選取 [ **wscript.echo** ] 或 [ **cscript**]，然後選取 [**一律使用此程式] 來開啟此檔案類型**。 這會針對此檔案類型的檔案，註冊**wscript.exe**或**cscript.exe**做為預設的腳本主機。
-   您可以設定個別腳本的屬性。 如需詳細資訊，請參閱[Windows Script Host 總覽](/previous-versions/windows/it-pro/windows-server-2003/cc738350(v=ws.10))。
-   Windows Script Host 可以使用**manage-bde.wsf**腳本檔案。 每個**manage-bde.wsf**檔案都可以使用多個腳本引擎並執行多個作業。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

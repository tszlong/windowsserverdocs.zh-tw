---
title: wscript
description: Wscript.echo 的參考文章，其中提供的環境讓使用者可以使用各種不同的語言執行腳本，以使用各種不同的物件模型來執行工作。
ms.topic: reference
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 08/21/2018
ms.openlocfilehash: af39abab4d493e0bd4a5ed9227c68e2e2e34dc2b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628459"
---
# <a name="wscript"></a>wscript



Windows Script Host 提供的環境讓使用者可以使用各種不同的語言來執行腳本，以使用各種不同的物件模型來執行工作。

## <a name="syntax"></a>語法

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|scriptname|指定腳本檔案的路徑和檔案名。|
|/b|指定不顯示警示、腳本錯誤或輸入提示的批次模式。 這與 **/i**相反。|
|/d|啟動偵錯工具。|
|/e|指定用來執行腳本的引擎。 這可讓您執行使用自訂副檔名的腳本。 如果沒有/e 參數，您只能執行使用已註冊之副檔名的腳本。 例如，如果您嘗試執行此命令：<br>```cscript test.admin```<br>您將會收到下列錯誤訊息：輸入錯誤：副檔名沒有腳本引擎。系統管理員。<br>使用非標準副檔名的其中一個優點是，它會防止意外地按兩下腳本，並執行您不想要執行的某些工作。 <br>這不會在 admin 副檔名和 VBScript 之間建立永久關聯。 每次執行使用. admin 副檔名的腳本時，您都必須使用/e 參數。|
|/h： cscript|將 **cscript.exe** 註冊為執行腳本的預設腳本主機。|
|/h： wscript.echo|將 **wscript.exe** 註冊為執行腳本的預設腳本主機。 這是省略 **/h** 選項時的預設值。|
|/i|指定可顯示警示、腳本錯誤和輸入提示的互動模式。</br>這是預設值，相反于 **/b**。|
|工作\<identifier>|執行**w2kmiguser.wsf**腳本檔案中的*識別碼*所識別的作業。|
|/logo|指定在執行腳本之前，主控台中顯示 Windows Script Host 橫幅。</br>這是預設值，與 **/nologo**相反。|
|/nologo|指定在執行腳本之前，不會顯示 Windows Script Host 橫幅。 這與 **/logo**相反。|
|/s|為目前的使用者儲存目前的命令提示字元選項。|
|/t:\<number>|指定腳本可執行檔時間上限 (（以秒為單位）) 。 您最多可以指定32767秒。</br>預設值為 [無時間限制]。|
|/x|在偵錯工具中啟動腳本。|
|ScriptArguments|指定傳遞給腳本的引數。 每個腳本引數前面都必須加上斜線 (/) 。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   執行此工作不必有系統管理認證， 因此基於安全性最佳做法，請考慮以不具系統管理認證的使用者身分執行此工作。
-   若要開啟命令提示字元，請在 [開始]**** 畫面輸入 **cmd**，然後按一下 [命令提示字元]****。
-   每個參數都是選擇性的;不過，您不能指定腳本引數，而不需要指定腳本。 如果您未指定腳本或任何腳本引數， **wscript.exe** 會顯示 [ **Windows script Host 設定** ] 對話方塊，您可以使用此對話方塊，為 **wscript.exe** 在本機電腦上執行的所有腳本設定全域腳本屬性。
-   **/T**參數藉由設定計時器來防止腳本過多執行。 當時間超過指定的值時， **wscript.echo** 會中斷腳本引擎並結束進程。
-   Windows 腳本檔案通常會有下列副檔名之一： **. w2kmiguser.wsf**、 **.vbs**、 **.js**。
-   如果您按兩下副檔名沒有關聯的指令檔，[ **開啟** 檔案] 對話方塊隨即出現。 選取 [ **wscript.echo** ] 或 [ **cscript**]，然後選取 [ **一律使用這個程式] 來開啟此檔案類型**。 這會為此檔案類型的檔案註冊 **wscript.exe** 或 **cscript.exe** 作為預設腳本主機。
-   您可以設定個別腳本的屬性。 如需詳細資訊，請參閱 [Windows Script Host 總覽](/previous-versions/windows/it-pro/windows-server-2003/cc738350(v=ws.10)) 。
-   Windows Script Host 可以使用 **. w2kmiguser.wsf** 腳本檔案。 每個 **w2kmiguser.wsf** 檔案都可以使用多個腳本引擎並執行多個作業。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: cscript
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffdbd6f67e4e4c32022134191deabd861bf248b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827669"
---
# <a name="cscript"></a>cscript

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

啟動指令碼，使其在命令列環境中執行。
## <a name="syntax"></a>語法
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|Scriptname.extension|指定指令碼檔案的路徑和檔案名稱具有選擇性副檔名。|
|/B|指定批次模式下，不會顯示警示、 指令碼錯誤或輸入的提示。|
|/D|啟動偵錯工具。|
|/ E:<Engine>|指定用來執行指令碼引擎。|
|/H:cscript|註冊是預設指令碼主控件的 cscript.exe 來執行指令碼。|
|/H:wscript|暫存器 wscript.exe 做為執行指令碼的預設指令碼主機。 這是預設值。|
|/I|指定會顯示警示、 指令碼錯誤，以及輸入的提示中的互動模式。 這是預設值和相反**b**。|
|/ 作業：<Identifier>|執行所識別的作業*識別碼*.wsf 指令碼檔案中。|
|/ 標誌|指定指令碼執行之前，將 Windows Script Host 橫幅會顯示在主控台中。 這是預設值和相反 **/Nologo**。|
|/Nologo|指定指令碼執行之前不會顯示 Windows Script Host 橫幅。|
|/S|儲存目前的命令提示字元選項，為目前的使用者。|
|/ /T:<Seconds>|指定指令碼可以執行 （以秒為單位） 的時間上限。 您可以指定 32,767 秒。 預設值是沒有時間限制。|
|/U|指定輸入和輸出會從主控台重新導向的 Unicode。|
|/X|在 偵錯工具中啟動指令碼。|
|/?|顯示可用的命令參數，並提供說明使用它們。 這是與輸入相同**cscript.exe**沒有任何參數，沒有任何指令碼。|
|ScriptArguments|指定的引數傳遞至指令碼。 每個指令碼引數必須加上斜線 (**/**)。|
### <a name="remarks"></a>備註
-   執行此工作不需要具有系統管理認證。 因此，基於最佳安全做法，請考慮以不具系統管理認證的使用者身分執行此工作。
-   若要開啟命令提示字元中，在**開始**畫面上，輸入**cmd**，然後按一下**命令提示字元**。
-   每個參數是選擇性的;不過，您無法指定指令碼引數，而不需要指定指令碼。 如果您未指定指令碼或任何指令碼引數，cscript.exe 顯示 cscript.exe 語法和有效的主機選項。
-   **/T**參數可防止過度執行指令碼，藉由設定計時器。 當執行的階段超過指定的值時，cscript 中斷指令碼引擎，並結束程序。
-   Windows 指令碼檔案通常具有下列副檔名的其中一個：.wsf、.vbs、.js。
-   您可以設定個別的指令碼的屬性。 如需詳細資訊，請參閱相關主題。
-   Windows 指令碼主機可以使用.wsf 指令碼檔案。 每個.wsf 檔案可以使用多個指令碼引擎，並執行多項作業。
-   如果您按兩下副檔名沒有相關聯，指令碼檔案**開啟** 對話方塊隨即出現。 選取 wscript 或 cscript，然後選取**永遠用來開啟此檔案類型的 這個程式**。 這會註冊 wscript.exe 或 cscript 做為此檔案類型的檔案的預設指令碼主機。
-   您可以設定個別的指令碼的屬性。 請參閱[其他參考](#BKMK_references)如需詳細資訊。
-   Windows 指令碼主機可以使用.wsf 指令碼檔案。 每個.wsf 檔案可以使用多個指令碼引擎，並執行多項作業。

#### <a name="BKMK_references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

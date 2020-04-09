---
title: cscript
description: Cscript 的 Windows 命令主題，它會啟動腳本，讓它在命令列環境中執行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fc82e1203f81ed966beb8e3906ce95493265195
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846771"
---
# <a name="cscript"></a>cscript

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動腳本，使其在命令列環境中執行。

## <a name="syntax"></a>語法
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
#### <a name="parameters"></a>參數

|      參數       |                                                                      描述                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Scriptname 擴充功能 |                                 指定具有選擇性副檔名之腳本檔案的路徑和檔案名。                                 |
|          /B          |                                指定不會顯示警示、腳本錯誤或輸入提示的批次模式。                                |
|          /D          |                                                                  啟動偵錯工具。                                                                  |
|     /E：<Engine>      |                                                  指定用來執行腳本的引擎。                                                  |
|      /H： cscript      |                                         將 cscript.exe 註冊為執行腳本的預設腳本主機。                                          |
|      /H： wscript.echo      |                               將 wscript.echo 註冊為執行腳本的預設腳本主機。 這是預設值。                               |
|          /I          |        指定互動模式，以顯示警示、腳本錯誤和輸入提示。 這是預設值，與 **/b**相反。         |
|  /工作 x：<Identifier>   |                                             執行 manage-bde.wsf 腳本檔案中的*識別碼*所識別的作業。                                             |
|        /Logo         | 指定在執行腳本之前，主控台中顯示 Windows 腳本主機橫幅。 這是預設值，與 **/nologo**相反。 |
|       /Nologo        |                                 指定在腳本執行之前，不顯示 Windows Script 主機橫幅。                                 |
|          /S          |                                             儲存目前使用者目前的命令提示字元選項。                                             |
|     /T：<Seconds>     |            指定腳本可執行檔時間上限（以秒為單位）。 您最多可以指定32767秒。 預設值為 [無時間限制]。             |
|          U          |                                      針對從主控台重新導向的輸入和輸出，指定 Unicode。                                       |
|          /X          |                                                           啟動偵錯工具中的腳本。                                                           |
|          /?          |  顯示可用的命令參數，並提供使用它們的協助。 這與輸入不含參數且沒有腳本的**cscript.exe**相同。  |
|   ScriptArguments    |                        指定傳遞至腳本的引數。 每個腳本引數前面必須加上斜線（ **/** ）。                         |

### <a name="remarks"></a>備註
-   您不需要系統管理認證就能執行這項工作。 因此，基於最佳安全作法，請考慮以不具系統管理認證的使用者身分執行此工作。
-   若要開啟命令提示字元，請在 [**開始**] 畫面上輸入**cmd**，然後按一下 [**命令提示**字元]。
-   每個參數都是選擇性的;不過，您不能指定腳本引數，而不指定腳本。 如果您未指定腳本或任何腳本引數，cscript.exe 會顯示 cscript.exe 語法和有效的主機選項。
-   **/T**參數會藉由設定計時器來防止過多的腳本執行。 當執行時間超過指定的值時，cscript 會中斷腳本引擎並結束進程。
-   Windows 腳本檔案通常會有下列其中一個副檔名： manage-bde.wsf、.vbs、.js。
-   您可以設定個別腳本的屬性。 如需詳細資訊，請參閱相關主題。
-   Windows Script Host 可以使用 manage-bde.wsf 腳本檔案。 每個 manage-bde.wsf 檔案都可以使用多個腳本引擎並執行多個作業。
-   如果您按兩下副檔名沒有關聯的腳本檔案，[**開啟方式**] 對話方塊隨即出現。 選取 [wscript.echo] 或 [cscript]，然後選取 [**一律使用此程式] 來開啟此檔案類型**。 這會為此檔案類型的檔案註冊 wscript.echo 或 cscript 做為預設的腳本主機。
-   您可以設定個別腳本的屬性。 如需詳細資訊，請參閱[其他參考](#BKMK_references)。
-   Windows Script Host 可以使用 manage-bde.wsf 腳本檔案。 每個 manage-bde.wsf 檔案都可以使用多個腳本引擎並執行多個作業。

#### <a name="additional-references"></a><a name=BKMK_references></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

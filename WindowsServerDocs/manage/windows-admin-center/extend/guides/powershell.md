---
title: 在擴充功能中使用 PowerShell
description: 在您的擴充 Windows Admin Center SDK （專案檀香山） 中使用 PowerShell
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867599"
---
# <a name="using-powershell-in-your-extension"></a>在擴充功能中使用 PowerShell #

>適用於：Windows Admin Center，Windows Admin Center 預覽

我們要進入更深入 Windows Admin Center 擴充功能 SDK-讓我們來談談將 PowerShell 命令新增至您的延伸模組。

## <a name="powershell-in-typescript"></a>在 TypeScript 中的 PowerShell ##

Gulp 建置程序已將任何".ps1"放在"/ src/資源/scripts"的資料夾，並建置成"/ src/產生"資料夾下的 「 powershell 指令碼 」 類別的產生步驟。

>[!NOTE] 
> 不以手動方式更新 「 powershell-scripts.ts"或"strings.ts 」 檔案。 您進行任何變更都將在下一步 產生覆寫。

### <a name="adding-your-own-powershell-script"></a>新增您自己的 PowerShell 指令碼 ##

我們將新增即將新增您自己的 PowerShell 指令碼的相關資訊。

### <a name="managing-powershell-sessions"></a>管理 PowerShell 工作階段 ###

當您使用在 Windows Admin Center 中使用 PowerShell 時，工作階段是在指令碼執行程序的必要的元件。 若要執行指令碼遠端受管理的伺服器上，PowerShell 會使用的 runspace。 Windows Admin Center 會包裝 PowerShellSession 物件管理存留期，並讓循序的指令碼執行的 runspace 重複使用中 runspace 建立和管理。

每個元件必須建立 AppContextService 類別使用三個不同的選項所建立的工作階段物件的參考：
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

藉由提供 createSession 方法的節點名稱，新的 PowerShell 工作階段是建立、 使用以及然後 PowerShell 呼叫完成時立即終結。 這項功能很適合單次呼叫，但重複的使用可能會影響效能。 工作階段會建立大約 1 秒，因此持續回收工作階段可能會導致速度變慢。

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

第一個選擇性參數\<工作階段金鑰\>參數。 這會建立索引的工作階段可以查閱和重複使用，即使是跨元件 （亦即 Component1 可以具有索引鍵"SME-ROCKS，「 建立工作階段，和 Component2 可以使用該相同的工作階段）。  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>常見的模式 ###

在大部分情況下，建立索引的工作階段中**ngOnInit**方法，並在然後處置**ngOnDestroy**。 當有多個元件，但不是在元件之間共用的基礎工作階段中的 PowerShell 指令碼時，請遵循這個模式。

為了獲得最佳結果，請確定內部元件，而不是服務管理工作階段建立-這有助於確保該存留期和清除可正確管理。
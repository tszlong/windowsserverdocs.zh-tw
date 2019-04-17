---
title: 在擴充功能中使用 PowerShell
description: 使用 PowerShell 在擴充 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296720"
---
# 在擴充功能中使用 PowerShell #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們移至 Windows Admin Center 擴充功能 SDK 更深入-讓我們談談將 PowerShell 命令新增至您的擴充功能。

## 在 TypeScript PowerShell ##

Gulp 使用建置程序已採取任何產生步驟```{!ScriptName}.ps1```放在```\src\resources\scripts```資料夾並建置組合成```powershell-scripts```底下類別```\src\generated```資料夾。

>[!NOTE] 
> 不要手動更新```powershell-scripts.ts```和```strings.ts```檔案。 將覆寫您所做的任何變更，在下一步產生。

## 執行 Powerhell 指令碼 ##
任何您想要在節點上執行的指令碼可以放在```\src\resources\scripts\{!ScriptName}.ps1```。 
>[!IMPORTANT] 
> 中所做的任何變更```{!ScriptName}.ps1```檔案將不會反映於您的專案除非產生 

API 運作方式是第一個是目標、 使用中，傳遞需要的任何參數建立 PowerShell 指令碼，然後上所建立的工作階段中執行指令碼在節點上建立的 PowerShell 工作階段。

例如，我們有這個指令碼```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我們將會建立我們的目標節點的 PowerShell 工作階段：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然後我們將會建立 PowerShell 指令碼的輸入參數：
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
最後，我們需要在我們所建立的工作階段中執行該指令碼：
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
現在，我們需要訂閱可觀察我們剛建立的函式。 放置這需要呼叫的函式來執行 PowerShell 指令碼：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
藉由提供給 Useractivity 方法的節點名稱，新的 PowerShell 工作階段會建立、 使用，並立即終結 PowerShell 呼叫完成時。 

### 索引鍵的選項 ###
PowerShell API 時提供幾個選項。 每次建立工作階段時也可以建立使用或不使用索引鍵。 

**金鑰：** 這會建立索引的工作階段，可以查閱並重複使用，甚至是在元件 （亦 Component1 可以使用金鑰 」 SME 岩石 」，建立工作階段，並 Component2 可以使用該相同的工作階段） 之間。如果提供索引鍵，就建立的工作階段必須先處置藉由呼叫 dispose （） 為已完成上述範例中。 不超過 5 分鐘處置的情況下不應保留在工作階段。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Keyless:** 工作階段時，會自動建立索引鍵。 與此工作階段 3 分鐘後自動處置。 使用 keyless，可讓您要回收的任何 Runspace 已在建立工作階段時使用的擴充功能。 如果沒有 Runspace 比將會建立一個新的使用。 這項功能的有效期為一次性通話，但重複的使用可能會影響效能。 在工作階段來建立約 1 秒，因此會持續回收工作階段可能會造成變慢。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大多數情況下，建立索引的工作階段中```ngOnInit()```方法，並在它的然後 dispose ```ngOnDestroy()```。 有多個元件，但基礎不是元件之間共用的工作階段中的 PowerShell 指令碼時，請遵循這個模式。
為獲得最佳結果，請確定元件，而不是服務內管理工作階段建立-這有助於確保該存留期，並妥善管理清理。

為獲得最佳結果，請確定元件，而不是服務內管理工作階段建立-這有助於確保該存留期，並妥善管理清理。

### PowerShell 資料流 ###
如果您有長時間執行的指令碼和資料被 outputted 逐漸，PowerShell 資料流可讓您處理資料，而不需要等待完成此指令碼。 儘速接收資料，就會呼叫可觀察 next()。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### 長時間執行的指令碼 ###
如果您有您想要在背景中執行長時間執行指令碼，就可以提交工作項目。 閘道將追蹤指令碼的狀態並更新狀態可以傳送通知。 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> 要顯示的進度，寫入進度必須包含在您撰寫的指令碼。 例如：
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### 工作項目選項 ####

| function | 說明 |
| ----- | ----------- |
| submit() | 提交工作項目 
| submitAndWait() | 提交工作項目，並等待完成其執行的狀態
| wait() | 等待現有的工作項目來完成
| query() | 查詢的現有的工作項目依識別碼
| find() | 尋找並現有的工作項目，藉由 TargetNodeName、 ModuleName 或 typeId。

### PowerShell 批次 Api # # #
如果您需要在多個節點上執行相同的指令碼，可以使用批次的 PowerShell 工作階段。 例如：
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### PowerShellBatch 選項 ####
| 選項 | 說明 |
| ----- | ----------- |
| runSingleCommand | 針對陣列中的所有節點執行單一命令 
| 執行 | 在已配對的節點上執行對應的命令
| 取消 | 取消陣列中的所有節點上命令
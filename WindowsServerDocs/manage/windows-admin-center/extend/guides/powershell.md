---
title: 在擴充功能中使用 PowerShell
description: 在您的擴充 Windows Admin Center SDK （專案檀香山） 中使用 PowerShell
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621363"
---
# <a name="using-powershell-in-your-extension"></a>在擴充功能中使用 PowerShell #

>適用於：Windows Admin Center，Windows Admin Center 預覽

我們要進入更深入 Windows Admin Center 擴充功能 SDK-讓我們來談談將 PowerShell 命令新增至您的延伸模組。

## <a name="powershell-in-typescript"></a>在 TypeScript 中的 PowerShell ##

Gulp 建置程序已產生步驟，以接受任何```{!ScriptName}.ps1```，位於```\src\resources\scripts```資料夾，並建置其```powershell-scripts```類別下```\src\generated```資料夾。

>[!NOTE] 
> 不要以手動方式更新```powershell-scripts.ts```和```strings.ts```檔案。 您進行任何變更都將在下一步 產生覆寫。

## <a name="running-a-powershell-script"></a>執行 PowerShell 指令碼 ##
您想要在節點上執行任何指令碼可以放在```\src\resources\scripts\{!ScriptName}.ps1```。 
>[!IMPORTANT] 
> 中所做的任何變更```{!ScriptName}.ps1```檔案將不會反映在您的專案，直到```gulp generate```已執行。

API 的運作方式是先建立 PowerShell 工作階段是目標，建立 PowerShell 指令碼的任何參數，必須在中，傳遞及再執行所建立的工作階段中的 指令碼在節點上。

比方說，我們有這個指令碼```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我們將建立我們的目標節點的 PowerShell 工作階段：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然後我們將建立 PowerShell 指令碼的輸入參數：
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
最後，我們需要建立工作階段中執行該指令碼：
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
現在，我們必須訂閱 observable 我們剛剛建立的函式。 將此資訊是在您要呼叫的函式，以執行 PowerShell 指令碼：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
藉由提供 createSession 方法的節點名稱，新的 PowerShell 工作階段是建立、 使用以及然後 PowerShell 呼叫完成時立即終結。 

### <a name="key-options"></a>金鑰選項 ###
呼叫 PowerShell API 時，可以使用幾個選項。 每次建立工作階段時可以建立包含或不含索引鍵。 

**索引鍵：** 這會建立索引的工作階段可以查閱和重複使用，即使是跨元件 （亦即 Component1 可以具有索引鍵"SME-ROCKS，「 建立工作階段，和 Component2 可以使用該相同的工作階段）。如果提供的索引鍵，則會建立工作階段必須由處置呼叫 dispose （） 所做的是在上述範例中。 正在處置的超過 5 分鐘，應該不會保留工作階段。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**無索引鍵：** 工作階段時，會自動建立索引鍵。 在本課程的 3 分鐘後自動處置。 使用無索引鍵，可讓您的延伸模組，回收使用的任何已建立工作階段時的 Runspace。 如果沒有 Runspace 可以使用比會建立一個新的。 這項功能很適合單次呼叫，但重複的使用可能會影響效能。 工作階段會建立大約 1 秒，因此持續回收工作階段可能會導致速度變慢。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大部分情況下，建立索引的工作階段中```ngOnInit()```方法，並在然後處置```ngOnDestroy()```。 當有多個元件，但不是在元件之間共用的基礎工作階段中的 PowerShell 指令碼時，請遵循這個模式。
為了獲得最佳結果，請確定內部元件，而不是服務管理工作階段建立-這有助於確保該存留期和清除可正確管理。

為了獲得最佳結果，請確定內部元件，而不是服務管理工作階段建立-這有助於確保該存留期和清除可正確管理。

### <a name="powershell-stream"></a>PowerShell Stream ###
如果您有長時間執行的指令碼和資料會輸出以漸進的方式，PowerShell 資料流可讓您處理資料，而不必等候完成的指令碼。 一旦收到資料時，就會呼叫可觀察的 next （）。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>長時間執行的指令碼 ###
如果您有長時間執行的指令碼，您想要在背景中執行時，就可以提交的工作項目。 閘道將追蹤的指令碼的狀態和狀態的更新可以傳送通知。 
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
> 若要顯示的進度，Write-progress 必須包含在您撰寫的指令碼。 例如: 
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>工作項目選項 ####

| function | 說明 |
| ----- | ----------- |
| submit() | 提交工作項目 
| submitAndWait() | 提交工作項目，並等候完成其執行
| wait() | 等待現有的工作完成的項目
| query() | 依識別碼的現有工作項目查詢
| find() | 尋找和現有 TargetNodeName、 模組名稱，或 typeId 的工作項目。

### <a name="powershell-batch-apis"></a>PowerShell Batch Api ###
如果您需要在多個節點上執行相同的指令碼，則可以使用批次的 PowerShell 工作階段。 例如: 
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


#### <a name="powershellbatch-options"></a>PowerShellBatch 選項 ####
| 選項 | 說明 |
| ----- | ----------- |
| runSingleCommand | 針對陣列中的所有節點執行單一命令 
| 執行 | 配對的節點上執行對應的命令
| 取消 | 取消命令陣列中的所有節點上

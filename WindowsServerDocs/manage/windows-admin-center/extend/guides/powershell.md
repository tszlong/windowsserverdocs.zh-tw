---
title: 在擴充功能中使用 PowerShell
description: 在擴充功能 Windows 管理中心 SDK 中使用 PowerShell （Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c30f8a9b856db8250a16210931e6f8dd73c07aa7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869614"
---
# <a name="using-powershell-in-your-extension"></a>在擴充功能中使用 PowerShell #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們更深入瞭解 Windows 管理中心延伸模組 SDK，讓我們來討論如何將 PowerShell 命令新增至您的擴充功能。

## <a name="powershell-in-typescript"></a>TypeScript 中的 PowerShell ##

Gulp 組建```{!ScriptName}.ps1```程式有一個 [ ```\src\resources\scripts```產生] 步驟，其會採用放置於資料夾中的任何， ```powershell-scripts```並將其建立```\src\generated```到資料夾下的類別中。

>[!NOTE] 
> 不要手動更新```powershell-scripts.ts``` ```strings.ts```或檔案。 下一次產生時，將會覆寫您所做的任何變更。

## <a name="running-a-powershell-script"></a>執行 PowerShell 腳本 ##
您想要在節點上執行的任何腳本都可以放在```\src\resources\scripts\{!ScriptName}.ps1```中。 
>[!IMPORTANT] 
> 在檔案中```{!ScriptName}.ps1```進行的任何變更都不會反映在您的```gulp generate```專案中，直到執行為止。

API 的運作方式是先在您目標的節點上建立 PowerShell 會話，並使用任何需要傳入的參數來建立 PowerShell 腳本，然後在已建立的會話上執行腳本。

例如，我們有下列腳本```\src\resources\scripts\Get-NodeName.ps1```：
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

我們會為目標節點建立 PowerShell 會話：
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
然後，我們將使用輸入參數來建立 PowerShell 腳本：
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
最後，我們需要在我們所建立的會話中執行該腳本：
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
現在，我們必須訂閱剛建立的可觀察函式。 將此位置放在您需要呼叫函式來執行 PowerShell 腳本的位置：
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
藉由提供節點名稱給 createSession 方法，會建立並使用新的 PowerShell 會話，然後在 PowerShell 呼叫完成時立即予以終結。 

### <a name="key-options"></a>索引鍵選項 ###
呼叫 PowerShell API 時，有幾個選項可供使用。 每次建立會話時，都可以使用或不搭配金鑰來建立。 

**擊鍵**這會建立一個可以查閱和重複使用的金鑰會話，甚至是跨元件（這表示 Component1 可以建立具有金鑰 "ROCKS" 的會話，而 Component2 可以使用相同的會話）。如果提供了金鑰，則必須藉由呼叫 dispose （）來處置所建立的會話，如同上述範例中所做的一樣。 會話不應在未被處置超過5分鐘的情況下保留。 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**無索引鍵**系統會自動為會話建立一個金鑰。 此會話會在3分鐘後自動處置。 使用無索引鍵可讓您的擴充功能回收在建立會話時已可使用的任何執行時間。 如果沒有可用的運行時，將會建立一個新的。 這項功能適用于單次呼叫，但重複使用可能會影響效能。 會話大約需要1秒鐘的時間來建立，因此持續回收會話可能會導致速度變慢。

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
或 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
在大部分的```ngOnInit()```情況下，請在方法中建立金鑰會話，然後在中```ngOnDestroy()```處置它。 當元件中有多個 PowerShell 腳本，但基礎會話並未在元件之間共用時，請遵循此模式。
為了獲得最佳結果，請確定會話建立是在元件（而非服務）內進行管理，這有助於確保存留期和清除可以正確管理。

為了獲得最佳結果，請確定會話建立是在元件（而非服務）內進行管理，這有助於確保存留期和清除可以正確管理。

### <a name="powershell-stream"></a>PowerShell 資料流程 ###
如果您有長時間執行的腳本，且資料會逐漸輸出，PowerShell 資料流程可讓您處理資料，而不需要等候腳本完成。 一旦收到資料，就會立即呼叫可觀察的 next （）。
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>長時間執行的腳本 ###
如果您有想要在背景中執行的長時間腳本，可以提交工作專案。 腳本的狀態將會由閘道追蹤，而狀態的更新則可傳送至通知。 
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
> 若要顯示進度，寫入進度必須包含在您所撰寫的腳本中。 例如:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>工作專案選項 ####

| function | 說明 |
| ----- | ----------- |
| submit （） | 提交工作專案 
| submitAndWait() | 提交工作專案，並等候其執行完成
| wait （） | 等待現有的工作專案完成
| 查詢（） | 依識別碼查詢現有的工作專案
| 尋找（） | 依 TargetNodeName、ModuleName 或 typeId 尋找和現有的工作專案。

### <a name="powershell-batch-apis"></a>PowerShell Batch Api ###
如果您需要在多個節點上執行相同的腳本，則可以使用 batch PowerShell 會話。 例如:
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
| 件 | 說明 |
| ----- | ----------- |
| runSingleCommand | 針對陣列中的所有節點執行單一命令 
| 進行 | 在配對的節點上執行對應的命令
| 取消 | 在陣列中的所有節點上取消命令

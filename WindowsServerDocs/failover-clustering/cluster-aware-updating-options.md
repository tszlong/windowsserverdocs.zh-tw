---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: "光彩感知更新進階選項] 並執行的設定檔的更新"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: "如何設定 [進階的選項]，以及適用於叢集更新 (CAU) 更新執行設定檔"
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>進階選項]，然後執行設定檔的更新叢集更新

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述更新執行選項，您可以設定為[更新叢集](cluster-aware-updating.md)(CAU) 更新會執行。 當您使用 CAU UI 或 CAU Windows PowerShell cmdlet 適用的更新，或是設定自動更新的選項，就可以設定這些 [進階的選項]。

大部分的設定可以儲存 XML 檔案稱為更新執行設定檔，並針對較新的更新執行其他。 也可以叢集您的環境中使用所提供的 CAU 更新執行選項的預設值。

對更新執行設定檔及其他選項，您可以指定每個更新執行的相關資訊，請查看下列本主題稍後的區段：

指定當您要求更新執行使用更新執行設定檔的選項，可以設定更新執行設定檔的選項

下表列出您可以設定 CAU 更新執行設定檔的選項。 

> [!NOTE] 
> 若要設定 PreUpdateScript 或 PostUpdateScript 選項，請確定已安裝 Windows PowerShell 和.NET Framework 4.6 或 4.5 且遠端 PowerShell 可以在每個節點叢集中。 如需詳細資訊，請查看設定中的遠端管理節點[需求與最佳做法叢集更新](cluster-aware-updating-requirements.md)。


|選項|預設值。|詳細資料|  
|------------|-------------------|-------------|  
|**StopAfter**|無限制的時間|之後，更新執行將會停止如果尚未完成的分鐘的時間。 **注意：**的執行指令碼，並執行更新整個程序如果指定更新前或更新後 PowerShell 指令碼，必須先完成在**StopAfter**的時間限制。|  
|**WarnAfter**|根據預設，不出現任何警告|之後，如果尚未完成更新執行（包括更新前指令碼及更新後指令碼，如果設定），會顯示警告分鐘的時間。|  
|**MaxRetriesPerNode**|3|最大的次數（包括更新前指令碼及更新後指令碼，如果設定）更新程序將會重試每個節點。 最大值是 64。|  
|**MaxFailedNodes**|針對大部分叢集，大約一位第三個叢集節點數目的整數|在哪一個更新可能會失敗，可能是因為節點失敗或執行叢集服務停止節點的上限。 如果有一個更多節點失敗，就會停止更新執行。<br /><br /> 值的有效範圍是 0 到 1 小於叢集節點數目。|  
|**RequireAllNodesOnline**|無|指定所有節點都必須 online，並開始更新之前，您可取得。|  
|**RebootTimeoutMinutes**|15|重新節點（如果重新開機必要）和開始自動 [開始] 畫面的所有服務允許 CAU 分鐘的時間。 如果在重新開機程序未完成在這次，更新執行該節點標示為失敗。|  
|**PreUpdateScript**|無|PowerShell 指令碼執行每個節點上開始更新之前，和之前節點置於維護模式路徑和檔案名稱。 必須副檔名**.ps1**，，路徑加上檔案名稱的總長度不可超過 260 個字元。 做為最佳做法，指令碼應該位於叢集存放裝置，或，以確保都可以存取所有叢集節點高度可用的網路檔案共用磁碟上。 如果指令碼位於檔案共用網路，請確認您設定朗讀您請求權限的檔案共用群組，每個人，然後限制以防止未經授權的使用者的檔案所竄改寫入權限。<br /><br /> 如果指定更新前指令碼，請確定您的設定，例如與的時間限制 (例如，**StopAfter**) 允許指令碼已成功執行設定。 這些限制跨越執行指令碼，並安裝更新，而不只安裝更新的處理程序整個程序。|  
|**PostUpdateScript**|無|PowerShell 指令碼執行（之後節點離開維護模式）更新完成之後路徑和檔案名稱。 必須副檔名**.ps1**，路徑加上檔案名稱的總長度不可超過 260 個字元。 做為最佳做法，指令碼應該位於叢集存放裝置，或，以確保都可以存取所有叢集節點高度可用的網路檔案共用磁碟上。 如果指令碼位於檔案共用網路，請確認您設定朗讀您請求權限的檔案共用群組，每個人，然後限制以防止未經授權的使用者的檔案所竄改寫入權限。<br /><br /> 如果指定更新後指令碼，請確定您的設定，例如與的時間限制 (例如，**StopAfter**) 允許指令碼已成功執行設定。 這些限制跨越執行指令碼，並安裝更新，而不只安裝更新的處理程序整個程序。|  
|**ConfigurationName**|這個設定只效果如果您執行指令碼。<br /><br /> 若您指定前更新或更新後指令碼，但是您不指定**ConfigurationName**，使用 PowerShell (Microsoft.PowerShell) 設定預設工作階段。|指定 PowerShell 工作階段組態定義工作階段中的指令碼 (指定的**PreUpdateScript**和**PostUpdateScript**) 執行，並可以限制的可執行的命令。|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|外掛程式您設定的使用預覽更新或執行更新執行叢集更新。 如需詳細資訊，請查看[運作方式叢集更新插](cluster-aware-updating-plug-ins.md)。|  
|**CauPluginArguments**|無|一組*名稱 = 值*以插件，以使用，例如更新配對（引數）：<br /><br /> **Domain=Domain.local**<br /><br /> 這些*名稱 = 值*配對必須意義外掛程式您在指定的**CauPluginName**。<br /><br /> 若要指定引數使用 CAU UI，輸入*名稱*，長按 Tab 鍵，然後輸入適當的對應*值*。 按下 Tab 鍵，再試一次，以提供的下一步引數。 每個*名稱*和*值*會自動以等號（=）。 多個配對會自動以分號。<br /><br /> 預設值的**Microsoft.WindowsUpdatePlugin**會需要外掛程式，不引數。 不過，您可以指定選擇性引數，例如指定標準 Windows Update 代理程式查詢字串篩選更新外掛程式套用的設定。 適用於*名稱*，使用**查詢**，以及*值*，引號住完整查詢。<br /><br /> 如需詳細資訊，請查看[運作方式叢集更新插](cluster-aware-updating-plug-ins.md)。|  
  
##  <a name="BKMK_runtime"></a>指定您要求更新執行選項  
 下表列出您要求更新執行時，您可以指定（以外這些更新執行設定檔）的選項。 您可以設定更新執行設定檔的選項的相關資訊，會看到上述表格。  
  
|選項|預設值。|詳細資料|  
|------------|-------------------|-------------|  
|**ClusterName**|無 <br>**注意：** CAU UI 不執行容錯移轉叢集節點中，或您想要參考容錯移轉叢集不同 CAU UI 在何處執行時才必須設定此選項。|在其上執行更新執行叢集 NetBIOS 名稱。|  
|**認證**|目前 account 認證|管理權限目標叢集更新執行將會執行。 您可能已經必要的認證如果開始 CAU UI（或打開 PowerShell 工作階段，如果您正在使用 CAU PowerShell cmdlet）叢集具有系統管理員權限 account 從。|  
|**NodeOrder**|根據預設，CAU 開頭擁有小數目叢集的角色，然後進行到的第二小的數字和等等節點節點。|叢集節點，其應該更新（如果可能的話）的順序的名稱。|  
  
##  <a name="BKMK_profile"></a>使用更新執行設定檔  
 每個更新執行可以相關聯的特定更新執行的設定檔。 預設的更新執行的個人檔案儲存在*%windir%\cluster*資料夾。 如果您使用 CAU UI 遠端更新模式，您可以指定更新執行設定檔，適用於更新的時間，或者您可以使用的預設更新執行設定檔。 如果您正在使用 CAU 自我更新模式中，您可以匯入的設定指定更新執行的設定檔當您在設定自動更新的選項。 在這兩個案例中，您可以依據您的需求覆寫顯示的值對更新執行選項。 如果您想，您可以將儲存的更新執行選項更新執行設定檔相同的檔案名稱或其他檔案名稱。 下次您適用的更新，或設定自動更新的選項，CAU 自動選取更新執行設定檔先前選取。  
  
 您可以修改現有更新執行的設定檔，或建立新的 homegroup，選取 [**建立修改更新執行設定檔或**在 CAU UI 中。

以下是一些有關使用更新執行設定檔的重要事項：

* 更新執行設定檔不會儲存叢集特定資訊，例如系統管理員認證。 如果您使用 CAU 自我更新模式，請更新執行設定檔也不會儲存自我更新排程資訊。 這可讓您可以在所有容錯指定課程中分享更新執行設定檔。
* 如果您在設定自動更新使用更新執行設定檔的選項稍後修改使用不同的值對更新執行選項的設定檔，不會自動變更設定自動更新。 若要套用新的更新執行設定，您必須再試一次設定自動更新的選項。
* 執行設定檔編輯器很可惜不支援的檔案路徑包含空間，例如*C:\Program 檔案*。 因應措施可用，將您前和更新指令碼文章中不包含空間，或使用 PowerShell 專屬執行設定檔，將路徑引號執行時的路徑**叫用-CauRun**。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 相同的命令
  
 當您執行時，您可以會匯入設定更新執行設定檔**叫用-CauRun**，**新增-CauClusterRole**，或**設定-CauClusterRole** cmdlet。  
  
 下列範例執行掃描完整更新上並執行叢集名為*CONTOSO-FC1*，使用更新執行選項中指定的*C:\Windows\Cluster\DefaultParameters.xml*。 剩餘的 cmdlet 參數會使用預設值。  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 使用更新執行設定檔，您可以設定一致例外管理、時間範圍，以及其他操作參數更新容錯移轉叢集中重複的方式。 因為通常是一種容錯特定這些設定，例如「所有 Microsoft SQL Server 叢集」或「我關鍵性叢集」，您可能想要為每個更新執行設定檔依據種容錯它將會搭配。 此外，您可能想要管理更新執行上的設定檔的無障礙的容錯特定課程 IT 組織中的所有檔案共用。  
  
  
  
## <a name="see-also"></a>也了

-   [叢集更新](cluster-aware-updating.md)
  
-   [Windows PowerShell 中的叢集更新 Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)
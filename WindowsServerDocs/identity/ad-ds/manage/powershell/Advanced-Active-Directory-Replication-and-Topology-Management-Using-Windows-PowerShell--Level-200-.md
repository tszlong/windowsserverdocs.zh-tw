---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: 使用 Windows PowerShell 進行進階的 Active Directory 複寫和拓撲管理 (層級 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: eeac84fb4e875ffe31b560bc72190895cd0527bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402672"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>使用 Windows PowerShell 進行進階的 Active Directory 複寫和拓撲管理 (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此主題詳細說明新的 AD DS 複寫和拓撲管理 Cmdlet，並提供額外的範例。 如需簡介，請參閱[使用 Windows PowerShell &#40;層級 100&#41;的 Active Directory 複寫和拓撲管理簡介](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)。  
  
1.  [簡介](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [複寫和中繼資料](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [ADReplicationQueueOperation 和 ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [同步處理-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [拓](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>問世  
Windows Server 2012 對「適用於 Windows PowerShell 的 Active Directory 模組」擴充了 25 個新的 Cmdlet 來管理複寫和樹系拓撲。 在此之前，您已強制使用一般 **\*AdObject**名詞或呼叫 .net 函數。  
  
就像所有 Active Directory Windows PowerShell Cmdlet 一樣，此功能必須至少在一部網域控制站 (或者最好在所有網域控制站) 安裝 [Active Directory 管理閘道服務](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) 。  
  
下表列出 Active Directory Windows PowerShell 模組新增的複寫和拓撲 Cmdlet。  
  
|||  
|-|-|  
|Cmdlet|說明|  
|Get-ADReplicationAttributeMetadata|傳回物件的屬性複寫中繼資料|  
|Get-ADReplicationConnection|傳回網域控制站連線物件詳細資料|  
|Get-ADReplicationFailure|傳回網域控制站最近的複寫失敗|  
|Get-ADReplicationPartnerMetadata|傳回網域控制站的複寫設定|  
|Get-ADReplicationQueueOperation|傳回目前的複寫佇列待處理項目|  
|Get-ADReplicationSite|傳回站台資訊|  
|Get-ADReplicationSiteLink|傳回站台連結資訊|  
|Get-ADReplicationSiteLinkBridge|傳回站台連結橋接器資訊|  
|Get-ADReplicationSubnet|傳回 AD 子網路資訊|  
|Get-ADReplicationUpToDatenessVectorTable|傳回網域控制站的 UTD 向量|  
|Get-ADTrust|傳回網域間或樹系間信任的相關資訊|  
|New-ADReplicationSite|建立新的站台|  
|New-ADReplicationSiteLink|建立新的站台連結|  
|New-ADReplicationSiteLinkBridge|建立新的站台連結橋接器|  
|New-ADReplicationSubnet|建立新的 AD 子網路|  
|Remove-ADReplicationSite|刪除站台|  
|Remove-ADReplicationSiteLink|刪除站台連結|  
|Remove-ADReplicationSiteLinkBridge|刪除站台連結橋接器|  
|Remove-ADReplicationSubnet|刪除 AD 子網路|  
|Set-ADReplicationConnection|修改連線|  
|Set-ADReplicationSite|修改站台|  
|Set-ADReplicationSiteLink|修改站台連結|  
|Set-ADReplicationSiteLinkBridge|修改站台連結橋接器|  
|Set-ADReplicationSubnet|修改 AD 子網路|  
|Sync-ADObject|強制複寫單一物件|  
  
大部分這些 Cmdlet 在 Repadmin.exe 中都有自己的基礎。 其他 (未列出) 的 Cmdlet 則處理如「動態存取控制」與「群組受管理的服務帳戶」等功能。  
  
如需所有 Active Directory Windows PowerShell Cmdlet 的完整清單，請執行：  
  
```  
Get-command -module ActiveDirectory  
```  
  
如需所有 Active Directory Windows PowerShell Cmdlet 引數的完整清單，請參閱說明。 例如：  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
使用 `Update-Help` Cmdlet 來下載並安裝說明檔  
  
### <a name="BKMK_Repl"></a>複寫和中繼資料  
Repadmin.exe 會驗證 Active Directory 複寫的健康情況與一致性。 Repadmin.exe 提供簡單的資料管理選項 (例如某些引數支援 CSV 輸出)，但自動化通常需要透過文字檔案輸出剖析。 「適用於 Windows PowerShell 的 Active Directory 模組」是第一次嘗試提供可真正控制傳回資料的選項；在此之前，您必須建立指令碼或使用協力廠商工具。  
  
此外，下列 Cmdlet 實作新的參數集 **Target**、**Scope** 與 **EnumerationServer**：  
  
-   **ADReplicationFailure**  
  
-   **ADReplicationPartnerMetadata**  
  
-   **ADReplicationUpToDatenessVectorTable**  
  
**Target** 引數接受一個以逗號分隔的字串清單，識別由 **Scope** 引數所指定的目標伺服器、站台、網域或樹系。 也允許星號（\*），表示指定範圍內的所有伺服器。 如果未指定任何範圍，則表示目前使用者樹系中的所有伺服器。 **Scope** 引數指定搜尋的範圍。 可接受的值為 **Server**、**Site**、**Domain** 與 **Forest**。 **EnumerationServer** 指定的伺服器會列舉 **Target** 和 **Scope**中指定的網域控制站清單。 其運作方式與 **Server** 引數相同，而且要求指定的伺服器必須執行「Active Directory Web 服務」。  
  
為了介紹新的 Cmdlet，以下範例案例顯示 repadmin.exe 無法執行的功能；有了這些實例，就能明確顯示出系統管理的可能性。 如需特定的使用需求，請檢閱 Cmdlet 說明。  
  
### <a name="BKMK_ReplAttrMD"></a>ADReplicationAttributeMetadata  
此 Cmdlet 類似 **repadmin.exe /showobjmeta**。 它可讓您傳回復寫中繼資料，例如當屬性變更時，來源網域控制站、版本和 USN 資訊，以及屬性資料。 此 Cmdlet 可用來稽核變更的位置與時間。  
  
Windows PowerShell 與 Repadmin 不同的地方在於，可提供彈性的搜尋與輸出控制。 例如，您可以將 Domain Admins 物件的中繼資料輸出成排列過而方便讀取的清單：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
或者，您也可以將資料排列成表格，類似 repadmin：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
或者，您可以搭配篩選條件 (例如所有群組，然後與特定日期結合) 以管線方式輸出 **Get-Adobject** Cmdlet，以取得整個物件類別的中繼資料。 管線是用來在多個 Cmdlet 之間傳送資料的通道。 若要查看在 2012 年 1 月 13 日因為某些原因修改過的所有群組：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
如需更多 Windows PowerShell 作業搭配管線的詳細資訊，請參閱 [Windows PowerShell 中的管線處理與管線](https://technet.microsoft.com/library/ee176927.aspx)。  
  
或者，想要找出成員有 Tong Wang 的每個群組，以及上次修改群組的時間：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
或者，若要根據人工方式的高版本，找出網域中使用系統狀態備份進行系統授權還原的所有物件：  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
或者，將所有使用者中繼資料都傳送到 CSV 檔案，以供稍後在 Microsoft Excel 中檢查：  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>ADReplicationPartnerMetadata  
此 Cmdlet 會傳回網域控制站之複寫設定與狀態的相關資訊，讓您監視、清查或疑難排解。 不像 Repadmin.exe，使用 Windows PowerShell 表示您只會以您想要的格式，看到對您重要的資料。  
  
例如，單一網域控制站的可讀取複寫狀態：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
或者，以表格格式查看上一次網域控制站內送複寫及其複寫協力電腦的資料：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
或者，連絡樹系中的所有網域控制站並顯示最後一次嘗試複寫卻因任何原因而失敗的網域控制站：  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>ADReplicationFailure  
此 Cmdlet 可用來傳回複寫時發生最近錯誤的相關資訊。 它類似 **Repadmin.exe /showreplsum**，不過同樣地，因為使用 Windows PowerShell 而能採取更多控制方式。  
  
例如，您可以傳回網域控制站最近的失敗，以及其無法連絡的複寫協力電腦：  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
或者，傳回特定 AD 邏輯站台中所有伺服器的表格檢視，因為經過排序，所以更容易檢視，而且只包含最重要的資料：  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>ADReplicationQueueOperation 和 ADReplicationUpToDatenessVectorTable  
這兩個 Cmdlet 都會傳回網域控制站「最即時」的其他層面，包括暫止中的複寫和版本向量資訊。  
  
### <a name="BKMK_Sync"></a>同步處理-ADObject  
此 Cmdlet 類似執行 **Repadmin.exe /replsingleobject**。 當您進行需要頻外複寫的變更，尤其是修正問題時，它會非常有用。  
  
例如，如果某人刪除了總裁的使用者帳戶，並使用 Active Directory 資源回收筒將它還原，您可能想要將它立即複寫到所有網域控制站。 您可能也想要執行此動作而不強制複寫所有其他物件的變更；畢竟，這就是為什麼要有複寫排程 (因為可以避免 WAN 連結超過負荷)。  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>拓  
雖然 Repadmin.exe 擅長傳回如站台、站台連結、站台連結橋接器與連線等複寫拓撲的相關資訊，但是它並沒有一組完整的引數可進行變更。 事實上，也從來沒有任何專門設計可編寫指令碼、附隨的 Windows 公用程式，讓系統管理員建立及修改 AD DS 拓撲。 由於 Active Directory 在眾多客戶環境中已經非常成熟，因此大量修改 Active Directory 邏輯資訊的需求就變得很重要。  
  
例如，新的分公司迅速擴編加上與其他分公司整併，根據實體位置、網路變更和新的容量需求，您可能會有上百個站台變更需要處理。 比起使用 Dssites.msc 和 Adsiedit.msc 進行變更，您可以進行自動化。 當您使用網路和設備團隊提供的試算表資料時，這樣會特別方便。  
  
**Get-adreplication\\** * Cmdlet 會傳回複寫拓撲的相關資訊，並且適用于大量**get-adreplication\\** * Cmdlet。 **Get** Cmdlet 不會變更資料，而只會顯示資料，或建立可以管線處理**get-adreplication\\** * Cmdlet 的 Windows PowerShell 會話物件。 **New** 和 **Remove** Cmdlet 可用來建立或移除 Active Directory 拓撲物件。  
  
例如，您可以使用 CSV 檔案建立新的站台：  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
或者，在兩具有自訂複寫間隔和站台成本的個現有站台之間建立新的站台連結：  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
或者，尋找樹系中的每個站台並以旗標取代其 **Options** 屬性來啟用站台間的變更通知，以便使用最大的壓縮速度複寫：  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> 設定 **-bor 5** 可一併停用那些站台連結的壓縮。  
  
或者，尋找缺少子網路指派的所有站台，以便與那些位置的實際子網路調解清單：  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![使用 powershell 進行先進的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>請參閱  
[使用 Windows PowerShell &#40;層級100的 Active Directory 複寫和拓撲管理簡介&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  


---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: "進階複寫 Active Directory 和拓撲管理，使用 Windows PowerShell (層級 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>進階複寫 Active Directory 和拓撲管理，使用 Windows PowerShell (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題解釋新 AD DS 複寫及拓撲管理 cmdlet 在更多詳細資料，並提供額外的範例。 如簡介、 [Active Directory 複寫和拓撲管理使用 Windows PowerShell 和 #40; 簡介層級 100 和 #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [簡介](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [複製和中繼資料](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [取得-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [取得-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [取得-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [取得-ADReplicationQueueOperation 並取得-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [同步-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [拓撲](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>簡介  
Windows Server 2012 擴充 Windows PowerShell 模組 25 新 cmdlet 管理複寫和拓撲樹系的 Active Directory。 在過去，您被迫使用一般**\*-AdObject**名詞或通話.NET 函式。  
  
如同所有 Active Directory Windows PowerShell cmdlet，這個新的功能需要安裝[Active Directory 管理閘道服務](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852)上至少一個網域控制站 （最好所有網域控制站）。  
  
下表列出新複寫和拓撲 cmdlet 新增至 Active Directory Windows PowerShell 模組。  
  
|||  
|-|-|  
|Cmdlet|解釋|  
|取得-ADReplicationAttributeMetadata|退貨屬性物件複寫中繼資料|  
|取得-ADReplicationConnection|傳回網域控制站連接物件的詳細資料|  
|取得-ADReplicationFailure|退貨最多複寫最近失敗網域控制站|  
|取得-ADReplicationPartnerMetadata|退貨複寫設定的網域控制站|  
|取得-ADReplicationQueueOperation|退貨為目前複寫佇列待處理|  
|取得-ADReplicationSite|退貨網站資訊|  
|取得-ADReplicationSiteLink|退貨網站連結資訊|  
|取得-ADReplicationSiteLinkBridge|退貨網站連結橋接器資訊|  
|取得-ADReplicationSubnet|傳回 AD 子網路的資訊|  
|取得-ADReplicationUpToDatenessVectorTable|退貨網域控制站 UTD 向量|  
|取得-ADTrust|退貨間網域或跨樹系信任的相關資訊|  
|新 ADReplicationSite|建立新的網站|  
|新 ADReplicationSiteLink|建立新的網站連結|  
|新 ADReplicationSiteLinkBridge|建立新的網站連結橋接器|  
|新 ADReplicationSubnet|建立新的廣告子|  
|移除-ADReplicationSite|刪除網站|  
|移除-ADReplicationSiteLink|刪除網站連結|  
|移除-ADReplicationSiteLinkBridge|刪除網站連結橋接器|  
|移除-ADReplicationSubnet|刪除 AD 子網路|  
|設定-ADReplicationConnection|修改連接|  
|設定-ADReplicationSite|修改網站|  
|設定-ADReplicationSiteLink|修改網站連結|  
|設定-ADReplicationSiteLinkBridge|修改網站連結橋接器|  
|設定-ADReplicationSubnet|修改 AD 子網路|  
|同步-ADObject|力量︰ 複寫單一物件|  
  
大部分的下列 cmdlet 中 Repadmin.exe 有他們的方式。 其他 cmdlet （未列出） 處理動態存取控制與群組管理服務帳號等功能。  
  
適用於所有 Active Directory Windows PowerShell cmdlet 的完整清單，請執行：  
  
```  
Get-command -module ActiveDirectory  
```  
  
適用於所有 Active Directory Windows PowerShell cmdlet 引數的完整清單，參考協助。 例如：  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
使用`Update-Help`cmdlet 來下載並安裝協助檔案  
  
### <a name="BKMK_Repl"></a>複製和中繼資料  
Repadmin.exe 驗證複寫 Active Directory 的一致性與健康。 Repadmin.exe 提供簡單資料操作選項-某些引數支援 CSV 輸出，例如-但自動化通常需要剖析透過文字檔案輸出。 Windows PowerShell 模組 Active Directory 是第一次嘗試提供的選項，可讓真正控制傳回的資料;在過去，您必須建立指令碼，或使用第三方工具。  
  
此外，下列 cmdlet 實作參數全新的**目標**，**範圍**，並**EnumerationServer**:  
  
-   **取得-ADReplicationFailure**  
  
-   **取得-ADReplicationPartnerMetadata**  
  
-   **取得-ADReplicationUpToDatenessVectorTable**  
  
**目標**引數接受清單以逗號分隔的目標伺服器、 網站、 網域或指定的樹系找出字串**範圍**引數。 星號 (\ *) 也允許，表示指定的範圍中的所有伺服器。 如果未指定範圍，就表示目前使用者的森林中的所有伺服器。 **範圍**引數指定緯度的搜尋]。 可接受的值為**伺服器**，**網站**、**網域**，和**樹系**。 **EnumerationServer**指定列舉清單中所指定的網域控制站伺服器**目標**並**範圍**。 它的運作方式相同**伺服器**引數，並且需要執行 Active Directory Web 服務指定的伺服器。  
  
若要引入新 cmdlet，以下是一些樣本顯示功能無法 repadmin.exe;配備圖例，系統可能性明顯。 檢視需求特定使用 cmdlet 協助。  
  
### <a name="BKMK_ReplAttrMD"></a>取得-ADReplicationAttributeMetadata  
這個 cmdlet 是類似**repadmin.exe /showobjmeta**。 它可以讓您回到複寫中繼資料，例如屬性的變更時，原始的網域控制站、 版本及 USN 的詳細資訊，屬性資料。 這個 cmdlet 是適用於稽核位置，以及當發生變更。  
  
然而 Repadmin，Windows PowerShell 提供彈性搜尋和輸出控制。 例如，您可以輸出網域系統管理員物件，以讀取清單訂購中繼的資料：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
或者，您可以安排看起來像 repadmin、 表格中的資料：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
或者，您可以取得中繼資料的物件，整個種管線**取得-Adobject** cmdlet 篩選器，例如所有群組-然後結合的特定日期。 管線是之間傳送資料的多個 cmdlet 所使用的通道。 若要查看所有群組 2012 年 1 月 13 修改一些方式：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
如需有關更多的 Windows PowerShell 作業管線的詳細資訊，請查看[傳送及 Windows PowerShell 中的管線](https://technet.microsoft.com/library/ee176927.aspx)。  
  
或者，以了解每個群組，有 Tony Wang 成員，並上次修改群組：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
此外，系統授權尋找所有物件還原網域中，使用系統狀態備份依據他們手動高版本：  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
或者，將所有使用者中繼資料都傳送給 CSV 檔案以供日後檢查 Microsoft Excel 中：  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>取得-ADReplicationPartnerMetadata  
這個 cmdlet 傳回設定和的網域控制站，讓您監視、 清單，或疑難排解複寫狀態的相關資訊。 然而 Repadmin.exe，使用 Windows PowerShell 表示您會看到格式您想要對您而言重要的資料。  
  
例如，單一網域控制站的讀取複寫狀態：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
最後一次網域控制站複寫或者，輸入與表格中的，其合作夥伴格式化：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
或者，請連絡森林中的所有網域控制站，並顯示任何嘗試的最後一個複寫失敗任何原因：  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>取得-ADReplicationFailure  
這個 cmdlet 可用於退貨複寫最近錯誤的資訊。 它就像**Repadmin.exe /showreplsum**，再試一次，多與進一步控制感謝 Windows PowerShell，但。  
  
例如，您可以退還網域控制站的最新失敗和他失敗連絡合作夥伴：  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
或者，退款表格所有伺服器] 檢視中特定 AD 邏輯網站排列變得更容易檢視，並包含只最重要的資料：  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>取得-ADReplicationQueueOperation 並取得-ADReplicationUpToDatenessVectorTable  
這兩個下列 cmdlet 傳回進一步層面網域控制站 「 操之在 dateness 」、 暫停複寫和版本向量資訊包括。  
  
### <a name="BKMK_Sync"></a>同步-ADObject  
這個 cmdlet 是執行類似**Repadmin.exe /replsingleobject**。 當您需要退出 band 複寫，尤其是以修正問題的變更，它會很有幫助。  
  
例如，如果其他人刪除執行的帳號，然後將它還原的 Active Directory 資源回收筒您可能想所有網域控制站立即都複製。 您也可能會想要執行此動作不必複寫的所有其他物件所做的變更。畢竟，這是您已複寫排程-避免載 WAN 連結的原因。  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>拓撲  
Repadmin.exe 擅長退貨複寫拓撲等網站、 網站連結、 網站連結橋接器，並連接的相關資訊時，它不會有一組完整的變更引數。 事實上，不會有編寫指令碼，在方塊專為系統管理員來建立和修改 AD DS 拓撲設計的 Windows 公用程式。 在 Active Directory 中數百萬客戶環境成熟，需要大量修改 Active Directory，就能發揮邏輯資訊。  
  
例如新的分公司，加上的其他彙總快速擴充之後，您可能會所在位置、 網路的變更，以及新容量需求讓根據數百網站變更。 您可以使用 Dssites.msc 和 Adsiedit.msc 進行變更，除了自動化。 當您開始使用您的網路和功能的小組所提供之資料試算表，這是非常便利。  
  
**取得-Adreplication\ *** cmdlet 傳回複寫拓撲的相關資訊，適合用來插入管線**設定-Adreplication\ ***中大量 cmdlet。 **取得**cmdlet 不會變更資料，這些只顯示的資料或建立 Windows PowerShell 工作階段物件，可以趁著以**設定為 Adreplication\ *** cmdlet。 **新增]**和**移除**cmdlet 所建立，或移除 Active Directory 拓撲物件實用。  
  
例如，您可以建立使用 CSV 檔案的新網站：  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
或者，建立新的網站連結之間自訂複寫長的時間間隔與網站成本較兩個現有網站：  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
或者，尋找森林中的每個網站，並更換其**選項**以允許網站間的旗標屬性變更通知，以複寫壓縮使用最快的速度在：  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> 設定**-bor 5**若要停用壓縮這些網站連結。  
  
或者，尋找所有網站遺失子網路設定，以便協調實際這些位置的子網路清單：  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![透過 powershell 進階的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>也了  
[複寫 Active Directory 和拓撲管理，使用 Windows PowerShell 與 #40; 簡介層級 100 和 #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  


---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: "複寫 Active Directory 和拓撲管理，使用 Windows PowerShell (層級 100) 簡介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>複寫 Active Directory 和拓撲管理，使用 Windows PowerShell (層級 100) 簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows PowerShell 的 Active Directory 包含管理複寫、網站、網域及森林、網域控制站和磁碟分割的能力。 使用者的先前管理工具，例如的 Active Directory 網站和服務] 嵌入式管理單元，repadmin.exe 將會注意到類似的功能現在已可使用的 Active Directory 操作 Windows PowerShell 中的。 此外，cmdlet 的現有 Windows PowerShell 的 Active Directory cmdlet，因此建立精簡的體驗，並允許輕鬆建立自動化的指令碼針對相容。

> [!NOTE]
> Windows PowerShell cmdlet 拓撲複寫 Active Directory，可在下列環境中：
> 
> -    Windows Server 2012 網域控制站
> -    Windows Server 2012 的遠端伺服器管理工具 AD DS 和 AD LDS 安裝。
> -   Windows&reg;使用遠端伺服器管理工具 AD DS 和 AD LDS 安裝 8。

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>安裝 Windows PowerShell 模組 Active Directory
AD DS 伺服器角色執行 Windows Server 2012 的伺服器上安裝時，預設會安裝 Active Directory 模組適用於 Windows PowerShell。 任何額外的步驟不會需要以外新增伺服器角色。 您也可以安裝 Active Directory 模組伺服器執行，安裝遠端伺服器管理工具，Windows Server 2012 上，您可以安裝 Active Directory 模組執行來下載並安裝 Windows 8 電腦上[遠端伺服器管理工具 (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972)。 查看[的指示](https://www.microsoft.com/download/details.aspx?id=28972)如安裝步驟。

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>適用於 Windows PowerShell Active Directory 複寫和拓撲管理 cmdlet 測試案例
下列案例的設計系統管理員，管理新 cmdlet 熟悉：

-   取得所有網域控制站與他們對應的網站清單

-   管理複寫拓撲

-   檢視複寫狀態和資訊

## <a name="lab-requirements"></a>實驗室需求

-   有兩個 Windows Server 2012 網域控制站：**DC1**和**DC2**的一部分 contoso.com 網域和位於網域中的公司的網站。

## <a name="view-domain-controllers-and-their-sites"></a>檢視網域控制站與他們的網站
在此步驟，您將會使用 Active Directory 模組適用於 Windows PowerShell 檢視現有的網域控制站及複寫拓撲的網域。

若要完成下列程序中的步驟，您必須網域管理群組成員或有相當權限。

#### <a name="to-view-all-active-directory-sites"></a>若要檢視所有的 Active Directory 網站

1.  在**DC1**，按一下 [ **Windows PowerShell**在工作列上。

2.  輸入下列命令：

    `Get-ADReplicationSite -Filter *`

    這會每個網站的相關詳細的資訊。 `Filter`在 Active Directory PowerShell cmdlet 使用參數限制傳回物件的清單。 此時，請星號（*）會顯示所有網站的物件。

    > [!TIP]
    > Windows PowerShell 中，您可以使用 Tab 鍵在自動完成的命令。
    > 
    > 範例：輸入`Get-ADRep`和多次按 tab 鍵，直到符合命令略過`Get-ADReplicationSite`。 自動完成也適用於參數名稱為`Filter`。

    要格式化的輸出`Get-ADReplicationSite`命令為表格和限制顯示到特定的欄位，您可以管道輸出到`Format-Table`命令 (或」`ft`」的簡短):

    `Get-ADReplicationSite -Filter * | ft Name`

    這會傳回的網站清單，包括名稱欄位較短的版本。

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>製作所有網域控制站的表格

-   輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    此命令傳回網域控制站裝載的名稱，以及它們的網站的關聯。

## <a name="manage-replication-topology"></a>管理複寫拓撲
在上一個步驟中，執行命令之後，`Get-ADDomainController -Filter * | ft Hostname,Site`，**DC2**已列於**公司**的網站。 在下列程序，您將會建立新的分支 office 網站，**分支 1**建立新的網站連結、設定的網站連結成本和複寫頻率，然後將**DC2**以**分支 1**。

若要完成下列程序中的步驟，您必須網域管理群組成員或有相當權限。

#### <a name="to-create-a-new-site"></a>若要建立新的網站

-   輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `New-ADReplicationSite BRANCH1`

    這個命令建立的新分支」office 網站，分支 1。

#### <a name="to-create-a-new-site-link"></a>若要建立新的網站連結

-   輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    這個命令建立的網站連結**分支 1**並已變更通知程序。

    > [!TIP]
    > 使用索引標籤自動完成參數名稱，例如`-SitesIncluded`與`-OtherAttributes`除了手動輸入出。

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>若要設定的網站連結成本和複寫頻率

-   輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    設定的網站連結成本為**分支 1**，**100**，並將網站的頻率**15 分鐘**。

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>網域控制站移動到不同的網站

-   輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    這個命令移動網域控制站**DC2**以**分支 1**的網站。

### <a name="verification"></a>驗證

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>若要確認網站建立新的網站連結、成本和複寫的頻率

-   按一下**伺服器管理員**，按一下 [**工具**，然後按一下 [ **Active Directory 網站和服務**，並確認動作：

    確認**分支 1**網站包含所有的 Windows PowerShell 命令正確的值。

    確認**公司-分支 1**會建立網站的連結，並連接**分支 1**和**公司**網站。

    確認**DC2**現在已在**分支 1**的網站。 或者，您可以開放**Active Directory 模組適用於 Windows PowerShell**，輸入下列命令，以確認**DC2**現在已在 [**分支 1**網站：`Get-ADDomainController -Filter * | ft Hostname,Site`。

## <a name="view-replication-status-information"></a>檢視複寫狀態的資訊
下列程序，您將會使用 Windows PowerShell 中的其中一個複寫 Active Directory 和管理 cmdlet，`Get-ADReplicationUpToDatenessVectorTable DC1`，若要使用的最新向量表格維護的每個網域控制站的簡單複寫報告。 森林中的每個網域控制站從看到最高原始寫入 USN 保留的最新向量本表。

若要完成下列程序中的步驟，您必須網域管理群組成員或有相當權限。

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>若要檢視最新的單一網域控制站向量表格

1.  輸入下列命令，**Windows PowerShell 模組 Active Directory**命令提示字元中：

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    這會顯示一份最高的 Usn 所見**DC1**針對每個森林中的網域控制站。 **伺服器**值指伺服器維護表格，這種情形下**DC1**。 **合作夥伴**所進行的變更︰ 複寫合作夥伴（直接或間接）是指值。 UsnFilter 價值，是最高 USN 所見**DC1**的合作夥伴。 森林中新增了新的網域控制站，如果這不會出現在**DC1**直到的表格**DC1**收到來自新的網域的變更。

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>若要檢視所有網域控制站的最新向量表格網域中

1.  在 Active Directory 模組的 Windows PowerShell 命令提示字元中輸入下列命令：

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    這個命令取代**DC1**的`*`，因此收集的所有網域控制站的最新向量表格資料。 資料會依**合作夥伴**和**伺服器**，然後在表格中顯示。

    排序可讓您輕鬆地比較最後 USN 的每個網域控制站看見的特定的複寫合作夥伴。 這是快速查看該複寫在您的環境。 如果複寫正確運作，回報特定的複寫合作夥伴 UsnFilter 值應相似所有網域控制站。

## <a name="see-also"></a>也了
[進階複寫 Active Directory 和拓撲管理，使用 Windows PowerShell 與 #40;層級 200 和 #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)



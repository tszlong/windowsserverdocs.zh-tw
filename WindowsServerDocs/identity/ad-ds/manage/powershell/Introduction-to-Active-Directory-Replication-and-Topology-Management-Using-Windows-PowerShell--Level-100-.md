---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Introduction to Active Directory Replication and Topology Management Using Windows PowerShell (Level 100)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 62ba9e757195861989fcd6d9eca395a47262aa7e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967775"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introduction to Active Directory Replication and Topology Management Using Windows PowerShell (Level 100)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 的 Windows PowerShell 可讓您管理複寫、站台、網域與樹系、網域控制站以及分割。 使用舊版管理工具 (像是 [Active Directory 站台及服務] 嵌入式管理單元與 repadmin.exe) 的使用者將在 Active Directory 的 Windows PowerShell 環境下找到類似的功能。 此外，這些 Cmdlet 也和 Active Directory 適用的現有 Windows PowerShell Cmdlet 相容，因此可以簡化程序，讓客戶輕鬆地建立自動化指令碼。

> [!NOTE]
> 下列環境可使用 Active Directory 複寫與拓撲適用的 Windows PowerShell Cmdlet：
>
> -    Windows Server 2012 網域控制站
> -    Windows Server 2012，並已安裝適用于 AD DS 和 AD LDS 的遠端伺服器管理工具。
> -   Windows &reg; 8，並已安裝適用于 AD DS 和 AD LDS 的遠端伺服器管理工具。

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>安裝 Windows PowerShell 的 Active Directory 模組
當 AD DS 伺服器角色安裝在執行 Windows Server 2012 的伺服器上時，預設會安裝適用于 Windows PowerShell 的 Active Directory 模組。 除了新增伺服器角色，不需要執行任何額外的步驟。 您也可以藉由安裝遠端伺服器管理工具，在執行 Windows Server 2012 的伺服器上安裝 Active Directory 模組，而且您可以下載並安裝[遠端伺服器系統管理工具 (RSAT) ](https://www.microsoft.com/download/details.aspx?id=28972)，在執行 windows 8 的電腦上安裝 Active Directory 模組。 如需安裝步驟，請參閱[指示](https://www.microsoft.com/download/details.aspx?id=28972)。

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>測試 Active Directory 複寫與拓撲管理適用的 Windows PowerShell Cmdlet 的案例
下列案例是為了讓系統管理員熟悉新的管理 Cmdlet 而設計的：

-   取得所有網域控制站與其對應站台的清單

-   管理複寫拓撲

-   檢視複寫狀態與資訊

## <a name="lab-requirements"></a>實驗室需求

-   兩個 Windows Server 2012 網域控制站： **DC1**和**DC2**屬於 contoso.com 網域，並且位於該網域內的公司網站。

## <a name="view-domain-controllers-and-their-sites"></a>檢視網域控制站與它們的站台
在此步驟中，您將使用 Windows PowerShell 的 Active Directory 模組來檢視網域現有的網域控制站與複寫拓撲。

您必須是 Domain Admins 群組的成員或具備相等的權限，才能完成下列程序中的步驟。

#### <a name="to-view-all-active-directory-sites"></a>檢視所有 Active Directory 站台

1.  在 **DC1** 上，按一下工作列的 **Windows PowerShell**。

2.  輸入下列命令：

    `Get-ADReplicationSite -Filter *`

    這樣會傳回關於每個站台的詳細資訊。 所有 Active Directory PowerShell Cmdlet 都使用 `Filter` 參數來限制傳回的物件清單。 在這種情況下，星號 (*) 代表所有站台物件。

    > [!TIP]
    > 在 Windows PowerShell 中，您可以使用 Tab 鍵自動完成命令。
    >
    > 範例：輸入 `Get-ADRep` 並按 Tab 多次可以跳過相符的命令，直到 `Get-ADReplicationSite` 出現為止。 自動完成也可以用在像是 `Filter` 的參數名稱上。

    若要將命令的輸出格式化 `Get-ADReplicationSite` 為表格，並將顯示限制為特定欄位，您可以使用管線將輸出傳送至 `Format-Table` 命令 (或 " `ft` "，以進行短) ：

    `Get-ADReplicationSite -Filter * | ft Name`

    這樣會傳回只包含 [名稱] 欄位的較短站台清單。

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>產生包含所有網域控制站的表格

-   在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    這個命令會傳回網域控制站主機名稱與它們的站台關聯。

## <a name="manage-replication-topology"></a>管理複寫拓撲
在上一個步驟中，執行完 `Get-ADDomainController -Filter * | ft Hostname,Site` 命令以後，**DC2** 會列為 **CORPORATE** 站台的一部分。 在下列程序中，您將會建立一個新的分公司站台 **BRANCH1**、建立新的站台連結、設定站台連結成本與複寫頻率，然後將 **DC2** 移到 **BRANCH1**。

您必須是 Domain Admins 群組的成員或具備相等的權限，才能完成下列程序中的步驟。

#### <a name="to-create-a-new-site"></a>建立新站台

-   在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `New-ADReplicationSite BRANCH1`

    這個命令會建立新的分公司站台 branch1。

#### <a name="to-create-a-new-site-link"></a>建立新的站台連結

-   在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    這個命令建立了與 **BRANCH1** 的站台連結，並開啟變更通知處理程序。

    > [!TIP]
    > 使用 Tab 鍵自動完成參數名稱 (像是 `-SitesIncluded` 與 `-OtherAttributes`)，而不以手動方式輸入。

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>設定站台連結成本與複寫頻率

-   在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    這個命令會將 **BRANCH1** 的站台連結成本設為 **100**，並將站台複寫頻率設為 **15 分鐘**。

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>將網域控制站移到其他站台

-   在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    這個命令會將網域控制站 **DC2** 移到 **BRANCH1** 站台。

### <a name="verification"></a>驗證

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>驗證站台建立結果、新的站台連結以及成本與複寫頻率

-   按一下 [伺服器管理員]****，依序按一下 [工具]**** 和 [Active Directory 站台及服務]****，然後進行下列驗證：

    驗證 **BRANCH1** 站台包含 Windows PowerShell 命令中所有正確的值。

    驗證 **CORPORATE-BRANCH1** 站台連結已經建立，而且連接了 **BRANCH1** 與 **CORPORATE** 站台。

    驗證 **DC2** 現在位於 **BRANCH1** 站台。 或者，您可以開啟 [Windows PowerShell 的 Active Directory 模組]**** 並輸入下列命令以驗證 **DC2** 現在位於 **BRANCH1** 站台：`Get-ADDomainController -Filter * | ft Hostname,Site`。

## <a name="view-replication-status-information"></a>檢視複寫狀態資訊
在下列程序中，您將使用 Active Directory 複寫與管理適用的其中一個 Windows PowerShell Cmdlet (`Get-ADReplicationUpToDatenessVectorTable DC1`)，產生使用最新向量表格製成的簡單複寫報告 (由各個網域控制站維護)。 這個最新向量表格會記錄從樹系中的每個網域控制站所觀察到的最高來源寫入 USN。

您必須是 Domain Admins 群組的成員或具備相等的權限，才能完成下列程序中的步驟。

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>檢視單一網域控制站的最新向量表格

1.  在 [Windows PowerShell 的 Active Directory 模組]**** 命令提示字元中輸入下列命令：

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    這樣會顯示 **DC1** 在樹系的每個網域控制站觀察到的最高 USN 清單。 **Server** 值指的是維護表格的伺服器，在此案例中是 **DC1**。 **Partner** 值指的是發生變更的複寫協力電腦 (直接或間接)。 UsnFilter 值是 **DC1** 從 Partner 觀察到的最高 USN。 如果將新的網域控制站新增至樹系，在**dc1**接收到來自新網域的變更之前，它不會出現在**dc1**的表格中。

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>檢視網域中所有網域控制站的最新向量表格

1.  在 [Windows PowerShell 的 Active Directory 模組] 命令提示字元中輸入下列命令：

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    這個命令會將 **DC1** 取代為 `*`，因此會收集所有網域控制站的最新向量表格。 資料會依照 **Partner** 與 **Server** 排序，然後顯示在表格中。

    排序功能可以讓您輕鬆比較每個網域控制站針對指定的複寫協力電腦所觀察到的最後一個 USN。 這是確認您的環境正在進行複寫的一個快速方法。 如果複寫可以正常進行，則所有網域控制站針對指定的複寫協力電腦所回報的 UsnFilter 值應該相當類似。

## <a name="see-also"></a>另請參閱
[使用 Windows PowerShell 進行先進的 Active Directory 複寫和拓撲管理 &#40;層級 200&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)



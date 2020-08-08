---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: 虛擬網域控制站技術參考附錄
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 058dbc3082a5d35f29e049d2eb8661e4d2624961
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954404"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>虛擬網域控制站技術參考附錄

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋︰

-   [詞彙](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)

-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)

## <a name="terminology"></a><a name="BKMK_Terms"></a>術語

-   **快照**-虛擬機器在特定時間點的狀態。 它相依于先前拍攝的快照集、硬體上和虛擬化平臺上的鏈。

-   **複製**-完整且獨立的虛擬機器複本。 它相依于虛擬硬體 (系統管理) 。

-   **完整複製**-完整複製是一種獨立的虛擬機器複本，在複製操作之後，與父虛擬機器共用沒有資源。 完整複製的持續操作與父虛擬機器完全分開。

-   **差異磁片**-一種虛擬機器複本，它會以持續的方式與父系虛擬機器共用虛擬磁片。 這通常會節省磁碟空間，並允許多部虛擬機器使用相同的軟體安裝。

-   **VM 複製**-虛擬機器所有相關檔案和資料夾的檔案系統複本。

-   **Vhd 檔案複製**-虛擬機器的 vhd 複本

-   **VM 世代識別碼**-由管理者提供給虛擬機器的128位整數。 此識別碼會儲存在記憶體中，並在每次套用快照集時重設。 此設計會使用不受虛擬程式管理的機制，在虛擬機器中呈現 VM 世代識別碼。 Hyper-v 執行會公開虛擬機器的 ACPI 資料表中的識別碼。

-   匯**入/匯出**-hyper-v 功能，可讓使用者將整個虛擬機器儲存 (VM 檔案、VHD 和電腦設定) 。 接著，它可讓使用者使用該檔案集，將機器恢復與相同的 VM 相同的電腦上 (還原) 、在不同的電腦上作為相同的 VM (移動) 或新的 VM (複製) 

## <a name="fixvdcpermissionsps1"></a><a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1

```
# Unsigned script, requires use of set-executionpolicy remotesigned -force
# You must run the Windows PowerShell console as an elevated administrator

# Load Active Directory Windows PowerShell Module and switch to AD DS drive
import-module activedirectory
cd ad:

## Get Domain NC
$domainNC = get-addomain

## Get groups and obtain their SIDs
$dcgroup = get-adgroup "Cloneable Domain Controllers"

$sid1 = (get-adgroup $dcgroup).sid

## Get the DACL of the domain
$acl = get-acl $domainNC

## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"

$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid

## Add the ACE in the ACL and set the ACL on the object

$acl.AddAccessRule($ace1)
set-acl -aclobject $acl $domainNC
write-host "Done writing new VDC permissions."
cd c:
```




---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: 虛擬網域控制站技術參考附錄
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832229"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>虛擬網域控制站技術參考附錄

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題涵蓋：  
  
-   [術語](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>術語  
  
-   **快照集**-某個特定時間點上的虛擬機器的狀態。 它是相依鏈結的上一個快照，在硬體上，以及虛擬化平台。  
  
-   **複製**-完成和不同的虛擬機器的複本。 它會相依於虛擬硬體 (hypervisor)。  
  
-   **完整複製**-完整複製是不共用與父代的虛擬機器的任何資源，複製作業之後的虛擬機器的獨立複本。 進行中作業的完整複本是完全不同，從父代的虛擬機器。  
  
-   **差異磁碟**-進行中的方式共用與父代的虛擬機器的虛擬磁碟的虛擬機器的複本。 這通常可以節省磁碟空間，並可讓多部虛擬機器使用相同的軟體安裝。  
  
-   **VM 複製**-檔案系統複製的所有相關檔案和資料夾的虛擬機器。  
  
-   **VHD 檔案複製**-虛擬機器之 VHD 的複本  
  
-   **VM 世代識別碼**-由 hypervisor 所提供給虛擬機器的 128 位元整數。 此 ID 是儲存在記憶體中，並每次套用快照集時，重設。 設計使用 hypervisor 無從驗證機制，呈現在虛擬機器中的 VM 世代識別碼。 HYPER-V 實作公開 ACPI 表格的虛擬機器中的識別碼。  
  
-   **匯入/匯出**-HYPER-V 功能，可讓使用者儲存整部虛擬機器 （VM 檔案，VHD 和機器組態）。 它接著可讓使用者重新開啟相同的電腦相同的 VM （還原），使機器使用的一組檔案為相同的 VM （移動） 或 （複製） 的新 VM 的不同電腦上  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
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
  



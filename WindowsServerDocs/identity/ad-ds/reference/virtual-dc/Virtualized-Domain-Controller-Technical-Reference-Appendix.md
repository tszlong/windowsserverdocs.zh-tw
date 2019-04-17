---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: "模擬的網域控制站技術參考附錄"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>模擬的網域控制站技術參考附錄

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋：  
  
-   [詞彙](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>詞彙  
  
-   **快照**-一樣，在特定時間點的狀態。 它是和模擬平台上鏈結的上一個快照、 上的硬體相關。  
  
-   **複製**-完成，不同的一樣。 它是 virtual 硬體 (hypervisor) 而定。  
  
-   **完整複製**-完整複製是獨立複製操作之後會與家長一樣共用不資源一樣複本。 完整的複製執行的作業會完全分開家長一樣。  
  
-   **差異磁碟**-一份執行的方式會與家長一樣共用 virtual 磁碟一樣。 這通常是節省磁碟空間，可使用相同的軟體安裝多個虛擬電腦。  
  
-   **VM 複製**-檔案系統複本相關的所有檔案和資料夾的一樣。  
  
-   **VHD 複製檔案**-一樣 VHD 複本  
  
-   **VM 代 ID** -128 元整數 hypervisor 所提供給一樣。 這個 ID 是儲存在記憶體中，每次快照套用重設。 設計使用 hypervisor 無關機制則新一代 VM 中的 ID 一樣。 HYPER-V 實作公開中 ACPI 一樣的來電顯示。  
  
-   **匯入日匯出**-A HYPER-V 功能，可讓使用者儲存整個一樣 （VM 的檔案、 VHD 和電腦組態）。 然後可讓使用者將電腦相同 VM （還原），以相同的電腦上使用該檔案的設定為相同的 VM （移） 或新增 VM （複製） 是不同的電腦上  
  
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
  



---
title: 移轉角色和功能
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 486c11ebd46c6fd23b3bd16cd90463f8d607287e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443541"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>在 Windows Server 中移轉角色和功能

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

此頁面包含工具和資訊的連結，可協助指引您完成將角色和功能移轉至 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的程序。 許多角色和功能都可以使用 Windows Server 移轉工具進行移轉，這一組工具共有五個在 Windows Server 2008 R2 中導入的 Windows PowerShell Cmdlet，可用來輕鬆移轉角色及功能項目和資料。

移轉指南支援將指定的角色和功能從一個伺服器移轉至另一個 (而不是就地升級)。 除非指引中另有說明，支援的移轉指的都是在實體與虛擬電腦之間，以及在 Windows Server 的完整安裝選項與執行 Server Core 安裝選項的伺服器之間進行的移轉。  

## <a name="before-you-begin"></a>開始之前

在您開始移轉角色和功能之前，請確認來源和目的地伺服器是否都在執行適用於其作業系統的最新 Service Pack。
目前已有 Windows Server 2012 R2 與 Windows Server 2012 移轉指南電子書可供閱讀。 如需詳細資訊並下載電子書，請參閱 [Microsoft 技術電子書庫](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles)。 

>[!NOTE]
>每當您移轉或升級到任何 Windows Server 版本時，都應該檢閱和了解該版本的[支援週期原則](https://support.microsoft.com/lifecycle)與時間範圍，並進行適當規劃。 您可以針對感興趣的特定 Windows Server 版本[搜尋週期資訊](https://support.microsoft.com/lifecycle)。
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>移轉指南
Windows Server 2016 的更新版移轉指南仍在開發製作中。 當這些指南推出時，請回到這個位置來查看是否有更新。 有很多時候，Windows Server 2012 R2 移轉指南中的步驟仍然適用於 Windows Server 2016。

- [遠端桌面服務](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Web Server (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint 服務](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>移轉指南
依照下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 或 Windows Server 2012 R2 的伺服器移轉到 Windows Server 2012 R2。 Windows Server 2012 R2 的 Windows Server 移轉工具支援跨子網路移轉。

- [安裝、 使用以及移除 Windows Server 移轉工具](https://technet.microsoft.com/library/jj134202.aspx)
- [Windows Server 2012 R2 的 active Directory 憑證服務移轉指南](https://technet.microsoft.com/library/dn486797.aspx)
- [Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Active Directory Rights Management Services 移轉和升級指南](https://technet.microsoft.com/library/cc754277.aspx)
- [將檔案和存放服務移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [將 Hyper-V 從 Windows Server 2012 移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn486799.aspx)
- [將網路原則伺服器移轉到 Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [將遠端桌面服務移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [將 Windows Server Update Services 移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [將叢集角色移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [將 DHCP 伺服器移轉到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>移轉指南
依照下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 的伺服器移轉到 Windows Server 2012。 Windows Server 2012 的 Windows Server 移轉工具支援跨子網路移轉。

- [安裝、 使用以及移除 Windows Server 移轉工具](https://technet.microsoft.com/library/jj134202)
- [將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [將健康情況登錄授權單位移轉到 Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [將 HYPER-V 移轉到 Windows Server 2012 中，從 Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [將 IP 設定移轉到 Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [將網路原則伺服器移轉到 Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [移轉列印和文件至 Windows Server 2012 的服務](https://technet.microsoft.com/library/jj134150)
- [將遠端存取移轉到 Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [將 Windows Server Update Services 移轉到 Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Active Directory 網域控制站升級到 Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [移轉叢集的服務和應用程式到 Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

如需其他移轉資源，請瀏覽[將角色和功能移轉到 Windows Server 2012](https://technet.microsoft.com/library/jj134039)。

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>移轉指南
依照下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的伺服器移轉到 Windows Server 2008 R2。 Windows Server 2008 R2 的 Windows Server 移轉工具不支援跨子網路移轉。

- [Windows Server 移轉工具安裝、 存取和移除](https://technet.microsoft.com/library/dd379545)
- [Active Directory 憑證服務移轉指南](https://technet.microsoft.com/library/ee126170)
- [Active Directory 網域服務及網域名稱系統 (DNS) 伺服器移轉指南](https://technet.microsoft.com/library/dd379558)
- [BranchCache 移轉指南](https://technet.microsoft.com/library/dd548365)
- [動態主機設定通訊協定 (DHCP) 伺服器移轉指南](https://technet.microsoft.com/library/dd379535)
- [檔案服務移轉指南](https://technet.microsoft.com/library/dd379487)
- [HRA 移轉指南](https://technet.microsoft.com/library/ee791829)
- [HYPER-V 移轉指南](https://technet.microsoft.com/library/ee849855)
- [IP 設定移轉指南](https://technet.microsoft.com/library/dd379537)
- [本機使用者和群組移轉指南](https://technet.microsoft.com/library/dd379531)
- [NPS 移轉指南](https://technet.microsoft.com/library/ee791849)
- [列印服務移轉指南](https://technet.microsoft.com/library/dd379488)
- [遠端桌面服務移轉指南](https://technet.microsoft.com/library/ff849223)
- [RRAS 移轉指南](https://technet.microsoft.com/library/ee822825)
- [Windows Server 移轉常見工具和資訊](https://technet.microsoft.com/library/ff400258)
- [Windows Server Update Services 3.0 SP2 移轉指南](https://technet.microsoft.com/library/ee822826)
 
如需其他移轉資源，請瀏覽[將角色和功能移轉到 Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353)。

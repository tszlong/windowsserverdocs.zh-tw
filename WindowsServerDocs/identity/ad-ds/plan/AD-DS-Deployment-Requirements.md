---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: "AD DS 部署需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>AD DS 部署需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您現有的環境的結構判斷策略部署 Windows Server 2008 Active Directory Domain Services (AD DS)。 如果您要建立 AD DS 環境，您不需要現有的網域結構完成 AD DS 設計，才能開始建立您的環境 AD DS。 您可以部署新的樹系根網域然後部署中的其餘部分網域結構依據您的設計。  
  
此外，部分 AD DS 部署，您可以選擇升級並重建您的環境。 例如如果您的組織已經現有的 Windows 2000 網域結構，您可能會執行某些網域的就地升級並重建其他人。 此外，您可以選擇由重建樹系之間網域或重建森林中的網域部署 AD DS 之後減少複雜的環境。  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 森林根網域  
森林根網域 AD DS 森林基礎結構提供基本知識。 若要部署 AD DS，您必須先部署森林根網域。 若要這樣做，您必須檢視 AD DS 設計。設定樹系根網域; DNS 的服務建立部署森林根網域控制站、設定網站拓撲森林根網域，以及設定（也稱為彈性的單一主機操作或 FSMO）; 操作主機角色所組成森林根網域並引發的樹系和網域正常運作的層級。 下圖顯示部署森林根網域整體程的序。  
  
![AD DS 需求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
如需詳細資訊，請查看[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 區域的網域  
森林根網域部署完成之後，您就可以將任何新的 Windows Server 2008 區域網域所指定的設計。 若要這樣做，您必須部署網域控制站的每個地區的網域。 下圖顯示部署區域網域程的序。  
  
![AD DS 需求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
如需詳細資訊，請查看[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>升級到 Windows Server 2008 的 Active Directory 網域  
您的 Windows 2000 或 Windows Server 2003 網域升級到 Windows Server 2008 網域是要利用其他 Windows Server 2008 功能有效、簡單的方式。 您可以升級以維護您目前的網路及網域設定時改善安全性、延展性及管理您的網路基礎結構的網域。 從 Windows 2000 或 Windows Server 2003 升級到 Windows Server 2008 需要最小的網路設定。 升級使用者作業上也有太大的影響。 如需詳細資訊，請查看[Windows Server 2008 和 Windows Server 2008 R2 AD DS 網域升級 Active Directory 網域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
## <a name="restructuring-ad-ds-domains"></a>重新建構 AD DS 網域  
當您重建網域之間 Windows Server 2008 森林（樹系間重建）時，您可以在您的環境中減少的網域並減少管理複雜和負擔。 當您移轉重組程序的一部分樹系之間物件時，這兩個來源網域與目標網域環境同時存在。 這可讓您可以回復到原始檔環境移轉必要。  
  
當您重建 Windows Server 2008 的網域中的 Windows Server 2008 森林（樹系內重建）時，您可以整合網域結構，減少管理複雜和管理成本。 您重建森林中的網域，當移轉的帳號不會存在來源網域中。  
  
如需有關如何使用 Active Directory 移轉工具 (ADMT) 3.1 (ADMT v3.1) 版重建網域的詳細資訊，請查看[ADMT v3.1 移轉指南](https://go.microsoft.com/fwlink/?LinkId=93678)。  
  



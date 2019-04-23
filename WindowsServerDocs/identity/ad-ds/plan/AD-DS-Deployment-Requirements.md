---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: AD DS 部署需求
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d21491f5ce9c15ecc514e4be24a91de28b0fd0ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890969"
---
# <a name="ad-ds-deployment-requirements"></a>AD DS 部署需求

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

現有環境的結構會判斷您的策略來部署 Windows Server 2008 Active Directory 網域服務 (AD DS)。 如果您要建立 AD DS 環境，而且您沒有現有的網域結構，完成您的 AD DS 設計，才能開始建立您的 AD DS 環境。 然後，您可以部署新樹系根網域，並部署您的網域結構的其餘部分，根據您的設計。  
  
此外，在 AD DS 部署，您可能會決定要升級並重新建構您的環境。 比方說，如果您的組織有現有的 Windows 2000 網域結構，您可以執行某些網域的就地升級，並重新建構其他人。 此外，您可能會決定重新建構樹系之間的網域，或重新建構樹系中的網域，在您部署 AD DS 之後藉此降低環境的複雜度。  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 樹系根網域  
樹系根網域提供您的 AD DS 樹系基礎結構的基礎。 若要部署 AD DS，您必須先部署樹系根網域。 若要這樣做，您必須檢閱您的 AD DS 設計;設定樹系根網域的 DNS 服務建立樹系根網域，其中包含部署樹系根網域控制站、 設定樹系根網域的站台拓撲和設定操作主機角色 （也稱為彈性單一主機操作或 FSMO）;和提高樹系和網域功能等級。 下圖顯示部署樹系根網域的整體程序。  
  
![AD DS 需求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
如需詳細資訊，請參閱 <<c0> [ 部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 地區網域  
在完成樹系根網域部署後，您準備好要部署任何新的 Windows Server 2008 地區網域設計所指定。 若要這樣做，您必須部署每個地區網域的網域控制站。 下圖顯示部署地區網域的程序。  
  
![AD DS 需求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
如需詳細資訊，請參閱 <<c0> [ 部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>升級到 Windows Server 2008 的 Active Directory 網域  
升級至 Windows Server 2008 網域的 Windows 2000 或 Windows Server 2003 的網域是利用額外的 Windows Server 2008 功能的有效率且直接的方式。 您可以升級網域，以維護您目前的網路與網域組態時改善安全性、 延展性及網路基礎結構的管理能力。 從 Windows 2000 或 Windows Server 2003 升級至 Windows Server 2008 需要基本網路設定。 升級對使用者作業也沒有太大的影響。 如需詳細資訊，請參閱 < [Windows Server 2008 和 Windows Server 2008 R2 AD DS 網域升級 Active Directory 網域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
## <a name="restructuring-ad-ds-domains"></a>重新建構 AD DS 網域  
當您重建 Windows Server 2008 樹系 （樹系間進行重建） 之間的網域時，您可以減少您的環境中的網域數目，並因此減少系統管理的複雜性和額外負荷。 當您移轉的這一部分的樹系之間的物件結構的程序時，同時存在兩個來源網域和目標網域環境。 這項功能可讓您回復至來源環境在移轉期間，如有必要。  
  
當您重建 Windows Server 2008 中的網域的 Windows Server 2008 樹系 （樹系內進行重建） 時，您可以合併您的網域結構，並因此減少系統管理的複雜性和額外負荷。 當您重新建構樹系中的網域時，已移轉的帳戶將不再存在於來源網域。  
  
如需如何使用 Active Directory 遷移工具 (ADMT) 版本 3.1 (ADMT v3.1) 重建網域的詳細資訊，請參閱[ADMT v3.1 移轉指南](https://go.microsoft.com/fwlink/?LinkId=93678)。  
  



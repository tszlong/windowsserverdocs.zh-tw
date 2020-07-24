---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: AD DS 部署需求
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 228d4d1644c3bae60dcf293540ad764fb511922a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962500"
---
# <a name="ad-ds-deployment-requirements"></a>AD DS 部署需求

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

現有環境的結構決定了部署 Windows Server 2008 Active Directory Domain Services （AD DS）的策略。 如果您要建立 AD DS 環境，而且沒有現有的網域結構，請在開始建立 AD DS 環境之前，先完成您的 AD DS 設計。 然後，您可以部署新的樹系根域，並根據您的設計部署網域結構的其餘部分。

此外，做為 AD DS 部署的一部分，您可能會決定升級並重新組織您的環境。 例如，如果您的組織具有現有的 Windows 2000 網域結構，您可能會執行某些網域的就地升級，並重新組織其他網域。 此外，您可能會決定在部署 AD DS 之後，在樹系之間重建網域，或重建樹系中的網域，以降低環境的複雜度。

## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>部署 Windows Server 2008 樹系根域
樹系根網域是 AD DS 樹系基礎結構的基石。 若要部署 AD DS，您必須先部署樹系根域。 若要這麼做，您必須檢查 AD DS 設計;設定樹系根域的 DNS 服務;建立樹系根域，其中包含部署樹系根網域控制站、設定樹系根域的網站拓朴，以及設定操作主機角色（也稱為彈性單一主機操作或 FSMO）;並提高樹系和網域功能等級。 下圖顯示部署樹系根域的整體程式。

![AD DS 需求](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)

如需詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="deploying-windows-server-2008-regional-domains"></a>部署 Windows Server 2008 地區網域
完成樹系根域的部署之後，您就可以開始部署您的設計所指定的任何新的 Windows Server 2008 地區網域。 若要這樣做，您必須為每個地區網域部署網域控制站。 下圖顯示部署地區網域的程式。

![AD DS 需求](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)

如需詳細資訊，請參閱[部署 Windows Server 2008 地區網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755118(v=ws.10))。

## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>將 Active Directory 網域升級至 Windows Server 2008
將您的 Windows 2000 或 Windows Server 2003 網域升級至 Windows Server 2008 網域，是利用額外的 Windows Server 2008 特性和功能的有效率且簡單的方式。 您可以升級網域以維護目前的網路和網域設定，同時改善網路基礎結構的安全性、擴充性和管理性。 從 Windows 2000 或 Windows Server 2003 升級至 Windows Server 2008 需要最少的網路設定。 升級也對使用者作業沒有太大的影響。 如需詳細資訊，請參閱[將 Active Directory 網域升級至 Windows server 2008 和 Windows server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10))。

## <a name="restructuring-ad-ds-domains"></a>重建 AD DS 網域
當您在 Windows Server 2008 樹系（樹系重建）之間重建網域時，您可以減少環境中的網域數目，進而降低系統管理的複雜性和額外負荷。 當您在此重組程式中遷移樹系之間的物件時，來源網域和目標網域環境同時存在。 如有需要，您可以在遷移期間回復至來源環境。

當您在 Windows Server 2008 樹系（林內重建）中重建 Windows Server 2008 網域時，可以合併您的網域結構，因而降低系統管理的複雜性和額外負荷。 當您在樹系中重建網域時，已遷移的帳戶已不存在於來源網域中。

如需如何使用 Active Directory 遷移工具（ADMT）3.1 版（ADMT 3.1）來重建網域的詳細資訊，請參閱[ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10))。

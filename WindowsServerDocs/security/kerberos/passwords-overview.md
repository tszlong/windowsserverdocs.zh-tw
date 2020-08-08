---
title: 密碼概觀
description: Windows Server 安全性
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4f38ee2062b2c154cc99a22a398e02e823a47ca2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996076"
---
# <a name="passwords-overview"></a>密碼概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明 Windows 作業系統中使用的密碼，以及在認證管理原則中使用密碼的相關檔和討論的連結。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
現今的作業系統和應用程式都是以密碼為架構，即使您使用智慧卡或生物特徵辨識系統，所有帳戶仍有密碼，而且在某些情況下仍可使用。 有些帳戶，特別是用來執行服務的帳戶，甚至無法使用智慧卡和生物識別權杖，因此必須使用密碼來進行驗證。 Windows 會使用密碼編譯雜湊來保護密碼。

如需 Windows 密碼的詳細資訊，請參閱[密碼技術總覽](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994558(v=ws.10))。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
在 Windows 和許多其他作業系統中，驗證使用者身分識別最常見的方法是使用密碼複雜密碼或密碼。 保護您的網路環境需要所有使用者都必須使用強式密碼。 這有助於避免惡意使用者猜測弱式密碼的威脅，不論是透過手動方法或使用工具，取得遭到盜用的使用者帳戶的認證。 尤其是系統管理帳戶的情況。 當您定期變更複雜密碼時，它會降低密碼攻擊危害該帳戶的可能性。

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>新功能和變更的功能
在 Windows Server 2012 和 Windows 8 中，圖片密碼是新的。 圖片密碼是使用者選取影像與一系列手勢結合的組合。 已加入網域之電腦上的圖片密碼功能已停用 \- 。 如需圖片密碼的詳細資訊連結，[請參閱](#BKMK_LINKS)下文。

Windows Server 2012 和 Windows 8 中的密碼功能沒有任何變更。 尚未新增任何新的群組原則設定。 不過，認證和密碼管理方面的改進和增強功能 \( \) ，例如使用圖片密碼、認證保險箱，以及使用 Microsoft 帳戶（先前稱為 WINDOWS Live ID）登入 Windows 8。

## <a name="deprecated-functionality"></a><a name="BKMK_DEP"></a>已被取代的功能
Windows Server 2012 和 Windows 8 中沒有任何密碼功能已被取代。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
在企業環境中，通常會使用 Active Directory Domain Services 來管理密碼。 您也可以使用 [本機安全性設定]、[帳戶原則]、[密碼原則] 中的設定，在本機電腦上管理密碼。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>另請參閱
下表列出密碼功能、技術和認證管理的其他資源。

|內容類型|參考|
|--------|-------|
|**案例文件**|[保護您的數位識別身分](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**作業**|[Active Directory 使用者及電腦](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754217(v=ws.11))|
|**疑難排解**|[瞭解您的密碼何時過期 \- Active Directory PowerShell Blog](https://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全性**| Windows Server 2008 R2 和 Windows 7 的[威脅與對策指南：帳戶原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125920(v=ws.10))<p>[變更和建立強式密碼的](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)指引|
|**工具及設定**|[Microsoft 下載中心上的 Windows 和 Windows Server 群組原則設定參考](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社群資源**|[保護您的數位識別身分](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<p>[使用 Windows Live ID 登入 Windows 8](https://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<p>[使用圖片密碼登入](/archive/blogs/b8/signing-in-with-a-picture-password)<p>[優化圖片密碼安全性](/archive/blogs/b8/optimizing-picture-password-security)|
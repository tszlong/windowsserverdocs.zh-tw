---
title: "密碼概觀"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="passwords-overview"></a>密碼概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

適用於 IT 專業人員本主題描述在所使用的 Windows 作業系統和文件和認證管理策略中的密碼使用討論連結中的密碼。

## <a name="BKMK_OVER"></a>描述的功能
作業系統和應用程式目前的密碼周圍架構，而且即使您使用智慧卡或系統生物特徵辨識，所有帳號仍都有密碼，並仍然可以使用有時中。 某些帳號，您可以注意到帳號用來執行的服務，甚至不能使用智慧卡和生物特徵辨識權杖，因此必須使用密碼來驗證。 Windows 可以使用密碼編譯 hashes 密碼。

如需 Windows 密碼，請查看[密碼技術概觀](https://technet.microsoft.com/library/hh994558(WS.10).aspx)。

## <a name="BKMK_APP"></a>實用的應用程式
在 Windows 和許多其他的作業系統，適用於驗證身分使用者的最常見的方法是使用複雜密碼秘密或密碼。 保護您的環境網路需要密碼穩固供所有使用者。 這有助於避免猜測低密碼，無論是透過手動方法，或使用工具，以取得受損的使用者的認證惡意使用者的威脅。 這非常適用於系統帳號。 當您定期變更複雜密碼時，它可以減少密碼攻擊危害該 account 的可能性。

## <a name="BKMK_NEW"></a>新功能和變更功能
在 Windows Server 2012 和 Windows 8，這會新圖片密碼。 搭配手勢的一系列的使用者選取映像的組合會圖片密碼。 停用圖片密碼功能 domain\ 加入電腦上。 圖片密碼的相關詳細資訊連結會列在[也看到](#BKMK_LINKS)下方。

已有密碼功能在 Windows Server 2012 和 Windows 8 中不會變更。 已新增新的群組原則設定。 不過，改良功能和改進已在認證 \(and password\) 管理，例如使用圖片密碼、Credential 購物服務區及使用 Microsoft account 登入 Windows 8，之前稱為「Windows Live ID

## <a name="BKMK_DEP"></a>過時的功能
無密碼的功能已在 Windows Server 2012 和 Windows 8 中已取代。

## <a name="BKMK_SOFT"></a>軟體需求
在企業環境中，通常是使用 Active Directory Domain Services 管理密碼。 密碼也可以使用當地的安全性設定，Account 原則，密碼原則中的設定會在本機電腦上進行管理。

## <a name="BKMK_LINKS"></a>也了
下表中列出的密碼的功能，其他資源技術和認證管理。

|內容類型|資訊尋找參考資料|
|--------|-------|
|**案例的文件**|[保護您的數位身分](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**作業**|[Active Directory 使用者與電腦](https://technet.microsoft.com/library/cc754217.aspx)|
|**疑難排解**|[了解當您的密碼後 \-Active Directory PowerShell 部落格](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全性**| Windows Server 2008 R2 和 Windows 7[威脅和措施快速入門：帳號原則](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />指引，以[變更，並建立穩固密碼](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**工具和設定**|[適用於 Windows 和 Windows Server Microsoft 下載中心的群組原則設定參考資料](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社群資源**|[保護您的數位身分](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Windows 8 的 Windows Live ID 登入](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[使用圖片密碼登入](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[將圖片密碼安全性最佳化](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|



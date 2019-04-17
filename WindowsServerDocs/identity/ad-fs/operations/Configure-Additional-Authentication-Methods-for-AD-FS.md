---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: "AD fs 中設定額外的驗證方法"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>AD fs 中設定額外的驗證方法

>適用於：Windows Server 2016、Windows Server 2012 R2

為了讓多因素驗證 (MFA)，您必須選取至少一個額外的驗證方法。 根據預設，在 Windows Server 2012 R2 的 Active Directory 同盟 Services (AD FS) 在您可以選取憑證驗證（亦即，智慧卡架構的驗證）做為額外的驗證方法。

> [!NOTE]
> 如果您選擇憑證驗證，確保的智慧卡憑證已確實提供，而且有釘選的需求。

您知道，Microsoft Azure 會提供類似在雲端中的功能嗎？ 深入了解[Microsoft Azure 的身分方案](http://aka.ms/m2w274)。<br /><br />Microsoft Azure 建立混合式身分方案：<br /> - [深入了解 Azure 多因素驗證。](http://aka.ms/ey6o9r)<br /> - [管理適用於單一樹系混合式環境使用雲端驗證身分。](http://aka.ms/g1jat8)<br /> - [[管理其他多因素驗證敏感的應用程式的風險。](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Microsoft 和第三方額外的驗證方法
您也可以設定，並讓 Microsoft 和第三方驗證方法 AD FS 在 Windows Server 2012 R2。 安裝和使用廣告 FS 註冊之後, 您可以執行 MFA 全球或每個--信賴驗證原則的一部分。

以下是 Microsoft 和第三方提供者 MFA 方案的字母清單目前可在 Windows Server 2012 R2 AD fs。

||||
|-|-|-|
|**提供者**|**提供**|**連結，以了解更多**|
|Gemalto|Gemalto 身分和安全性服務|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo 技術|inWebo 驗證企業服務|[inWebo 企業驗證](http://www.inwebo.com)|
|登入連絡人|登入人 MFA API 連接器 ad FS 2012 R2（公用搶鮮版）|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[逐步解說指南：管理敏感的應用程式與其他多因素驗證風險](https://technet.microsoft.com/library/dn280946.aspx)（看到執行「步驟 3）|
|RSA，EMC 的安全性區域|Microsoft Active Directory 同盟服務的 RSA SecurID 驗證代理程式|[Microsoft Active Directory 同盟服務的 RSA SecurID 驗證代理程式](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet，Inc.|AD fs SafeNet 驗證服務 (SAS) 代理程式|[SafeNet 驗證服務：AD FS 代理設定指南](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|行動裝置版 ID 驗證服務和特徵碼服務|[行動裝置版 ID 驗證服務](http://swisscom.ch/mid)|
|Symantec|Symantec 驗證和 ID Protection Service (VIP)|[Symantec 驗證和 ID Protection Service (VIP)](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 AD FS 自訂的驗證方法
現在，我們會提供 AD FS 在 Windows Server 2012 R2 建置您自己的自訂的驗證方法的指示操作。 如需詳細資訊，請查看[適用於在 Windows Server 2012 R2 AD FS 建置自訂驗證方法](https://go.microsoft.com/fwlink/?LinkID=511980)。

## <a name="see-also"></a>也了
[管理其他多因素驗證敏感的應用程式的風險](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)



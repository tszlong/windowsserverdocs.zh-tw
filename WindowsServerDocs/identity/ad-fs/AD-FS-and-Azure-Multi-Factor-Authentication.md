---
ms.assetid: 0f7e56fe-1cbc-43ff-bb87-f4672137ed7f
title: "廣告 FS 和 Azure 多重要素驗證"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 67ce41bbd351bedc8d87e31ddb79c48338e44de9
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: zh-TW
---
# <a name="ad-fs-and-azure-multi-factor-authentication"></a>廣告 FS 和 Azure 多重要素驗證

>適用於︰ Windows Server 2016

在 Windows Server 2016 的廣告 FS 導入和多重要素驗證功能與 Windows Server 2012 R2 引進了而建置。   引入建 Azure MFA 介面卡，安裝和使用 Azure MFA 做為主要的驗證設定提供者已不簡單。

## <a name="why-use-ad-fs-with-azure-mfa"></a>為何使用 Azure MFA 廣告 FS
目前也有一些限制以廣告 FS 使用穩固驗證。  在 Windows Server 2016 的廣告 FS，這些處理與組織可以利用 usuing Azure MFA 與廣告 FS 其實。  組織可以利用下列動作︰

-   廣告 FS 發電廠可以輕鬆地多重要素驗證使用過的設定或額外的基礎結構 Azure MFA 設定。

-   建 MFA 體驗很容易地與 Windows Server 2016 安裝

-   Azure MFA 可能已設定為主要驗證提供者內部替代驗證機制允許或外部網路。

## <a name="how-it-works"></a>它的運作方式
為了讓 Azure MFA 做為主要的驗證提供者，驗證提供者會被視為只其他提供者中之後建置。  就像表單或憑證驗證。  只有不足的部分將會提供者會當作透過只公開實作 Azure MFA 提供者介面外接式驗證提供者。

下圖顯示在 Windows Server 2016 的廣告 FS 之間 Azure MFA 資料流。

![廣告 FS 和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_1.png)

## <a name="before-you-get-started"></a>在您開始之前
以下是資訊，您還必須知道的之前先嘗試使用 Azure MFA 做為主要的驗證提供者的清單。

-   為了設定為主要的驗證方法 Azure MFA 您需要有 Azure Active Directory。

-   設定完成主要驗證方法 Azure MFA 使用 Azure AD 連接。  您必須使用下列方法下列其中一項設定，並將其設定

-   此功能不適混合模式。  您的整個廣告 FS 發電廠必須 aWindows Server 2016

## <a name="using-azure-ad-connect-to-setup-azure-mfa-as-primary-authentication-provider"></a>設定為主要驗證提供者 Azure MFA 使用 Azure AD 連接
Azure MFA 像使用您的主要驗證提供者混合案例，因為您可以設定，並使用 Azure AD 連接精靈安裝它。  而定，您就會將它設，或在位置已經 Azure AD 連接仍然可用它來安裝和使用 Azure MFA 廣告 FS 的設定。  請使用以下程序

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-ad-fs"></a>設定與 Azure AD 連接-新安裝的 Azure AD 連接與廣告 FS Azure MFA
如果這是新的安裝 simplyIf 全新安裝，只要遵循指示執行設定可以找到 Azure AD 連接與廣告 FS 聯盟[以下](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/)。   當您進入 [檢視選項] 畫面時，請確定已選取 [適用於廣告 FS 設定 Azure MFA，按一下 [確認]。   Azure MFA 做為主要的驗證提供者會自動設定 automatically.When 您進入 [檢視選項] 畫面時，請務必**廣告 fs 設定 Azure MFA**已選取，然後按一下**確認**。   Azure MFA 做為主要的驗證提供者會自動設定。

![廣告 FS 和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_2.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-existing-ad-fs"></a>設定與 Azure AD 連接-新安裝的 Azure AD 連接和現有廣告 FS Azure MFA
如果您已經安裝廣告 FS 且現在前往安裝 Azure AD 連接，您需要只要依照指示執行設定 Azure AD 連接與廣告 FS 聯盟可以找到[以下](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/)，請務必選取現有的廣告 FS 伺服器發電廠。   當您進入 [檢視選項] 畫面時，請務必**廣告 fs 設定 Azure MFA**已選取，然後按一下**確認。** Azure MFA 做為主要的驗證提供者會自動設定。  .

![廣告 FS 和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_3.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---existing-azure-ad-connect-and-existing-ad-fs"></a>設定與 Azure AD 連接-現有 Azure AD 連接和現有廣告 FS Azure MFA
如果您已經安裝 Azure AD 連接與廣告 FS，然後再試一次和其他的工作，您可以直接執行 Azure AD 連接精靈選取**的我的廣告 FS 農場設定 Azure MFA**再依照完成精靈執行。

![廣告 FS 和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_4.png)

## <a name="setting-up-azure-mfa-with-server-manager-in-windows-server-2016"></a>Azure MFA 與伺服器經理 Windows Server 2016 的設定



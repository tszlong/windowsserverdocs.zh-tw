---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: 使用裝置註冊服務設定同盟伺服器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: cba3f26a2c5e3d17e1c29609e44115c667838613
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963069"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>使用裝置註冊服務設定同盟伺服器

\( \) 完成[步驟4：設定同盟伺服器](/previous-versions/orphan-topics/ws.11/dn303424(v=ws.11))中的程式之後，您可以在同盟伺服器上啟用裝置註冊服務 DRS。 裝置註冊服務會提供上線機制，以進行無縫式的第二因素驗證、持續的單一登入 \- \( SSO \) ，以及要求存取公司資源的取用者的條件式存取。 如需 DRS 的詳細資訊，請參閱[從任何裝置加入工作場所以進行 SSO，以及跨公司應用程式進行無縫第二因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

## <a name="prepare-your-active-directory-forest-to-support-devices"></a>準備您的 Active Directory 樹系以支援裝置

> [!NOTE]
> 這是一項一次性作業 \- ，您必須執行此作業，才能準備 Active Directory 樹系以支援裝置。 您必須以企業系統管理員權限登入，且您的 Active Directory 樹系必須具有 Windows Server 2012 R2 結構描述才能完成此程序。
>
> 此外，DRS 要求您的樹系根域中至少要有一部通用類別目錄伺服器。 需要通用類別目錄伺服器，才能在 \- AD FS 驗證期間執行 Initialize initialize-addeviceregistration 和。 AD FS \- 會在每個驗證要求上初始化 DRS config 物件的記憶體中標記法，而且如果在目前網域的 DC 上找不到 drs config 物件，則會嘗試針對在初始化 initialize-addeviceregistration 期間布建 DRS 物件的 GC 進行要求 \- 。

#### <a name="to-prepare-the-active-directory-forest"></a>準備 Active Directory 樹系

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗並輸入：

    ```
    Initialize-ADDeviceRegistration
    ```

2.  出現 ServiceAccountName 的提示時，請輸入您選取作為 AD FS 服務帳戶的服務帳戶名稱。  如果它是 gMSA 帳戶，請以**domain \\ accountname $** 格式輸入帳戶。 若為網域帳戶，請使用**domain \\ accountname**格式。

## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>啟用同盟伺服器陣列節點上的裝置註冊服務

> [!NOTE]
> 您必須以網域系統管理員許可權登入，才能完成此程式。

#### <a name="to-enable-device-registration-service"></a>啟用裝置註冊服務

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗並輸入：

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  在 AD FS 伺服器陣列中的每個同盟伺服器陣列節點上重複此步驟。

## <a name="enable-seamless-second-factor-authentication"></a>啟用無縫式的第二因素驗證
無縫的第二因素驗證是 AD FS 中的增強功能，可針對嘗試存取的外部裝置，提供對公司資源和應用程式的額外存取保護層級。 當個人裝置加入工作地點時，它會變成「已知」的裝置，而系統管理員可以使用此資訊來驅動條件式存取和閘道存取資源。

#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>若要啟用無縫式的第二因素驗證，已 \- \( \) 加入工作場所裝置的持續單一登入 SSO 和條件式存取

1.  在 [AD FS 管理] 主控台中，流覽至 [驗證原則]。 選取 [啟用全域主要驗證]。 選取 [啟用裝置驗證]，然後按一下 [確定]。

## <a name="update-the-web-application-proxy-configuration"></a>更新 Web 應用程式 Proxy 設定

> [!IMPORTANT]
> 您不需要將裝置註冊服務發佈至 Web 應用程式 Proxy。  一旦在同盟伺服器上啟用「Web 應用程式 Proxy」，就可以使用「裝置註冊服務」。  如果您在啟用裝置註冊服務之前已部署 Web 應用程式 Proxy 設定，則您可能需要完成此程式來更新它。

#### <a name="to-update-the-web-application-proxy-configuration"></a>若要更新 Web 應用程式 Proxy 設定

1.  在您的 Web 應用程式 Proxy 伺服器上，開啟 Windows PowerShell 命令視窗，然後輸入

    ```
    Update-WebApplicationProxyDeviceRegistration
    ```

2.  當系統提示您提供認證時，請輸入對您的同盟伺服器具有管理許可權之帳戶的認證。

## <a name="see-also"></a>另請參閱

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)


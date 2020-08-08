---
title: 正在設定 Azure 整合
description: 設定 Azure 整合 Windows 管理中心 (Project 檀香山) 。 將您的 Windows 管理中心閘道連接到 Azure。
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: c0a19e9bf00667e142c3aa6585c26b69c63e2aa7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997351"
---
# <a name="configuring-azure-integration"></a>正在設定 Azure 整合

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 系統管理中心支援數個與 Azure 服務整合的選用功能。 [瞭解 Windows 管理中心提供的 Azure 整合選項。](./index.md)

若要允許 Windows 管理中心閘道與 Azure 通訊，以利用 Azure Active Directory 驗證來進行閘道存取，或代表您建立 Azure 資源 (例如，若要使用 Azure Site Recovery) 來保護在 Windows 管理中心內管理的 Vm，您必須先向 Azure 註冊您的 Windows 管理中心閘道。 您只需要針對您的 Windows 管理中心閘道執行此動作-當您將閘道更新為較新版本時，會保留此設定。

## <a name="register-your-gateway-with-azure"></a>向 Azure 註冊您的閘道

第一次嘗試在 Windows 管理中心使用 Azure 整合功能時，系統會提示您向 Azure 註冊閘道。 您也可以前往 Windows 管理中心 [設定] 中的 [ **Azure** ] 索引標籤來註冊閘道。 請注意，只有 Windows 管理中心閘道系統管理員可以向 Azure 註冊 Windows 管理中心閘道。 [深入瞭解 Windows 管理中心使用者和系統管理員許可權](../configure/user-access-control.md#gateway-access-role-definitions)。

引導式的產品中步驟會在您的目錄中建立 Azure AD 應用程式，讓 Windows 系統管理中心能夠與 Azure 通訊。 若要查看自動建立的 Azure AD 應用程式，請移至 Windows 管理中心 [設定] 的 [ **Azure** ] 索引標籤。 Azure 超連結**中的 view**可讓您在 Azure 入口網站中查看 Azure AD 應用程式。

建立的 Azure AD 應用程式會用於 Windows 系統管理中心內的所有 Azure 整合點，包括[對閘道的 Azure AD 驗證](../configure/user-access-control.md#azure-active-directory)。 Windows 管理中心會自動設定建立和管理 Azure 資源所需的許可權，以代表您：

- Azure Active Directory 圖形
    - Directory.AccessAsUser.All
    - User.Read
- Azure 服務管理
    - user_impersonation

### <a name="manual-azure-ad-app-configuration"></a>手動 Azure AD 應用程式設定

如果您想要手動設定 Azure AD 應用程式，而不是使用 Windows 系統管理中心自動建立的 Azure AD 應用程式，則必須執行下列步驟。

1. 授與 Azure AD 應用程式上述所列的必要 API 許可權。 若要這麼做，您可以流覽至 Azure 入口網站中的 Azure AD 應用程式。 移至 Azure 入口網站 > **Azure Active Directory**  >  **應用程式註冊**> 選取您要使用的 Azure AD 應用程式。 然後，前往 [ **API 許可權**] 索引標籤，並新增上面所列的 api 許可權。
2. 將 Windows 管理中心閘道 URL 新增至 [回復 Url] (也稱為 [重新導向 Uri]) 。 流覽至您的 Azure AD 應用程式，然後移至 [**資訊清單**]。 在資訊清單中尋找 "replyUrlsWithType" 索引鍵。 在機碼中，新增包含兩個索引鍵的物件： "url" 和 "type"。 金鑰 "url" 應具有 Windows 管理中心閘道 URL 的值，並在結尾附加萬用字元。 金鑰「類型」索引鍵的值應為「Web」。 例如：

    ```json
    "replyUrlsWithType": [
            {
                    "url": "http://localhost:6516/*",
                    "type": "Web"
            }
    ],
    ```
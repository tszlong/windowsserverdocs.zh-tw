---
title: Settings
description: 了解有關 Windows Admin Center (Project Honolulu) 的設定。 使用者設定可讓使用者變更其語言/區域和其他喜好設定。 閘道設定可讓系統管理員設定閘道。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: e0fd6618f275058d4e22fe9abb9e484d4752ac9a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71407050"
---
# <a name="windows-admin-center-settings"></a>Windows Admin Center 設定

> 適用於：Windows Admin Center

Windows Admin Center 設定包含使用者層級和閘道層級的設定。 對使用者層級設定所做的變更只會影響目前使用者的設定檔，而對閘道層級設定所做的變更會影響該 Windows Admin Center 閘道上的所有使用者。

## <a name="user-settings"></a>使用者設定

使用者層級設定包含下列區段：

- 帳戶
- Personalization
- 語言/地區
- 建議
- 進階

在 [帳戶]  索引標籤中，使用者可以檢閱他們用來向 Windows Admin Center 進行驗證的認證。 如果 Azure AD 設定為身分識別提供者，則使用者可以從這個索引標籤登出其 Azure AD 帳戶。

在 [個人化]  索引標籤中，使用者可以切換到深色 UI 主題。

在 [語言/區域]  索引標籤中，使用者可以變更 Windows Admin Center 所顯示的語言和地區格式。

在 [建議]  索引標籤中，使用者可以切換 Azure 服務和新功能的相關建議。

[進階]  索引標籤可為 Windows Admin Center 擴充功能開發人員提供額外的功能。

## <a name="gateway-settings"></a>閘道設定

閘道層級設定包含下列區段：

- Extensions
- 存取權
- Azure
- 共用的連線

只有閘道系統管理員能夠查看及變更這些設定。 對這些設定所做的變更會變更閘道的設定，並影響 Windows Admin Center 閘道的所有使用者。

在 [擴充功能]  索引標籤中，系統管理員可以安裝、卸載或更新閘道擴充功能。 [深入了解擴充功能。](using-extensions.md)

[存取]  索引標籤可讓系統管理員設定可存取 Windows Admin Center 閘道的人員，以及用來驗證使用者的身分識別提供者。 [深入了解控制閘道的存取權。](user-access-control.md)

從 [Azure]  索引標籤中，系統管理員可以向 Azure 註冊閘道，以在 Windows Admin Center 中啟用 [Azure 整合功能](azure-integration.md)。

系統管理員可以使用 [共用連線]  索引標籤，設定要在 Windows Admin Center 閘道的所有使用者之間共用的單一連線清單。 [深入了解如何一次設定所有閘道使用者的連線。](shared-connections.md)
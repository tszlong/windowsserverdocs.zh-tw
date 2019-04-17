---
title: 設定
description: 深入了解 Windows Admin Center (Project Honolulu) 中的設定。 使用者設定可讓使用者變更其語言/地區和其他喜好設定。 閘道設定可讓系統管理員設定閘道。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296620"
---
# Windows Admin Center 設定

> 適用於： Windows Admin Center

Windows Admin Center 設定是由使用者層級和閘道層級設定所組成。 使用者層級設定的變更只會影響目前的使用者設定檔，雖然閘道層級設定的變更會影響該 Windows Admin Center 閘道上的所有使用者。

## 使用者設定

使用者層級的設定包含下列各節：

- 帳戶
- Personalization
- 語言/地區
- 建議
- 進階

在 [**帳戶**] 索引標籤中，使用者可以檢閱他們用來驗證 Windows Admin Center 的認證。 如果設定 Azure AD 的身分識別提供者，使用者可以登出他們的 Azure AD 帳戶，從這個索引標籤。

在 [**個人化**] 索引標籤中，使用者可以切換至深色的 UI 佈景主題。

在 [**語言/地區**] 索引標籤中，使用者可以變更 Windows Admin Center 所顯示的語言和地區格式。

在 [**建議**] 索引標籤中，使用者可以切換 Azure 服務和新功能的相關建議。

**進階**] 索引標籤提供 Windows Admin Center 擴充功能開發人員額外的功能。

## 閘道設定

閘道層級的設定包含下列各節：

- 擴充功能
- Access
- Azure
- 共用的連線

只有閘道的系統管理員可查看和變更這些設定。 變更這些設定變更閘道的組態，而且會影響所有使用者的 Windows Admin Center 閘道。

在 [**擴充功能**] 索引標籤中，系統管理員可以安裝、 解除安裝，或更新閘道擴充功能。 [深入了解擴充功能。](using-extensions.md)

**存取**] 索引標籤可讓系統管理員設定誰可以存取 Windows Admin Center 閘道，以及用來驗證使用者的身分識別提供者。 [深入了解控制閘道的存取。](user-access-control.md)

從**Azure**索引標籤中，系統管理員可以註冊閘道與 Azure 以啟用 Windows Admin Center 中的[Azure 整合功能](azure-integration.md)。

使用**共用的連線**] 索引標籤，系統管理員可以設定單一跨 Windows Admin Center 閘道的所有使用者共用的連線的清單。 [深入了解設定一次為所有使用者的閘道的連線。](shared-connections.md)
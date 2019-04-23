---
title: 設定
description: 了解 Windows Admin Center （專案檀香山） 中的設定。 使用者設定可讓使用者變更其語言/地區和其他喜好設定。 設定閘道的系統管理員可使用閘道設定。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882379"
---
# <a name="settings"></a>設定

> 適用於：Windows Admin Center

Windows Admin Center 的設定是由使用者層級和閘道層級設定所組成。 使用者層級設定變更只會影響目前的使用者設定檔，而閘道層級設定的變更會影響該 Windows Admin Center 閘道上的所有使用者。

## <a name="user-settings"></a>使用者設定

使用者層級設定是由下列各節所組成：

- 帳戶
- 語言/地區
- 建議

在 **帳戶**索引標籤上，使用者可以檢閱它們用來驗證 Windows Admin Center 的認證。 如果 Azure AD 設定為身分識別提供者，使用者可以登出其 Azure AD 帳戶，從這個索引標籤中。

在 **語言/地區**索引標籤上，使用者可以變更 Windows Admin Center 所顯示的語言和地區格式。

在 **建議**索引標籤上，使用者可以切換相關 Azure 服務和新功能建議。

### <a name="dark-theme"></a>暗色調佈景主題

> 適用於：Windows Admin Center 預覽版

在 Windows Admin Center 預覽版中，您可以解除鎖定額外**個人化**區段，其中包含可切換至 暗色調的 UI 佈景主題選項。 若要啟用**個人化**區段中，輸入```msft.sme.shell.personalization```做為實驗的索引鍵。

>[!IMPORTANT]
> 深色佈景主題是進行中工作，請在此階段中執行不報告錯誤。

## <a name="gateway-settings"></a>閘道設定

閘道層級設定是由下列各節所組成：

- 延伸模組
- 存取權
- Azure

只有閘道系統管理員就可以看到及變更這些設定。 變更這些設定變更閘道的組態，而且會影響 Windows Admin Center 閘道的所有使用者。

在 **延伸模組**索引標籤上，系統管理員可以安裝、 解除安裝，或更新閘道擴充功能。 [深入了解延伸模組。](using-extensions.md)

**存取** 索引標籤可讓管理員設定誰可以存取的 Windows Admin Center 閘道，以及用來驗證使用者的身分識別提供者。 [深入了解控制閘道存取權。](user-access-control.md)

從**Azure**索引標籤上，系統管理員可以將閘道向 Azure 註冊以便[Azure 的整合功能](azure-integration.md)Windows Admin Center 中。
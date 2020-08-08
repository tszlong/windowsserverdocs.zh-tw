---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: 檢閱 OU 設計概念
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 99b358d503a165a72785bd09ead8825b08d175a0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972265"
---
# <a name="reviewing-ou-design-concepts"></a>檢閱 OU 設計概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網域的組織單位 (OU) 結構包括下列各項：

-   OU 階層結構的圖表

-   Ou 清單

-   針對每個 OU：

    -   OU 的用途

    -   可控制 ou 或 OU 中物件的使用者或群組清單

    -   使用者和群組在 OU 中的物件上所擁有的控制項類型

OU 階層不需要反映組織或群組的部門階層。 Ou 是針對特定用途所建立，例如管理委派、群組原則的應用程式，或限制物件的可見度。

您可以設計 OU 結構來將管理委派給組織內的個人或群組，而這些小組需要自我管理自己的資源和資料。 Ou 代表系統管理界限，並可讓您控制資料管理員的授權範圍。

例如，您可以建立名為 ResourceOU 的 OU，並用它來儲存屬於群組所管理之檔案和列印伺服器的所有電腦帳戶。 然後，您可以在 OU 上設定安全性，讓群組中的資料管理員只能存取 OU。 這可防止其他群組中的資料管理員篡改檔案和列印伺服器帳戶。

您可以針對特定用途建立 Ou 的子樹，以進一步精簡您的 OU 結構，例如群組原則的應用程式，或限制受保護物件的可見度，讓只有特定使用者可以看到它們。 例如，如果您需要將群組原則套用至選取的使用者或資源群組，您可以將這些使用者或資源新增至 OU，然後將群組原則套用至該 OU。 您也可以使用 OU 階層來啟用進一步的系統管理控制委派。

雖然 OU 結構中的層級數目沒有任何技術限制，但為了方便管理，我們建議您將 OU 結構限制為不超過10個層級的深度。 每個層級上的 Ou 數目沒有任何技術限制。 請注意，Active Directory Domain Services (AD DS) 啟用的應用程式可能會限制分辨名稱中使用的字元數 (也就是完整的輕量型目錄存取協定 (LDAP) 目錄) 中的物件路徑，或階層內的 OU 深度。

AD DS 中的 OU 結構不適合用戶看到。 OU 結構是服務管理員和資料管理員的系統管理工具，而且很容易變更。 繼續檢查並更新 OU 結構設計，以反映系統管理結構中的變更，並支援以原則為基礎的系統管理。




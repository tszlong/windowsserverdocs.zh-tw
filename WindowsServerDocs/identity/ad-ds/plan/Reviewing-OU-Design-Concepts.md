---
description: 深入瞭解：檢查 OU 設計概念
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: 檢閱 OU 設計概念
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2741b8871c82ec28add21d948a4d650ad4182442
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042566"
---
# <a name="reviewing-ou-design-concepts"></a>檢閱 OU 設計概念

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

網域 (OU) 結構的組織單位包括下列各項：

-   OU 階層的圖表

-   Ou 清單

-   針對每個 OU：

    -   OU 的用途

    -   可以控制 ou 或 OU 中物件的使用者或群組清單

    -   使用者和群組針對 OU 中的物件所擁有的控制項類型

OU 階層不需要反映組織或群組的部門階層。 系統會針對特定用途建立 Ou，例如管理委派、群組原則的應用程式，或限制物件的可見度。

您可以設計 OU 結構，將系統管理委派給組織內需要自主性來管理自己資源和資料的個人或群組。 Ou 代表系統管理界限，可讓您控制資料管理員的授權範圍。

例如，您可以建立名為 ResourceOU 的 OU，並用它來儲存屬於檔案的所有電腦帳戶，以及由群組管理的列印伺服器。 然後，您可以設定 OU 的安全性，如此一來，只有群組中的資料管理員可以存取 OU。 這可防止其他群組中的資料管理員篡改檔案和列印伺服器帳戶。

您可以針對特定用途建立 Ou 的子樹，例如群組原則的應用程式，或限制受保護物件的可見度，讓只有特定使用者可以看到它們，藉以進一步精簡 OU 結構。 例如，如果您需要將群組原則套用至選取的使用者或資源群組，您可以將這些使用者或資源新增至 OU，然後將群組原則套用至該 OU。 您也可以使用 OU 階層來啟用系統管理控制的進一步委派。

雖然您的 OU 結構中的層級數目沒有技術上的限制，但為了方便管理，我們建議您將 OU 結構限制為不超過10個層級的深度。 每個層級上的 Ou 數量沒有任何技術性限制。 請注意，Active Directory Domain Services (AD DS) 啟用的應用程式可能會限制分辨名稱中使用的字元數 (也就是說，目錄 (中物件的完整輕量型目錄存取協定) LDAP) 路徑，或階層內的 OU 深度。

AD DS 中的 OU 結構不能讓使用者看到。 OU 結構是服務管理員和資料管理員的系統管理工具，而且很容易變更。 繼續查看並更新您的 OU 結構設計，以反映系統管理結構中的變更，並支援以原則為基礎的系統管理。




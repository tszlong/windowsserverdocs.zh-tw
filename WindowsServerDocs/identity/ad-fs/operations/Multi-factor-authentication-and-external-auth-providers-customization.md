---
description: 深入瞭解：多重要素驗證和外部驗證提供者自訂
title: 多重要素驗證和外部驗證提供者自訂
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.openlocfilehash: 55ae8f4630c21ceb5cde91982970ff52c10047da
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039736"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多重要素驗證和外部驗證提供者自訂

在 AD FS 中，提供多因素驗證的支援 \- \- \- 。 例如，您可以將 AD FS 設定為使用 \- 內建的憑證驗證，作為第二個因素驗證。 您也可以使用外部驗證提供者。 這種方法可讓 AD FS 與其他服務整合，例如 Azure 多重要素驗證，或您可以開發自己的提供者。 請參閱 [解決方案指南：使用多 \- 因素存取控制管理風險](./manage-risk-with-conditional-access-control.md) ，以取得如何使用 AD FS 註冊外部驗證提供者的詳細資訊。

建議外部驗證提供者使用 AD FS 提供的 .css 檔案中所定義的類別，以撰寫驗證 UI。 您可以使用下列 Cmdlet 來匯出預設網頁佈景主題，並查看使用者介面類別和.css 檔案中定義的元素。 .Css 檔案可用於開發 \- 外部驗證提供者的登入使用者介面。

```powershell
Export-AdfsWebTheme -Name default -DirectoryPath C:\theme
```

以下是「登 \- 入」使用者介面的範例（以紅色反白顯示，由外部驗證提供者）。 使用者介面會使用 AD FS .css 檔案中的 UI 類別。

![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)

在您撰寫新的自訂驗證方法之前，建議您先研究 AD FS 主題和樣式定義，以瞭解內容撰寫需求。

-   自訂驗證方法只會編寫 AD FS 登入頁面上的 HTML 區段 \- ，而不是整個頁面。 您應使用 AD FS 的樣式定義，以取得一致的外觀和行為。

![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)

-   請注意，AD FS 系統管理員可以自訂 AD FS 的樣式。 . 不建議以硬式編碼的方式自行編寫樣式。 相反地，我們建議盡可能使用 AD FS 的樣式。

-   現成 \- 的 \- AD FS 樣式是由左 \- 到 \- 右的 \( LTR \) 樣式以及一個由右 \- 至左的 \- RTL \( \) 所撰寫。 系統管理員可以自訂這兩者，也可以 \- 透過 web 主題定義提供語言特定樣式。 每個樣式表有三個區段，並有各自的註解：

    -   **主題樣式** \- 這些樣式不應使用，也不能使用。 這些樣式是用來定義所有頁面的佈景主題。 它們是刻意由元素 ID 所定義，以避免被重複使用。

    -   **常見樣式** \- 這些是您的內容應該使用的樣式。

    -   **外型規格樣式** \- 這些是不同外型規格的樣式。 您應該了解此區段，以確定內容可搭配不同的尺寸，例如手機和平板電腦。

如需其他資訊，請參閱 [解決方案指南：使用多 \- 因素存取控制管理風險](./manage-risk-with-conditional-access-control.md) 和 [解決方案指南：透過其他多 \- 因素驗證管理機密應用程式的風險](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。

## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)

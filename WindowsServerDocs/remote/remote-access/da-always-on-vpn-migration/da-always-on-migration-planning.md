---
title: 遠端存取 Always On VPN 遷移規劃
description: 從 DirectAccess 遷移至 Always On VPN 需要適當的規劃來判斷您的遷移階段，這有助於在問題影響整個組織之前識別出任何問題。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/29/2018
ms.openlocfilehash: 80a7a8b3ee13a9d9cc99b81ab917f6443147565f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314945"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>規劃 DirectAccess 移轉至 Always On VPN

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

&#171;[**上一步：** 瞭解 DIRECTACCESS 以 Always On VPN 遷移](da-always-on-migration-overview.md)<br>
&#187;[**下一步：** 遷移至 Always On VPN 並解除委任 DirectAccess](da-always-on-migration-deploy.md)


從 DirectAccess 遷移至 Always On VPN 需要適當的規劃來判斷您的遷移階段，這有助於在問題影響整個組織之前識別出任何問題。 遷移的主要目的是讓使用者在整個過程中維持對辦公室的遠端連線。 如果您按循序執行工作，可能會發生競爭情況，讓遠端使用者無法存取公司資源。 因此，Microsoft 建議執行規劃的並存從 DirectAccess 遷移至 Always On VPN。 如需詳細資訊，請參閱[ALWAYS ON VPN 遷移部署](da-always-on-migration-deploy.md)一節。

本節說明將使用者與遷移、標準設定考慮和 Always On VPN 功能增強功能分開的優點。 遷移規劃階段包括：

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>建立遷移環形
遷移環形用來將 Always On 的 VPN 用戶端遷移工作劃分成多個階段。 當您到達最後一個階段時，您的程式應該經過妥善測試且一致。

本節提供一種方法，可將使用者分隔成遷移階段，然後管理這些階段。 無論您選擇何種使用者階段分隔方法，都可以維護單一 VPN 使用者群組，以便在完成遷移時更輕鬆地進行管理。

>[!NOTE] 
>「單字」_階段_並非用來指出這是很長的進程。 不論您是在幾天或幾個月內移動每個階段，Microsoft 建議您利用並存遷移並使用階段式方法。

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>將遷移工作劃分成多個階段的優點

-   **大量中斷保護。** 藉由將遷移分成幾個階段，遷移產生的問題可能會有多大的影響。

-   **改進程式或來自意見反應的通訊。** 在理想的情況下，使用者並不會注意到發生遷移。 不過，如果他們的經驗不佳，這些使用的意見反應可讓您有機會改善您的規劃，並避免未來發生問題。

### <a name="tips-for-building-your-migration-ring"></a>建立您的遷移信號的秘訣

-   **識別遠端使用者。** 一開始先將使用者分成兩個值區：經常進入辦公室的人，以及不是誰。 這兩個群組的遷移程式都相同，但遠端用戶端可能需要較長的時間來接收更新，而不是較常連接的使用者。 在理想的情況下，每個遷移階段應該包含每個值區的成員。

-  **設定使用者的優先順序。** 領導和其他高影響力使用者通常是最後一次遷移的使用者。 不過，在排定使用者的優先順序時，如果用戶端電腦的遷移失敗，請考慮其企業生產力的影響。 例如，如果您的評等為1到3，而1表示員工無法正常工作，3表示沒有立即的工作中斷，則只使用內部企業營運（LOB）應用程式的商務分析師會是1，而銷售人員則是使用雲端應用程式會是3。

-   **以多個階段遷移每個部門或業務單位。** Microsoft 強烈建議您不要同時遷移整個部門。 如果發生問題，您不希望它阻礙整個部門的遠端工作。 相反地，請在至少兩個階段中遷移每個部門或業務單位。

-   **逐漸增加使用者計數。** 最常見的遷移案例是從 IT 組織的成員開始，然後移至商務使用者，然後再前往領導和其他高影響力的使用者。 每個遷移階段通常牽涉到越來越多的人。 例如，第一個階段可能包含十個使用者，而最終群組可能包含5000個使用者。 若要簡化部署，請建立單一 VPN 使用者安全性群組，並在其階段抵達時將使用者新增至其中。 如此一來，您就會得到單一 VPN 使用者群組，您可以在未來新增成員。

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>後續步驟

|如果您要...  |然後查看 。  |
|---------|---------|
|開始遷移至 Always On VPN     |[遷移至 ALWAYS ON VPN 並解除委任 DirectAccess](da-always-on-migration-deploy.md)。 從 DirectAccess 遷移至 Always On VPN 需要特定的程式來遷移用戶端，這有助於減少因執行遷移步驟而造成的競爭情形不按照順序。         |
|瞭解 Always On VPN 和 DirectAccess 的功能    |[ALWAYS ON VPN 和 DirectAccess 的功能比較](../vpn/vpn-map-da.md)。 在舊版的 Windows VPN 架構中，平臺限制使得您難以提供取代 DirectAccess 所需的重要功能（例如，在使用者登入之前起始的自動連線）。 不過，Always On VPN 已經減輕了這些限制，或擴充了 DirectAccess 功能以外的 VPN 功能。         |



---
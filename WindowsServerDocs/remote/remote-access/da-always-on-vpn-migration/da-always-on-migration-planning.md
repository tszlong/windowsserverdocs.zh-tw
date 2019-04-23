---
title: 規劃遠端存取一律開啟 」 VPN 移轉
description: 從 DirectAccess 移轉至一律開啟 」 VPN 需要適當規劃，以判斷您的移轉階段，可協助識別任何問題影響到整個組織之前。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 494dc7916b505991c22b07bec738c2300d660ec1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835669"
---
# <a name="plan-the-directaccess-to-always-on-vpn-migration"></a>規劃 DirectAccess 移轉至 Always On VPN

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

&#171;[**前一個：** DirectAccess，讓它一律開啟 」 VPN 移轉概觀](da-always-on-migration-overview.md)<br>
&#187;[**下一步:** 移轉到一律開啟 」 VPN，並解除委任 DirectAccess](da-always-on-migration-deploy.md)


從 DirectAccess 移轉至一律開啟 」 VPN 需要適當規劃，以判斷您的移轉階段，可協助識別任何問題影響到整個組織之前。 移轉的主要目標是為了在整個過程在辦公室的遠端連線的使用者。 如果您執行工作順序時，可能會發生競爭情形，讓遠端使用者，但沒有方法可以存取公司資源。 因此，Microsoft 建議一律開啟 」 VPN 從 DirectAccess 執行已規劃、 並排顯示的移轉。 如需詳細資訊，請參閱 <<c0> [ 一律開啟 」 VPN 移轉部署](da-always-on-migration-deploy.md)一節。

本節說明將使用者移轉、 標準組態考量及一律開啟 」 VPN 功能的增強功能的優點。 移轉規劃階段包含：

1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

## <a name="build-migration-rings"></a>建立移轉環形
移轉通道用來將一律開啟 」 VPN 用戶端移轉工作分割成多個階段。 您可以在最後一個階段的時間，您的程序應也經過測試且一致。

本節分成移轉階段中的使用者，再加以管理這些階段的其中一種方法。 完成移轉時，不論您選擇的使用者階段分隔方法，維持單一的 VPN 使用者群組，以便更容易管理。

>[!NOTE] 
>單字_階段_不是，表示這是較長的處理序。 是否您瀏覽數天或幾個月中每個階段，Microsoft 建議您利用並排顯示移轉，並使用階段性的方法。

### <a name="benefits-of-dividing-the-migration-effort-into-multiple-phases"></a>將移轉作業分割成多個階段的優點

-   **大量中斷的保護。** 藉由將分割成階段的移轉，移轉產生的問題可能會影響的人數是小很多。

-   **改進處理程序或意見回應的通訊。** 在理想情況下，使用者沒有不會注意，移轉便會發生。 不過，若其體驗不佳時，這些用法所得到的意見反應可讓您有機會改善您進行規劃，並避免未來發生問題。

### <a name="tips-for-building-your-migration-ring"></a>建置您的移轉通道的秘訣

-   **識別遠端使用者。** 著手分成兩個值區中的使用者： 經常進入辦公室以及不這麼做的任何人。 移轉程序是相同的兩個群組，但是也可能需要較長的時間，若要接收的連接更頻繁的人比更新遠端用戶端。 每個移轉階段，在理想情況下，應該包含來自每個值區的成員。

-  **設定使用者的優先順序。** 領導力和其他高影響使用者通常在最後一個使用者之間移轉。 時排列優先順序的使用者，不過，考慮其商務生產力的衝擊，如果其用戶端電腦的移轉失敗。 比方說，如果您有 1 到 3 的評等時，具有 1 的意義，員工不能工作 」 和 「 3，這表示不會立即的工作中斷，從遠端使用僅內部特定業務 (LOB) 應用程式的商務分析師會是 1，而銷售人員能在使用雲端 應用程式會是 3。

-   **移轉多個階段中的每個部門或營業單位。** Microsoft 強烈建議您不要在此同時移轉整個部門。 如果應該發生的問題，您不想它會妨礙整個部門的遠端工作。 相反地，移轉每個部門或營業單位在至少兩個階段。

-   **逐漸增加使用者計數。** 最常見的移轉案例開始 IT 組織中的成員，然後移動到後面領導力和其他高影響使用者的商務使用者。 每個移轉階段通常會包含漸進式的更多人。 比方說，第一個階段中可能包含十位使用者，並最終的群組可能包含 5,000 位使用者。 若要簡化部署，建立單一 VPN 使用者的安全性群組，並將使用者新增至它，其階段到達時。 如此一來，您最終會與單一 VPN 使用者群組，您可以在未來新增成員。

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="next-step"></a>後續步驟

|如果您想要...  |然後請參閱...  |
|---------|---------|
|開始移轉至一律開啟 」 VPN     |[移轉到一律開啟 」 VPN，並解除委任 DirectAccess](da-always-on-migration-deploy.md)。 從 DirectAccess 移轉至一律開啟 」 VPN 需要特定的程序，來移轉用戶端，可協助最小化所引發的執行移轉步驟順序的競爭情形。         |
|了解的一律開啟 」 VPN 和 DirectAccess 的功能    |[功能比較 Alwayson VPN 和 DirectAccess](../vpn/vpn-map-da.md)。 在舊版的 Windows VPN 架構中，平台限制並不容易以提供取代 DirectAccess （例如在使用者登入之前啟動的自動連線） 所需的重要功能。 不過，一律開啟 」 VPN，可能會有裝置減輕大部分的這些限制，或展開的 DirectAccess 功能的 VPN 功能。         |



---
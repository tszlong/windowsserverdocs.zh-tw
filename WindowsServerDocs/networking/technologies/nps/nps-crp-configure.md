---
title: 設定連線要求原則
description: 本主題提供如何在 Windows Server 2016 的網路原則伺服器中設定連線要求原則的相關資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7b42dd9470b44b0f1c7d25627d491cd6f2a2dfae
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316287"
---
# <a name="configure-connection-request-policies"></a>設定連線要求原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來建立和設定連線要求原則，以指定本機 NPS 是否處理連線要求，或轉送到遠端 RADIUS 伺服器進行處理。

連線要求原則是一組條件和設定，可讓網路系統管理員指定哪些遠端驗證撥入使用者服務（RADIUS）伺服器執行連線要求的驗證和授權，而 \(NPS 的伺服器\) 從 RADIUS 用戶端接收網路原則伺服器。

預設連線要求原則使用 NPS 做為 RADIUS 伺服器，並在本機處理所有的驗證要求。

若要將執行 NPS 的伺服器設定為 RADIUS Proxy，並將連線要求轉寄到其他 NPS 或 RADIUS 伺服器，除了新增連線要求原則以指定連線要求必須符合的條件與設定之外，還必須設定遠端 RADIUS 伺服器群組。

您可以透過 [新增連線要求原則精靈]，在建立新連線要求原則時，建立新的遠端 RADIUS 伺服器群組。

如果您不想讓 NPS 作為 RADIUS 伺服器，並在本機處理連線要求，您可以刪除預設連線要求原則。

如果您想要 NPS 同時做為 RADIUS 伺服器、在本機處理連線要求，以及做為 RADIUS proxy，請將一些連線要求轉送到遠端 RADIUS 伺服器群組，使用下列程式新增原則，然後驗證預設值[連線要求原則] 是最後一次在原則清單中進行處理的原則。

## <a name="add-a-connection-request-policy"></a>新增連線要求原則

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-add-a-new-connection-request-policy"></a>新增連接要求原則 

1. 在伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**] 以開啟 NPS 主控台。 
2. 在主控台樹中，按兩下 [**原則**]。
3. 在 [連線**要求原則**] 上按一下滑鼠右鍵，然後按一下 [**新增連線要求原則**]。
4. 使用 [新增連線要求原則精靈] 設定連線要求原則，如果之前未設定遠端 RADIUS 伺服器群組，也可以使用此精靈進行設定。


如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。


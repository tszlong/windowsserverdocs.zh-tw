---
title: 設定連線要求原則
description: 本主題提供有關如何設定 Windows Server 2016 中的網路原則伺服器的連線要求原則的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884509"
---
# <a name="configure-connection-request-policies"></a>設定連線要求原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來建立及設定連線要求原則，指定本機 NPS 處理連線要求，或將它們轉送到遠端 RADIUS 伺服器以進行處理。

連線要求原則是條件的集合，並讓網路系統管理員指定哪些遠端驗證撥號使用者服務 (RADIUS) 伺服器的設定，請執行驗證和授權連線要求執行網路原則伺服器的伺服器\(NPS\)接收自 RADIUS 用戶端。

預設連線要求原則使用 NPS 做為 RADIUS 伺服器，並處理所有驗證要求在本機。

若要設定執行 NPS 做為 RADIUS proxy，並將連線要求轉寄到其他 NPS 或 RADIUS 伺服器的伺服器，您必須設定遠端 RADIUS 伺服器群組，除了新增新的連線要求原則，指定條件和設定，必須符合連線要求。

使用新的連線要求原則精靈建立新的連線要求原則時，您可以建立新的遠端 RADIUS 伺服器群組。

如果您不想做為 RADIUS 伺服器與處理序的連接要求在本機 NPS，您可以刪除預設連線要求原則。

如果您想要做為這兩個 RADIUS 伺服器，處理連接要求在本機，並做為 RADIUS proxy，將一些連線要求轉送到遠端 RADIUS 伺服器群組，加入新的原則，使用下列程序，然後確認 NPS 預設值連線要求原則是最後放置在原則清單中的最後處理的原則。

## <a name="add-a-connection-request-policy"></a>新增連線要求原則

若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-add-a-new-connection-request-policy"></a>若要新增新的連線要求原則 

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**開啟 NPS 主控台。 
2. 在主控台樹狀目錄中，按兩下**原則**。
3. 以滑鼠右鍵按一下**連線要求原則**，然後按一下**新的連線要求原則**。
4. 使用 新連線要求原則精靈來設定您的連線要求原則，如果之前未設定，遠端 RADIUS 伺服器群組。


如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。


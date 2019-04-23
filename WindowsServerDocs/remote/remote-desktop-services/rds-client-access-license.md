---
title: 使用用戶端存取使用權 (Cal) 授權您的 RDS 部署
description: 在 遠端桌面服務中授權的用戶端的概觀。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853439"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>使用用戶端存取使用權 (Cal) 授權您的 RDS 部署

>適用於：Windows Server （半年通道），Windows Server 2016

每個使用者和裝置連線到遠端桌面工作階段主機需要用戶端存取使用權 (CAL)。 您使用 RD 授權來安裝、 簽發以及追蹤 RDS Cal。  

當使用者或裝置連線到 RD 工作階段主機伺服器時，RD 工作階段主機伺服器會判斷是否需要 RDS CAL。 然後，RD 工作階段主機伺服器會向遠端桌面授權伺服器要求 RDS CAL。 適當的 RDS CAL 是否可向授權伺服器，RDS CAL 發行給用戶端，而且用戶端能夠連線到 RD 工作階段主機伺服器，以及從該處到桌上型電腦或他們想要使用的應用程式。

雖然沒有任何授權伺服器是必要的之後在寬限期結束後，用戶端必須具有有效的 RDS CAL，由授權伺服器，然後再發出, 授權寬限期他們可以登入 RD 工作階段主機伺服器。

使用下列資訊以了解用戶端存取授權的運作方式在遠端桌面服務，以及部署和管理您的授權：

- [了解 Cal 模型](#understanding-the-cals-model)
- [啟用授權伺服器](rds-activate-license-server.md)
- [授權伺服器上安裝 RDS Cal](rds-install-cals.md)
- [追蹤您的部署中使用的 Cal](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>了解 Cal 模型

有兩種類型的 Cal:

- RDS 每一裝置 Cal
- RDS 每個使用者 Cal

下表概述兩種 Cal 的類型之間的差異：

| 每個裝置                                                     | 每位使用者                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Cal 實際指派給每個裝置。                   | Cal 會指派給 Active Directory 中的使用者。                                 |
| Cal 會追蹤由授權伺服器。                        | Cal 會追蹤由授權伺服器。                                          |
| 可以追蹤 Cal，而不論 Active Directory 成員資格。 | Cal 無法追蹤在工作群組內。                                       |
| 您可以撤銷 20%的 Cal。                              | 您無法撤銷任何 Cal。                                                      |
| 暫存的 Cal 就能有效 52 – 89 天。                       | 無法使用暫存的 Cal。                                                |
| 無法過度 Cal。                                  | （在遠端桌面授權合約的缺口），可以過度 Cal。 |

當您使用每一裝置模型時，臨時的授權就會發出第一次裝置連線到 RD 工作階段主機。 第二次該裝置連線時，只要授權伺服器已啟用，而且有可用的 Cal，授權伺服器問題永久的 RDS 每一裝置 CAL。

當您使用的每個使用者模型時，授權會不會強制執行，而且從任何數目的裝置連線到 RD 工作階段主機的授權授與每個使用者。 授權伺服器會發出授權從可用的 CAL 集區或 Over-Used CAL 的集區。 您必須負責確保所有使用者擁有有效的授權，以及零 Over-Used Cal，您是在遠端桌面服務的授權條款的違規情形的否則為。

若要確保您符合遠端桌面服務的授權條款，追蹤的 RDS 每個使用者 Cal 數量在組織中使用，並確定有足夠每個使用者 Cal 的所有使用者的授權伺服器上安裝。

您可以使用遠端桌面授權管理員，來追蹤及產生報告 RDS 每個使用者 Cal。

## <a name="note-about-cal-versions"></a>CAL 版本的注意事項

由使用者或裝置的 CAL 必須對應到使用者或裝置連接到的 Windows Server 的版本。 您無法存取較新的 Windows Server 版本中，使用較舊的 Cal，但您可以存取舊版的 Windows Server 使用較新的 Cal。

下表顯示相容 RD 工作階段主機及 RD 虛擬化主機的 Cal。

|                  |2008 R2 和較早的 CAL|2012 CAL|2016 CAL|2019 的 CAL|
|---------------------------------|--------|--------|--------|--------|
| **2008、 2008 R2 授權伺服器**| 是    | 否     | 否     | 否     |
| **2012 授權伺服器**         | 是    | 是    | 否     | 否     |
| **2012 R2 授權伺服器**      | 是    | 是    | 否     | 否     |
| **2016 授權伺服器**         | 是    | 是    | 是    | 否     |
| **2019 授權伺服器**         | 是    | 是    | 是    | 是    |

任何 RDS 授權伺服器可以裝載來自所有舊版的遠端桌面服務 」 和 「 遠端桌面服務的目前版本的授權。 比方說，Windows Server 2016 RDS 授權伺服器可以裝載來自所有舊版的 RDS，授權，而 Windows Server 2012 R2 RDS 授權伺服器只能裝載至 Windows Server 2012 R2 的授權。

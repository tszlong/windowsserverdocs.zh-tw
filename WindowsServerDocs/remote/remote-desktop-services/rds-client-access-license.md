---
title: 使用用戶端存取使用權 (CAL) 授權您的 RDS 部署
description: 在遠端桌面服務中授權的用戶端概觀。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 02/12/2020
manager: dongill
ms.openlocfilehash: a11820b9c75bbcb928da562f3f74e4130e9c8096
ms.sourcegitcommit: 599162b515c50106fd910f5c180e1a30bbc389b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775316"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>使用用戶端存取使用權 (CAL) 授權您的 RDS 部署

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

每個連線到遠端桌面工作階段主機的使用者和裝置都需要用戶端存取使用權 (CAL)。 您使用 RD 授權來安裝、發出及追蹤 RDS CAL。  

當使用者或裝置連線到 RD 工作階段主機伺服器時，RD 工作階段主機伺服器會判斷是否需要 RDS CAL。 接著，RD 工作階段主機伺服器會向遠端桌面授權伺服器要求 RDS CAL。 如果授權伺服器可提供適當的 RDS CAL，就會發出 RDS CAL 給用戶端，用戶端即可連線到 RD 工作階段主機伺服器，並從該伺服器連線到其嘗試使用的桌面或應用程式。

有 180 天的授權寬限期，在此期間不需要授權伺服器。 寬限期結束之後，用戶端必須具備授權伺服器所發出的有效 RDS CAL，才能登入 RD 工作階段主機伺服器。

使用下列資訊以了解用戶端存取使用權在遠端桌面服務的運作方式，並部署和管理您的授權：

- [使用用戶端存取使用權 (CAL) 授權您的 RDS 部署](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [了解 RDS CAL 模型](#understanding-the-rds-cal-model)
  - [RDS CAL 版本相容性](#rds-cal-version-compatibility)

## <a name="understanding-the-rds-cal-model"></a>了解 RDS CAL 模型

有兩種類型的 RDS CAL：

- RDS 每一裝置的 CAL
- RDS 每位使用者的 CAL

下表概述兩種 CAL 類型之間的差異：

| 每一裝置                                                     | 每位使用者                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| RDS CAL 會以實體方式指派給每部裝置。                   | RDS CAL 會指派給 Active Directory 中的使用者。                                 |
| RDS CAL 會由授權伺服器追蹤。                        | RDS CAL 會由授權伺服器追蹤。                                          |
| 不論是否具有 Active Directory 成員資格，都可以追蹤 RDS CAL。 | 無法在工作群組內追蹤 RDS CAL。                                       |
| 您可以撤銷最多 20% 的 RDS CAL。                              | 您無法撤銷任何 RDS CAL。                                                      |
| 臨時的 RDS CAL 有效期為 52-89 天。                       | 無法使用臨時的 RDS CAL。                                                |
| RDS CAL 無法過度配置。                                  | RDS CAL 可以過度配置 (違反遠端桌面授權合約)。 |

當您使用每一裝置模型時，臨時的授權會在裝置第一次連線到 RD 工作階段主機時發出。 該裝置第二次連線時，只要授權伺服器已啟用且有可用的 RDS CAL，授權伺服器即會發出永久性 RDS 每一裝置的 CAL。

使用每位使用者模型時不會施行授權，且每位使用者都會獲得授權，可從任何數量的裝置連線到 RD 工作階段主機。 授權伺服器會從可用 RDS CAL 集區或超量使用的 RDS CAL 集區發出授權。 您必須負責確保所有使用者都具備有效的授權，且不超量使用 CAL，否則即會違反遠端桌面服務授權條款。

其中一個範例是使用「每一裝置」模型的環境中，有兩個以上的轉移使用同一部電腦來存取 RD 工作階段主機。 「每個使用者」模型最適合用於使用者擁有專屬 Windows 裝置來存取 RD 工作階段主機的環境。

若要確保遵循遠端桌面服務授權條款，請追蹤組織中所使用 RDS 每位使用者的 CAL，並確定在授權伺服器上為所有使用者安裝足夠的 RDS 每位使用者的 CAL。

您可以使用遠端桌面授權管理員來追蹤 RDS 每位使用者的 CAL 並產生報告。

## <a name="rds-cal-version-compatibility"></a>RDS CAL 版本相容性

由使用者或裝置使用的 RDS CAL 必須對應於使用者或裝置所連線 Windows Server 版本。 您無法使用舊版 RDS CAL 來存取較新版的 Windows Server，但可以使用較新版的 RDS CAL 來存取舊版的 Windows Server。 例如，必須要有 RDS 2016 CAL 或更新版本，才能連接到 Windows Server 2016 RD 工作階段主機，而 RDS 2012 CAL 或更新版本則需要連接到 Windows Server 2012 R2 RD 工作階段主機。

下表顯示哪些 RDS CAL 和 RD 工作階段主機版本彼此相容。

|                  | RDS 2008 R2 和較舊的 CAL | RDS 2012 CAL | RDS 2016 CAL | RDS 2019 CAL |
|---------------------------------|--------|--------|--------|--------|
| **2008、2008 R2 工作階段主機** | 是    | 是    | 是    | 是     |
| **2012 工作階段主機**         | 否     | 是    | 是    | 是    |
| **2012 R2 工作階段主機**      | 否     | 是    | 是    | 是    |
| **2016 工作階段主機**         | 否     | 否     | 是    | 是    |
| **2019 工作階段主機**         | 否     | 否     | 否     | 是    |

您必須在相容的 RD 授權伺服器上安裝 RDS CAL。 任何 RDS 授權伺服器都可以裝載所有舊版遠端桌面服務的授權，以及最新版遠端桌面服務的授權。 例如，Windows Server 2016 RDS 授權伺服器可以裝載所有舊版 RDS 的授權，而 Windows Server 2012 R2 RDS 授權伺服器只能裝載最高到 Windows Server 2012 R2 的授權。

下表顯示哪些 RDS CAL 和授權伺服器版本彼此相容。

|                  | RDS 2008 R2 和較舊的 CAL | RDS 2012 CAL | RDS 2016 CAL | RDS 2019 CAL |
|---------------------------------|--------|--------|--------|--------|
| **2008、2008 R2 授權伺服器** | 是    | 否   | 否   | 否    |
| **2012 授權伺服器**         | 是     | 是    | 否   | 否    |
| **2012 R2 授權伺服器**      | 是     | 是    | 否   | 否    |
| **2016 授權伺服器**         | 是     | 是    | 是   | 否    |
| **2019 授權伺服器**         | 是     | 是    | 是  | 是   |

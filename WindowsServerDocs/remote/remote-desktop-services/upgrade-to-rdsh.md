---
title: 將遠端桌面工作階段主機升級至 Windows Server 2016
description: 了解如何將現有的遠端桌面工作階段主機升級至 Windows Server 2016。
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 37aefa57473b9fe8d16fc399ad9bdc2e2e5d53d0
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965810"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>將遠端桌面工作階段主機升級至 Windows Server 2016

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

> [!IMPORTANT]
> 在升級之前必須將所有應用程式解除安裝，並且在升級後重新安裝，以避免因升級而產生任何應用程式相容性問題。

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色且受支援的作業系統升級
目前僅支援從 Windows Server 2012 R2 和 Windows Server 2016 TP5 升級至 Windows Server 2016。

## <a name="upgrading-a-rds-session-based-collection"></a>升級以 RDS 工作階段為基礎的集合
為了盡可能縮短停機時間，升級以 RDS 工作階段為基礎的集合時，最好遵循下列步驟：

1. 識別要升級的伺服器，例如集合中一半的伺服器。
2. 將 [允許新連線]  設為 false，以防止對這些伺服器建立新連線。
3. 登出這些伺服器上的所有工作階段。
4. 從集合中移除這些伺服器。
5. 將伺服器升級至 Windows Server 2016。
6. 在集合中的其餘伺服器上，將 [允許新連線]  設為 "false"。
7. 將升級的伺服器重新新增至其對應的集合。
8. 從集合中移除要升級的其餘伺服器。
9. 在集合中已升級的伺服器上，將 [允許新連線]  設為 "true"。
10. 接著，依照上述的步驟 3 到 9 升級部署中的其餘伺服器。

## <a name="upgrading-a-standalone-rd-session-host-server"></a>升級獨立 RD 工作階段主機伺服器
獨立 RD 工作階段主機伺服器可以隨時升級。
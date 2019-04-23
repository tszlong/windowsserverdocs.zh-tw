---
title: 升級至 Windows Server 2016 遠端桌面工作階段主機
description: 本文說明如何升級現有的遠端桌面服務部署至 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856059"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>升級至 Windows Server 2016 遠端桌面工作階段主機

>適用於：Windows Server （半年通道），Windows Server 2016

> [!IMPORTANT]
> 所有應用程式必須在升級之前解除安裝和升級，以避免任何可能會提高，因為升級的應用程式相容性問題之後重新安裝。

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色支援 OS 升級
只能從 Windows Server 2012 R2 和 Windows Server 2016 TP5 支援至 Windows Server 2016 的升級。

## <a name="upgrading-a-rds-session-based-collection"></a>升級與 RDS 工作階段為基礎的集合
為了讓停機時間保持在最小值，最好是升級與 RDS 工作階段為基礎的集合時，遵循以下步驟：

1. 識別要升級的伺服器，一半的伺服器集合中。
2. 設定以避免新的連線到這些伺服器**允許新連線**設為 false。
3. 登出這些伺服器上的所有工作階段。 
4. 從集合中移除這些伺服器。
5. 升級至 Windows Server 2016 的伺服器。
6. 設定**允許新連線**為 「 false 」 集合中的其他伺服器上。
7. 將升級的伺服器加回其對應的集合。
8. 移除剩餘的一組伺服器要從集合升級。
9. 設定**允許新連線**為"true"，在集合中升級的伺服器上。
10. 現在遵循下列步驟 3 到 9 上述升級部署中的其他伺服器。

## <a name="upgrading-a-standalone-rd-session-host-server"></a>升級獨立 RD 工作階段主機伺服器
獨立 RD 工作階段主機伺服器可以隨時升級。
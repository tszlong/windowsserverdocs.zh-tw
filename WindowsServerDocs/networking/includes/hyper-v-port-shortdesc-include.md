---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 761deb136ebd4ec22dfeebc47b4eeb7650594d89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863709"
---
使用 HYPER-V 通訊埠，在 HYPER-V 主機上設定的 NIC 小組會提供獨立 MAC 位址的 Vm。  Vm MAC 位址或移植 VM 連線到 HYPER-V 交換器，可用來將 NIC 小組成員之間的網路流量。 您無法設定 NIC 小組與 HYPER-V 連接埠負載平衡模式內 Vm 所建立。 相反地，使用位址雜湊模式。 
---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: 43a4149d44bd24a7d8b3d68ed9e3c2b68c99e285
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952821"
---
使用 Hyper-v 通訊埠，在 Hyper-v 主機上設定的 NIC 小組會提供 Vm 獨立的 MAC 位址。  Vm 的 MAC 位址或已連線到 Hyper-v 交換器的 VM 會用來分割 NIC 小組成員之間的網路流量。 您無法使用 Hyper-v 埠負載平衡模式來設定在 Vm 內建立的 NIC 小組。 相反地，請使用位址雜湊模式。
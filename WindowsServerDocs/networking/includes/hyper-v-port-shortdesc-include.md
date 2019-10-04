---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935054"
---
使用 Hyper-v 通訊埠，在 Hyper-v 主機上設定的 NIC 小組會提供 Vm 獨立的 MAC 位址。  Vm 的 MAC 位址或已連線到 Hyper-v 交換器的 VM 會用來分割 NIC 小組成員之間的網路流量。 您無法使用 Hyper-v 埠負載平衡模式來設定在 Vm 內建立的 NIC 小組。 相反地，請使用位址雜湊模式。 
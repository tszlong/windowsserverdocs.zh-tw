---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 47a91c86ac75aedf532289055c94b34899fa01df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316512"
---
使用 Hyper-v 通訊埠，在 Hyper-v 主機上設定的 NIC 小組會提供 Vm 獨立的 MAC 位址。  Vm 的 MAC 位址或已連線到 Hyper-v 交換器的 VM 會用來分割 NIC 小組成員之間的網路流量。 您無法使用 Hyper-v 埠負載平衡模式來設定在 Vm 內建立的 NIC 小組。 相反地，請使用位址雜湊模式。 
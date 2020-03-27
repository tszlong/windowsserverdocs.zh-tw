---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: f2065acb89af4bed4dc525453bb5a294a4e2c3ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316479"
---
使用動態時，輸出負載是根據 TCP 通訊埠和 IP 位址的雜湊來散發。 動態模式也會即時重新平衡載入，讓指定的輸出流程可以在小組成員之間來回移動。 相反地，輸入負載會以與 Hyper-v 埠相同的方式進行散發。 簡言之，動態模式會利用位址雜湊和 Hyper-v 埠的最佳層面，而且是最高效能的負載平衡模式。 


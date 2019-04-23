---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879849"
---
待命配接器的選項都會**無 （所有介面卡使用）** 或您選擇的 NIC 小組，做為待命配接器中的特定網路介面卡。 當您設定 NIC 為待命配接器時，所有其他的未選取的小組成員都是作用中，並沒有網路流量傳送至或配接器所處理，直到作用中的 NIC 失敗。 作用中的 NIC 失敗之後，變成作用中待命 NIC，並處理網路流量。 當所有小組成員取得都還原到服務時，待命的小組成員就會回到待命狀態。  
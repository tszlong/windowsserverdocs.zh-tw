---
title: 'Pktmon support for Wireshark (pcapng) '
description: 使用本主題以 Wireshark 分析封包監視器產生的 pcapng 記錄。
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: c65604f6e3f4e87b806513063800ee62d52d1feb
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632443"
---
# <a name="pktmon-support-for-wireshark-pcapng"></a>Pktmon support for Wireshark (pcapng) 

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視器 (Pktmon) 可將記錄轉換成 pcapng 格式。 您可以使用 Wireshark (或任何 pcapng analyzer) 來分析這些記錄;不過，pcapng 檔中可能會遺漏某些重要資訊。 本主題說明預期的輸出，以及如何利用它。

## <a name="pktmon-pcapng-syntax"></a>Pktmon pcapng 語法

使用下列命令將 pktmon capture 轉換成 pcapng 格式。

```powershell
C:\Test> pktmon pcapng help
pktmon pcapng log.etl [-o log.pcapng]
    Convert log file to pcapng format.
    Dropped packets are not included by default.

-o, --out
    Name of the formatted pcapng file.

-d, --drop-only
    Convert dropped packets only.

-c, --component-id
    Filter packets by a specific component ID.

Example: pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

## <a name="output-filtering"></a>輸出篩選

所有關于封包捨棄報告的資訊，以及透過網路堆疊的封包流程，都會在 pcapng 輸出中遺失。 因此，您應該仔細預先篩選記錄內容以進行這類轉換。 例如：

- Pcapng 格式無法分辨流動的封包和捨棄的封包。 若要將捕獲中的所有封包與捨棄的封包分開，請產生兩個 pcapng 檔案;其中一個包含所有封包 ( " **pktmon pcapng log .etl--out log-capture .etl** " ) ，另一個只包含捨棄的封包 ( " **pktmon pcapng log. etl--drop-out log-drop .etl** " ) 。 如此一來，您就可以在個別的記錄檔中分析捨棄的封包。
- Pcapng 格式無法區別已捕捉到封包的不同網路元件。 針對這類多層式案例，請在 pcapng 輸出 "pktmon pcapng 記錄檔中指定所需的元件識別碼 **--元件識別碼 5** "。 針對您感興趣的每一組元件識別碼重複此命令。

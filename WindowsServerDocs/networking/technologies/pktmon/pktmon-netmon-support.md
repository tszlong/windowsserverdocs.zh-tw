---
title: Microsoft 網路監視器 (Netmon) 的 Pktmon 支援
description: 使用此頁面來分析 Netmon 內的封包監視器產生的 ETL 檔案。
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 7c6e3a40558ea27a273464d157fa4fbdbacd7134
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632446"
---
# <a name="pktmon-support-for-microsoft-network-monitor-netmon"></a>Microsoft 網路監視器 (Netmon) 的 Pktmon 支援

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視器 (Pktmon) 會以 ETL 格式產生記錄。 您可以使用特殊剖析器，利用 Microsoft 網路監視器 (Netmon) 來分析這些記錄。 本主題說明如何在 Netmon 內分析封包監視器產生的 ETL 檔案。

## <a name="network-monitor-setup-and-configuration"></a>網路監視器設定和設定

遵循下列步驟來安裝和設定 Netmon，以剖析封包監視器產生的 ETL 檔案：

   1. [安裝網路監視器 3.4](/download/4865)。
   1. 啟動網路監視器提升許可權，並在 (工具/選項/剖析器設定檔) 上將 Windows 設定為使用中剖析器設定檔。
   1. 將 etl_Microsoft-Windows-PktMon-Events npl 從 [這裡](https://github.com/microsoft/NetMon_Parsers_for_PacketMon/blob/main/etl_Microsoft-Windows-PktMon-Events.npl)複製   到 "%PROGRAMDATA%\Microsoft\Network Monitor 3 \ NPL\NetworkMonitor Parsers\Windows"
   1. 將 stub_etl_Microsoft-Windows-PktMon-Events npl 從 [這裡](https://github.com/microsoft/NetMon_Parsers_for_PacketMon/blob/main/stub_etl_Microsoft-Windows-PktMon-Events.npl) 複製到 "%PROGRAMDATA%\Microsoft\Network Monitor 3 \ NPL\NetworkMonitor Parsers\Windows\Stubs"
   1. 將 stub_etl_Microsoft-Windows-PktMon-Events. npl 重新命名為 etl_Microsoft-Windows-PktMon-Events. npl
   1. 將 etl_Microsoft-Windows-PktMon-Events npl 到 NetworkMonitor_Parsers_sparser. npl at "%PROGRAMDATA%\Microsoft\Network Monitor 3 \ NPL\NetworkMonitor 剖析器"
   1. 重新開機網路監視器提升為重建剖析器的許可權。


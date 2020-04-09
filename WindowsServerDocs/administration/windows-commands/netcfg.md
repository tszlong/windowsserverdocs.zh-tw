---
title: netcfg
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4895928ffdd5d923d370f82e699d69f42c0f81a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838931"
---
# <a name="netcfg"></a>netcfg

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會安裝 Windows 預先安裝環境（WinPE），這是用來部署工作站的輕量版本 Windows。
## <a name="syntax"></a>語法
```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/v|以**詳細**資訊（詳細）模式執行|
|/e|在安裝和卸載期間使用服務**環境**變數|
|/winpe|安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境（WinPE）|
|/l|提供 INF 的**位置**|
|/c|提供要安裝之元件的**類別**;通訊協定、服務或用戶端|
|/i|提供元件**識別碼**|
|/s|提供要**顯示**的元件類型。<p>\ta = 介面卡，n = net 元件|
|/b|顯示系結**路徑**，後面接著包含路徑名稱的字串。|
|/?|在命令提示字元**中顯示說明**。|

## <a name="examples"></a><a name=BKMK_Examples></a>典型

若要使用 c:\oemdir\example.inf 安裝通訊協定*範例*：
```
netcfg /l c:\oemdir\example.inf /c p /i example
```
若要安裝*MS_Server*服務：
```
netcfg /c s /i MS_Server
```
安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境
```
netcfg /v /winpe
```
若要顯示是否已安裝元件*MS_IPX* ：
```
netcfg /q MS_IPX
```
若要卸載元件*MS_IPX*：
```
netcfg /u MS_IPX
```
若要顯示所有已安裝的 net 元件：
```
netcfg /s n
```
若要顯示包含*MS_TCPIP*的系結路徑：
```
netcfg /b ms_tcpip
```
## <a name="additional-references"></a>其他參考資料
-   - [命令列語法關鍵](command-line-syntax-key.md)

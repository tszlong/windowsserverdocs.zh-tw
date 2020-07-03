---
title: netcfg
description: Netcfg 命令的參考文章，它會安裝 Windows 預先安裝環境（WinPE），這是用來部署工作站的輕量版本 Windows。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f9ed2dde5d85be5432fb7b3af8279b2e71e9db0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934815"
---
# <a name="netcfg"></a>netcfg

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會安裝 Windows 預先安裝環境（WinPE），這是用來部署工作站的輕量版本 Windows。

## <a name="syntax"></a>語法

```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /v | 以詳細（詳細）模式執行。 |
| /e | 在安裝和卸載期間使用服務環境變數。 |
| /winpe | 安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境（WinPE）。 |
| /l | 提供 INF 檔案的位置。 |
| /C | 提供要安裝之元件的類別;**通訊協定**、**服務**或**用戶端**。 |
| /i | 提供元件識別碼。 |
| /s | 提供要顯示的元件類型，包括介面卡的**\ta**或 net 元件的**n** 。 |
| /b | 顯示系結路徑，後面接著包含路徑名稱的字串。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要使用 c:\oemdir\example.inf 安裝通訊協定*範例*，請輸入：

```
netcfg /l c:\oemdir\example.inf /c p /i example
```

若要安裝*MS_Server*服務，請輸入：

```
netcfg /c s /i MS_Server
```

若要安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境，請輸入：

```
netcfg /v /winpe
```

若要顯示是否已安裝元件*MS_IPX* ，請輸入：

```
netcfg /q MS_IPX
```

若要卸載元件*MS_IPX*，請輸入：

```
netcfg /u MS_IPX
```

若要顯示所有已安裝的 net 元件，請輸入：

```
netcfg /s n
```

若要顯示包含*MS_TCPIP*的系結路徑，請輸入：

```
netcfg /b ms_tcpip
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

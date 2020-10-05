---
title: tapicfg
description: Tapicfg 命令的參考文章，可建立、移除或顯示 TAPI 應用程式目錄分割，或設定預設的 TAPI 應用程式目錄磁碟分割。
ms.topic: reference
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 17b596036251d6ea8588de3b70359ea161b67c58
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718125"
---
# <a name="tapicfg"></a>tapicfg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立、移除或顯示 TAPI 應用程式目錄分割，或設定預設的 TAPI 應用程式目錄磁碟分割。 TAPI 3.1 用戶端可以使用此應用程式目錄分割中的資訊與目錄服務定位器服務，來尋找和通訊 TAPI 目錄。 您也可以使用 **tapicfg** 來建立或移除服務連接點，讓 tapi 用戶端有效率地找出網域中的 tapi 應用程式目錄分割。

此命令列工具可在任何屬於網域成員的電腦上執行。

## <a name="syntax"></a>語法

```
tapicfg install
tapicfg remove
tapicfg publishscp
tapicfg removescp
tapicfg show
tapicfg makedefault
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| [tapicfg 安裝](tapicfg-install.md) | 建立 TAPI 應用程式目錄分割。 |
| [tapicfg 移除](tapicfg-remove.md) | 移除 TAPI 應用程式目錄分割。|
| [tapicfg publishscp](tapicfg-publishscp.md) | 建立用來發佈 TAPI 應用程式目錄分割的服務連接點。 |
| [tapicfg removescp](tapicfg-removescp.md) | 移除 TAPI 應用程式目錄磁碟分割的服務連接點。 |
| [tapicfg show](tapicfg-show.md) | 顯示網域中 TAPI 應用程式目錄分割的名稱和位置。 |
| [tapicfg makedefault](tapicfg-makedefault.md) | 設定網域的預設 TAPI 應用程式目錄分割。 |

#### <a name="remarks"></a>備註

- 您必須是 Active Directory 中 **Enterprise Admins** 群組的成員，才能執行 **tapicfg install** (來建立 tapi 應用程式目錄分割) 或 **tapicfg 移除** (移除 tapi 應用程式目錄分割) 。

- 使用者提供的文字 (例如，使用國際或 Unicode 字元) 的 TAPI 應用程式目錄磁碟分割、伺服器和網域的名稱，只有在已安裝適當的字型和語言支援時，才會正確顯示。

- 您仍然可以在組織中使用網際網路定位器服務 (ILS) 伺服器，如果需要使用 ILS 來支援特定的應用程式，因為執行 Windows XP 或 Windows Server 2003 作業系統的 TAPI 用戶端可以查詢 ILS 伺服器或 TAPI 應用程式目錄磁碟分割。

- 您可以使用 **tapicfg** 來建立或移除服務連接點。 如果 TAPI 應用程式目錄分割因為任何原因而重新命名 (例如，如果您重新命名所在的網域) ，您必須移除現有的服務連接點，並建立新的服務連接點，其中包含要發佈之 TAPI 應用程式目錄分割的新 DNS 名稱。 否則，TAPI 用戶端找不到並無法存取 TAPI 應用程式目錄分割。 您也可以移除服務連接點以進行維護或安全性用途 (例如，如果您不想要在特定的 TAPI 應用程式目錄磁碟分割上公開 TAPI 資料) 。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [tapicfg 安裝](tapicfg-install.md)

- [tapicfg 移除](tapicfg-remove.md)

- [tapicfg publishscp](tapicfg-publishscp.md)

- [tapicfg removescp](tapicfg-removescp.md)

- [tapicfg show](tapicfg-show.md)

- [tapicfg makedefault](tapicfg-makedefault.md)

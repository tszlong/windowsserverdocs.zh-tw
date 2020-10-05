---
title: tapicfg show
description: Tapicfg show 命令的參考文章，此命令會顯示網域中 TAPI 應用程式目錄分割的名稱和位置。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/29/2020
ms.openlocfilehash: b39021a1e7c76ee8db359b33f032bac11225e85c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730046"
---
# <a name="tapicfg-show"></a>tapicfg show

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示網域中 TAPI 應用程式目錄分割的名稱和位置。

## <a name="syntax"></a>語法

```
tapicfg show [/defaultonly] [/domain:<domainname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 僅限/default | 只顯示網域中預設 TAPI 應用程式目錄分割的名稱和位置。 |
| /domain `<domainname>` | 指定要顯示 TAPI 應用程式目錄分割之網域的 DNS 名稱。 如果未指定功能變數名稱，則會使用本機網域的名稱。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令列工具可在任何屬於網域成員的電腦上執行。

- 使用者提供的文字 (例如，使用國際或 Unicode 字元) 的 TAPI 應用程式目錄磁碟分割、伺服器和網域的名稱，只有在已安裝適當的字型和語言支援時，才會正確顯示。

- 您仍然可以在組織中使用網際網路定位器服務 (ILS) 伺服器，如果需要使用 ILS 來支援特定的應用程式，因為執行 Windows XP 或 Windows Server 2003 作業系統的 TAPI 用戶端可以查詢 ILS 伺服器或 TAPI 應用程式目錄磁碟分割。

- 您可以使用 **tapicfg** 來建立或移除服務連接點。 如果 TAPI 應用程式目錄分割因為任何原因而重新命名 (例如，如果您重新命名所在的網域) ，您必須移除現有的服務連接點，並建立新的服務連接點，其中包含要發佈之 TAPI 應用程式目錄分割的新 DNS 名稱。 否則，TAPI 用戶端找不到並無法存取 TAPI 應用程式目錄分割。 您也可以移除服務連接點以進行維護或安全性用途 (例如，如果您不想要在特定的 TAPI 應用程式目錄磁碟分割上公開 TAPI 資料) 。

## <a name="example"></a>範例

若要顯示新網域的預設 TAPI 應用程式目錄磁碟分割名稱，請輸入：

```
tapicfg show /defaultonly
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [tapicfg 安裝](tapicfg-install.md)

- [tapicfg 移除](tapicfg-remove.md)

- [tapicfg publishscp](tapicfg-publishscp.md)

- [tapicfg removescp](tapicfg-removescp.md)

- [tapicfg makedefault](tapicfg-makedefault.md)

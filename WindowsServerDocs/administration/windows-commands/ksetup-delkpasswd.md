---
title: ksetup： delkpasswd
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dce7d9666040ff0c234139932ea60e3589dfecb2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375129"
---
# <a name="ksetupdelkpasswd"></a>ksetup： delkpasswd

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除領域的 Kerberos 密碼伺服器（Kpasswd）。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
## <a name="syntax"></a>語法
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>參數

|   參數   |                                                                                                   描述                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或領域 =。                                |
| <KpasswdName> | 作為 Kerberos 密碼伺服器使用的 KDC 名稱會被視為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。 |

## <a name="remarks"></a>備註
執行命令**ksetup**來驗證 KDC 名稱。 如果**kpasswd =** 未出現在輸出中，表示尚未設定對應。 將會列出多個對應（如果已設定）。
## <a name="BKMK_Examples"></a>典型
確認領域 CORP。CONTOSO.COM 使用非 Windows KDC 伺服器 mitkdc.contoso.com 作為密碼伺服器：
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
若要確認命令是否如預期運作，請在 Windows 電腦上執行**ksetup**以確認領域 CORP。CONTOSO.COM 未對應到 Kerberos 密碼伺服器（KDC 名稱）。
## <a name="additional-references"></a>其他參考
-   [ksetup](ksetup.md)
-   [ksetup： delkpasswd](ksetup-delkpasswd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)

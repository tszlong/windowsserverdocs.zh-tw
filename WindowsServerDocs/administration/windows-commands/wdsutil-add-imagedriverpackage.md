---
title: wdsutil add-imagedriverpackage
description: Wdsutil add imagedriverpackage 命令的參考文章，此命令會將驅動程式存放區中的驅動程式套件新增至伺服器上的現有開機映射。
ms.topic: reference
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5d441007f673a194799e245bb704d31add81a483
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524503"
---
# <a name="wdsutil-add-imagedriverpackage"></a>wdsutil add-imagedriverpackage

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式存放區中的驅動程式套件新增至伺服器上現有的開機映射。

## <a name="syntax"></a>語法

```
wdsutil /add-ImageDriverPackage [/Server:<Servername>] [media:<Imagename>] [mediatype:Boot] [/Architecture:{x86 | ia64 | x64}] [/Filename:<Filename>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| [媒體： `<Imagename>` ] | 指定要新增驅動程式的映射名稱。 |
| [媒體：開機] | 指定要新增驅動程式的映射類型。 驅動程式套件只能新增至開機映射。 |
| [/Architecture： `{x86 | ia64 | x64}` ] | 指定開機映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。 |
| [/Filename： `<Filename>` ] | 指定檔案名。 如果映射無法依名稱唯一識別，則必須指定檔案名。 |
| [/DriverPackage:`<Name>` | 指定要新增至映射之驅動程式套件的名稱。 |
| [/PackageId： `<ID>` ] | 指定驅動程式套件的 Windows 部署服務識別碼。 如果無法依名稱唯一識別驅動程式套件，則必須指定此選項。 若要尋找封裝識別碼，請選取封裝所在的驅動程式群組 (或 [ **所有套件** ] 節點) ，以滑鼠右鍵按一下封裝，然後選取 [ **屬性**]。 封裝識別碼會列在 [ **一般** ] 索引標籤上。例如： {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="examples"></a>範例

若要將驅動程式套件新增至開機映射，請輸入下列其中一項：

```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```

```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-imagedriverpackages 命令](wdsutil-add-imagedriverpackages.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)

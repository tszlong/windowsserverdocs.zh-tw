---
title: wdsutil add-drivergroup
description: Wdsutil add-drivergroup 命令的參考文章，此命令會將驅動程式群組新增至伺服器。
ms.topic: reference
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 68583ba35c141df03cbc5e351da6c4dc8ea4e161
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524613"
---
# <a name="wdsutil-add-drivergroup"></a>wdsutil add-drivergroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式群組新增至伺服器。

## <a name="syntax"></a>語法

```
wdsutil /add-DriverGroup /DriverGroup:<Groupname>\n\ [/Server:<Servername>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filtertype> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /DriverGroup:`<Groupname>` | 指定新驅動程式群組的名稱。 |
| 伺服器`<Servername>` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| 使`{Yes|No}` | 啟用或停用套件。 |
| 中期`{Matched|All}` | 指定符合篩選準則時要安裝的封裝。 [**符合**] 表示只安裝符合用戶端硬體的驅動程式套件。 **所有方法都** 是將所有套件安裝至用戶端，不論其硬體為何。 |
| Filtertype`<Filtertype>` | 指定要加入至群組之篩選的類型。 您可以在單一命令中指定多個篩選器類型。 每個篩選類型後面都必須接著 **/policy** 和至少一個 **/Value**。 有效值包括：<ul><li>BiosVendor</li><li>Biosversion</li><li>Chassistype</li><li>製造商</li><li>Uuid</li><li>Osversion</li><li>Osedition</li><li>OsLanguage</li></ul> 如需取得其他所有篩選器類型值的相關資訊，請參閱 [驅動程式群組篩選](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11))。 |
| [/Policy： `{Include|Exclude}` ] | 指定要在篩選準則上設定的原則。 如果 **/policy** 設定為 [ **包含**]，則會允許符合篩選準則的用戶端電腦安裝此群組中的驅動程式。 如果 **/policy** 設定為 [ **排除**]，則不允許符合篩選準則的用戶端電腦安裝此群組中的驅動程式。 |
| [/Value： `<Value>` ] | 指定對應至 **/Filtertype**的用戶端值。 您可以為單一類型指定多個值。 如需可接受之篩選類型值的相關資訊，請參閱 [驅動程式群組篩選](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11))。 |

## <a name="examples"></a>範例

若要新增驅動程式群組，請輸入下列其中一項：

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-drivergrouppackage 命令](wdsutil-add-drivergrouppackage.md)

- [wdsutil add-drivergrouppackages 命令](wdsutil-add-drivergrouppackages.md)

- [wdsutil add-drivergroupfilter 命令](wdsutil-add-drivergroupfilter.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)

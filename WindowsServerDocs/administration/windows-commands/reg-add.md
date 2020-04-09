---
title: reg 新增
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df59477c980169699dac897e36836e5226b6a0fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836591"
---
# <a name="reg-add"></a>reg 新增


將新的子機碼或專案新增至登錄。

## <a name="syntax"></a>語法

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

### <a name="parameters"></a>參數

|      參數      |                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式 \\\\\<ComputerName >\) 做為*KeyName*的一部分。 省略 \\\\ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
|   /v \<ValueName >   |                                                                                                                                                                                                                                指定要在指定的子機碼底下新增的登錄專案名稱。                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                指定新增至登錄的登錄專案具有 null 值。                                                                                                                                                                                                                                |
|     /t \<類型 >      |                                                                                                                                          指定登錄專案的類型。 *類型*必須是下列其中一項：</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<分隔符號 >   |                                                                                                                                                              指定當指定了 REG_MULTI_SZ 資料類型，而且需要列出多個專案時，要用來分隔多個資料實例的字元。 如果未指定，預設分隔符號為 **\ 0**。                                                                                                                                                              |
|     /d \<資料 >      |                                                                                                                                                                                                                                                 指定新登錄專案的資料。                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           新增登錄專案，而不提示確認。                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              在命令提示字元中顯示**reg add**的說明。                                                                                                                                                                                                                                               |

## <a name="remarks"></a>備註

-   無法加入此作業的子樹。 這個版本的**reg**不會在新增子機碼時要求確認。
-   下表列出**reg add**作業的傳回值。

| 值 | 描述 |
|-------|-------------|
|   0   |   成功   |
|   1   |   失敗   |

-   針對 REG_EXPAND_SZ 金鑰類型，請在/d 參數內搭配使用插入號（ **^** ）與 **%**

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要在遠端電腦 ABC 上新增金鑰 HKLM\Software\MyCo，請輸入：
```
REG ADD \\ABC\HKLM\Software\MyCo
```
若要將登錄專案新增至 HKLM\Software\MyCo，並將名稱為**data**的數值型別 REG_BINARY 和**fe340ead**的資料，請輸入：
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
若要將多值登錄專案新增至 HKLM\Software\MyCo，並將值名稱為**MRU**類型的 REG_MULTI_SZ 和**fax\0mail\0\0**的資料，請輸入：
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
若要將擴充的登錄專案新增至 HKLM\Software\MyCo，並將值名稱的**路徑**類型 REG_EXPAND_SZ 和 **% systemroot%** 的資料，請輸入：
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: 詳細資訊
description: 適用于詳細資訊的 Windows 命令主題，會顯示指定命令的詳細資訊輸出。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f67a4fe99a5051e8844e6386d87911946a808c1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832841"
---
# <a name="verbose"></a>詳細資訊

顯示指定命令的詳細資訊輸出。 您可以將 **/verbose**與您執行的任何其他 WDSUTIL 命令搭配使用。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>範例

若要從自動新增資料庫中刪除已核准的電腦，並顯示詳細資訊輸出，請輸入：
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```
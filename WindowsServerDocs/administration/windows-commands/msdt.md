---
title: msdt
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781e83615d44a4adb20fd85fe5ce6ebc7c2d4966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818829"
---
# <a name="msdt"></a>msdt



叫用的疑難排解套件，在命令列或自動化的指令碼，並可讓使用者輸入的情況下的其他選項。

## <a name="syntax"></a>語法

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>參數

下表包含支援 msdt.exe 選項與參數。

|參數|描述|
|---------|-----------|
|/id\<封裝名稱 >|指定要執行哪一個診斷套件。 取得一份可用的套件，請參閱疑難排解組件中的識別碼 可疑難排解套件嗎？ 本主題稍後的章節。|
|/path \<directory | .diagpkg 檔案 | .diagcfg 檔案 >|指定診斷封裝的完整路徑。 如果您指定的目錄，目錄必須包含診斷的封裝。 您無法使用 /path 參數搭配 */id*， */dci*，或 */cab*參數。|
|/dci \<passkey>|Prepopulates msdt 中的 [密碼] 欄位。 支援提供者所提供的密碼金鑰時，才使用這個參數。|
|/dt \<directory>|指定的目錄中顯示的疑難排解的記錄。 診斷的結果會儲存在使用者的 **%LOCALAPPDATA%\Diagnostics**或是 **%LOCALAPPDATA%\ElevatedDiagnostics**目錄。|
|/af\<回應檔案 >|指定包含一或多個診斷互動回應的 XML 格式的回應檔案。|

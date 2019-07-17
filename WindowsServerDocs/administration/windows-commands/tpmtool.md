---
title: tpmtool
description: Windows 命令主題 tpmtool-取得受信任的平台模組的相關資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230860"
---
# <a name="tpmtool"></a>tpmtool

此公用程式可用來取得相關資訊[信賴平台模組 (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview)。

>[!IMPORTANT]
>預先發行的產品在正式發行前可能會大幅度修改，本文提供有關該產品的一些資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

如需如何使用此命令的範例，請參閱[範例](#tpmtool_examples)。

## <a name="syntax"></a>語法

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|getdeviceinformation|顯示的基本資訊的 TPM。 您可以找到資訊旗標值的意義[此處](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters)。|
|gatherlogs [輸出目錄路徑]|會收集 TPM 的記錄檔，並將它們放在指定的目錄。 如果該目錄不存在，它會建立它。 根據預設，它們會放在目前的目錄。 可能產生的檔案如下： </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [開始] / [停止]|啟動/停止收集 TPM 驅動程式追蹤。 將產生追蹤記錄檔，TPMTRACE.etl，，並將它放在目前的目錄中。|
|parsetcglogs [-驗證 (-v)]|顯示已剖析的 TCG 記錄檔，也就是 Windows 開機組態記錄檔 (WBCL)。 最新的事件描述可於[TCG 網站](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/)下方**事件描述**。 如果`-validate`旗標設定，驗證在 TPM 上的平台設定暫存器 (PCR) 值相符的記錄檔中的值。|
|/?|在命令提示字元顯示說明。|

## <a name="tpmtool_examples"></a>範例

若要顯示的 TPM 的基本資訊，請輸入：
```
tpmtool getdeviceinformation
```
若要收集 TPM 的記錄檔，並將它們放在目前的目錄中，輸入：
```
tpmtool gatherlogs
```
若要收集 TPM 的記錄檔，並將它們放入`C:\Users\Public`，型別：
```
tpmtool gatherlogs C:\Users\Public
```
若要收集 TPM 驅動程式追蹤，請輸入：
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
若要剖析的 TCG 記錄檔：
```
tpmtool parsetcglogs
```
若要剖析的 TCG 記錄檔，並驗證 PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>解碼錯誤代碼

TPM 特有的錯誤碼[此處](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6)。

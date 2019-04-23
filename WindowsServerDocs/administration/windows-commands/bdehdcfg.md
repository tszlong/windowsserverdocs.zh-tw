---
title: bdehdcfg
description: 適用於 Windows 命令主題**bdehdcfg** -準備硬碟使用 BitLocker 磁碟機加密所需的資料分割。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869699"
---
# <a name="bdehdcfg"></a>bdehdcfg



準備硬碟使用 BitLocker 磁碟機加密所需的資料分割。 大部分的 Windows 7 的安裝將不需要使用此工具，因為 BitLocker 安裝程式包含準備和重新分割所需的磁碟機的能力。

> [!WARNING]
> 使用已知的衝突**拒絕未受 bitlocker 保護的固定式磁碟機的寫入權限** 群組原則設定位於**電腦設定 \ 系統管理範本 \windows 元件 \bitlocker磁碟機加密資料磁碟機**。</br>> 如果 Bdehdcfg 執行的電腦上啟用此原則設定時，您可能會遇到下列問題：</br>>-如果您嘗試將壓縮的磁碟機，並建立系統磁碟機中，將會成功地減少磁碟機大小，並建立原始的分割區。 不過，原始的分割區不會格式化。 會顯示下列錯誤訊息：「 無法格式化新的作用中磁碟機。 您可能需要以手動方式為 BitLocker 準備您的磁碟機。 」</br>>-如果您嘗試使用未配置的空間來建立系統磁碟機，就會建立原始的分割區。 不過，原始的分割區不會格式化。 會顯示下列錯誤訊息：「 無法格式化新的作用中磁碟機。 您可能需要以手動方式為 BitLocker 準備您的磁碟機。 」</br>>-如果您嘗試合併現有的磁碟機的系統磁碟機，工具將無法複製到目標磁碟機建立系統磁碟機上的所需的開機檔案。 會顯示下列錯誤訊息：「 BitLocker 安裝程式無法將開機檔案複製。 您可能需要以手動方式為 BitLocker 準備您的磁碟機。 」</br>> 如果此原則設定會強制執行，因為受保護的磁碟機，不能重新分割硬碟機。 如果您貴組織的舊版 Windows 升級電腦，而且這些電腦已設定的單一資料分割，您應該建立必要的 BitLocker 系統磁碟分割，再將原則設定套用到電腦。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|顯示在指定的磁碟機上的磁碟機代號、 總大小、 最大的可用空間和一個分割區的磁碟分割特性。 列出有效的磁碟分割。 四個主要或延伸磁碟分割存在時，不會列出未配置的空間。|
|[Bdehdcfg： 目標](bdehdcfg-target.md)|定義哪一部分將作為系統磁碟機的磁碟機，並讓部分成為使用中。|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|將新的磁碟機代號指派給 做為系統磁碟機的磁碟機的部分。|
|[bdehdcfg： 大小](bdehdcfg-size.md)|當正在建立新的系統磁碟機，請決定系統磁碟分割的大小。|
|[Bdehdcfg： 無訊息](bdehdcfg-quiet.md)|會防止顯示所有動作和命令列介面中的錯誤，並指示使用任何 [是] 的 [是] 答案 Bdehdcfg/後續期間可能發生的任何提示磁碟機準備。|
|[Bdehdcfg： 重新啟動](bdehdcfg-restart.md)|電腦重新開機的磁碟機準備完成之後，會指示。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="BKMK_Examples"></a>範例

下列範例會說明 Bdehdcfg 建立系統磁碟分割為 500 MB 的預設磁碟機搭配使用。 因為未不指定任何磁碟機代號，則新的系統磁碟分割不會有磁碟機代號。
```
bdehdcfg -target default -size 500
```
下列範例會說明 Bdehdcfg 的預設磁碟機與用來建立一個系統分割區 (P)預設大小的 300 mb 的磁碟機上的未配置空間不足。 此工具不會提示使用者輸入任何進一步的輸入也不會顯示任何錯誤。 建立系統磁碟機之後，會自動重新啟動電腦。
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
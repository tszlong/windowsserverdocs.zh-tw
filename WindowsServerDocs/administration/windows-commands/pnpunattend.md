---
title: pnpunattend
description: 瞭解如何在電腦上審核設備磁碟機，以及如何執行無提示驅動程式安裝。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 77a6ab1ea45322e3c53e8b095c412cf8838be60d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372266"
---
# <a name="pnpunattend"></a>pnpunattend

會針對設備磁碟機審核電腦，並執行自動驅動程式安裝，或在不安裝和的情況下搜尋驅動程式，並選擇性地將結果報告到命令列。 使用此命令來指定特定硬體裝置的特定驅動程式安裝。 請參閱＜備註＞。

## <a name="syntax"></a>語法

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|auditSystem|指定線上驅動程式安裝。</br>必要項，但使用 **/help**或 **/？** 來執行**pnpunattend**時除外 參數.|
|/s|選用。 指定不安裝就搜尋驅動程式。|
|/L|選用。 指定在命令提示字元中顯示此命令的記錄資訊。|
|/?|選用。 在命令提示字元中顯示此命令的說明。|

## <a name="remarks"></a>備註

需要初步準備。 使用此命令之前，您必須完成下列工作：

1. 為您要安裝的驅動程式建立目錄。 例如，在**C:\Drivers\Video**上建立用於視訊卡驅動程式的資料夾。
2. 下載並解壓縮您裝置的驅動程式套件。 將包含您作業系統版本之 INF 檔案的子資料夾內容，以及您所建立之 [video] 資料夾的任何子資料夾複製到其中。 例如，將影片驅動程式檔案複製到 C:\Drivers\Video。
3. 將系統內容路徑變數新增至您在步驟1中建立的資料夾。例如， **C:\Drivers\Video**。
4. 建立下列登錄機碼，然後針對您建立的**DriverPaths**金鑰，將**值資料**設定為**1**。
5. 若為 Windows®7，請流覽登錄路徑： **HKEY_LOCAL_Machine \Software\microsoft\windows server\ NT\CurrentVersion\\** ，然後建立金鑰： **UnattendSettings\PnPUnattend\DriverPaths\\**
6. 針對 Windows Vista，流覽至登錄路徑： **HK_LM \Software\microsoft\windows server\ NT\CurrentVersion\\** ，然後建立金鑰 = **\UnattendSettings\PnPUnattend\DriverPaths**。

## <a name="examples"></a>範例

下列範例命令顯示如何使用**PNPUnattend**來審查電腦是否有可能的驅動程式更新，然後將結果報告到命令提示字元。

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
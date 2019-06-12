---
title: pnpunattend
description: 了解如何稽核的裝置驅動程式的電腦上，以及執行無訊息的驅動程式安裝。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 53b72459d497ac5d079336c2a00ba65634b2e3a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436331"
---
# <a name="pnpunattend"></a>pnpunattend

稽核時，裝置驅動程式的電腦和執行自動安裝的驅動程式安裝，或搜尋驅動程式而不需要安裝並，選擇性地將結果回報給命令列。 若要指定特定硬體裝置的特定驅動程式的安裝中使用此命令。 請參閱＜備註＞。

## <a name="syntax"></a>語法

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|auditSystem|指定線上驅動程式安裝。</br>必要，除非**pnpunattend**與執行 **/help**或 **/？** 參數。|
|/s|選擇性。 指定要搜尋而不需要安裝的驅動程式。|
|/L|選擇性。 指定要在命令提示字元中顯示此命令的記錄資訊。|
|/?|選擇性。 此命令在命令提示字元中顯示說明。|

## <a name="remarks"></a>備註

需要初步的準備。 之前使用此命令時，您必須完成下列工作：

1. 建立您想要安裝的驅動程式的目錄。 例如，建立的資料夾**C:\Drivers\Video**視訊卡驅動程式。
2. 下載並解壓縮您的裝置驅動程式套件。 將包含您的作業系統版本的 INF 檔案的子資料夾和任何子資料夾的內容複製到您所建立的視訊資料夾中。 例如，視訊驅動程式將檔案複製到 C:\Drivers\Video。
3. 例如，在步驟 1.建立的資料夾中加入系統環境路徑變數**C:\Drivers\Video**。
4. 建立下列登錄機碼，然後針對**DriverPaths**設定在建立時，金鑰**數值資料**來**1**。
5. 針對 Windows® 7 瀏覽登錄路徑：**HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion\\** ，然後再建立索引鍵：**UnattendSettings\PnPUnattend\DriverPaths\\**
6. Windows vista 中，瀏覽至以下登錄路徑：**HK_LM\Software\Microsoft\Windows NT\CurrentVersion\\** ，然後再建立索引鍵 = **\UnattendSettings\PnPUnattend\DriverPaths**。

## <a name="examples"></a>範例

下列範例命令示範如何使用**PNPUnattend.exe**稽核可用的驅動程式更新的電腦，並接著報表 命令提示字元所發現的錯誤。

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
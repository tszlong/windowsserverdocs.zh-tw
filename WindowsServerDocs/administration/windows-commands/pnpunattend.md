---
title: pnpunattend
description: Pnpunattend 命令的參考文章，此命令會在電腦上審核設備磁碟機，以及執行無提示驅動程式安裝。
ms.topic: reference
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 6e3e0f0dfd1b689a62bf59956d3934e5ea74d177
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633672"
---
# <a name="pnpunattend"></a>pnpunattend

針對設備磁碟機審核電腦，並執行自動驅動程式安裝，或在不安裝的情況下搜尋驅動程式，並選擇性地將結果報告到命令列。 您可以使用此命令來指定特定硬體裝置的特定驅動程式安裝。

## <a name="prerequisites"></a>必要條件

較舊版本的 Windows 作業系統需要初步準備。 在使用此命令之前，您必須完成下列工作：

1. 為您要安裝的驅動程式建立目錄。 例如，在 **C:\Drivers\Video** 上建立視訊卡驅動程式的資料夾。

2. 下載並解壓縮您裝置的驅動程式套件。 將包含您的作業系統版本和任何子資料夾之 INF 檔案的子資料夾內容，複製到您所建立的 [video] 資料夾中。 例如，將視頻驅動程式檔案複製到 **C:\Drivers\Video**。

3. 將系統內容路徑變數新增至您在步驟1中建立的資料夾，例如 **C:\Drivers\Video**。

4. 建立下列登錄機碼，然後針對您所建立的 **DriverPaths** 金鑰，將 **值資料** 設定為 **1**。

5. 若為 Windows®7，請流覽登錄路徑： **HKEY_LOCAL_Machine \\ \software\microsoft\windows NT\CurrentVersion**，然後建立金鑰： **UnattendSettings\PnPUnattend\DriverPaths \\ **

## <a name="syntax"></a>語法

```
PnPUnattend.exe auditsystem [/help] [/?] [/h] [/s] [/l]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| auditsystem | 指定線上驅動程式安裝。<p>必要項，除非使用 **/help** 或/來執行此命令 **。** 參數。 |
| /s | 選擇性。 指定在不安裝的情況下搜尋驅動程式。 |
| /l | 選擇性。 指定要在命令提示字元中顯示此命令的記錄資訊。 |
| `/? | /help` | 選擇性。 在命令提示字元中顯示此命令的說明。 |

### <a name="examples"></a>範例

若要命令顯示如何使用 **PNPUnattend.exe** 來審核電腦是否有可能的驅動程式更新，然後將結果報告給命令提示字元，請輸入：

```
pnpunattend auditsystem /s /l
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

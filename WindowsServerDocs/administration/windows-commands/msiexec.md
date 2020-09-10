---
title: msiexec
description: Msiexec 命令的參考文章，提供從命令列安裝、修改和執行 Windows Installer 作業的方法。
ms.topic: reference
ms.assetid: 122eb0ce-ecbc-4909-a52a-15c3413619af
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 93ce1de1f75ff03bc7bb5f79d2046502c2d81bc4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639614"
---
# <a name="msiexec"></a>msiexec

提供從命令列安裝、修改和執行 Windows Installer 之作業的方法。

## <a name="install-options"></a>安裝選項

設定用來啟動安裝套件的安裝類型。

### <a name="syntax"></a>語法

```
msiexec.exe [/i][/a][/j{u|m|/g|/t}][/x] <path_to_package>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /i | 指定一般安裝。 |
| /a | 指定系統管理安裝。 |
| /ju | 將產品公告給目前的使用者。 |
| /jm | 將產品公告給所有使用者。 |
| /j/g | 指定已公告封裝所使用的語言識別項。 |
| /j/t | 將轉換套用至已公告的封裝。 |
| /x | 卸載封裝。 |
| `<path_to_package>` | 指定安裝套件檔案的位置和名稱。 |

#### <a name="examples"></a>範例

若要從 C：磁片磁碟機安裝名為 *example.msi* 的封裝，請使用一般安裝程式，輸入：

```
msiexec.exe /i "C:\example.msi"
```

## <a name="display-options"></a>顯示選項

您可以根據您的目標環境，設定使用者在安裝過程中看到的內容。 例如，如果您要將封裝散發到所有用戶端以進行手動安裝，則應該要有完整的 UI。 但是，如果您要使用群組原則部署套件，而這不需要使用者互動，就不應與 UI 相關。

### <a name="syntax"></a>語法

```
msiexec.exe /i <path_to_package> [/quiet][/passive][/q{n|b|r|f}]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| `<path_to_package>` | 指定安裝套件檔案的位置和名稱。 |
| /quiet | 指定無訊息模式，這表示不需要使用者互動。 |
| /passive | 指定自動模式，這表示安裝只會顯示進度列。 |
| /qn | 指定在安裝程式期間沒有任何 UI。 |
| /qn + | 指定在安裝程式期間沒有任何 UI，但結尾的最後一個對話方塊除外。 |
| /qb | 指定在安裝過程中有基本的 UI。 |
| /qb + | 指定在安裝程式期間有基本的 UI，包括結尾的最後一個對話方塊。 |
| /qr | 在安裝過程中指定精簡的 UI 體驗。 |
| /qf | 在安裝過程中指定完整的 UI 體驗。 |

##### <a name="remarks"></a>備註

- 如果使用者取消安裝，就不會顯示模式方塊。 您可以使用 **qb +！** 或 **qb！ +** 隱藏 [ **取消** ] 按鈕。

#### <a name="examples"></a>範例

若要使用一般安裝程式和無 UI 安裝封裝 *C:\example.msi*，請輸入：

```
msiexec.exe /i "C:\example.msi" /qn
```

## <a name="restart-options"></a>重新開機選項

如果您的安裝套件覆寫檔案，或嘗試變更正在使用的檔案，則可能需要重新開機才能完成安裝。

### <a name="syntax"></a>語法

```
msiexec.exe /i <path_to_package> [/norestart][/promptrestart][/forcerestart]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| `<path_to_package>` | 指定安裝套件檔案的位置和名稱。 |
| /norestart | 在安裝完成之後，停止裝置重新開機。 |
| /promptrestart | 如果需要重新開機，則提示使用者。 |
| /forcerestart | 在安裝完成後重新開機裝置。 |

#### <a name="examples"></a>範例

若要安裝封裝 *C:\example.msi*，請使用一般安裝程式，而不是在結束時重新開機，請輸入：

```
msiexec.exe /i "C:\example.msi" /norestart
```

## <a name="logging-options"></a>記錄選項

如果您需要對安裝套件進行偵錯工具，您可以設定參數來建立含有特定資訊的記錄檔。

### <a name="syntax"></a>語法

```
msiexec.exe [/i][/x] <path_to_package> [/L{i|w|e|a|r|u|c|m|o|p|v|x+|!|*}] <path_to_log>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /i | 指定一般安裝。 |
| /x | 卸載封裝。 |
| `<path_to_package>` | 指定安裝套件檔案的位置和名稱。 |
| /li | 開啟記錄，並在輸出記錄檔中包含狀態訊息。 |
| /lw | 開啟記錄，並在輸出記錄檔中包含非嚴重警告。 |
| /le | 開啟記錄，並在輸出記錄檔中包含所有錯誤訊息。 |
| /la | 開啟記錄功能，並包含在輸出記錄檔中啟動動作時的相關資訊。 |
| /lr | 開啟記錄，並在輸出記錄檔中包含動作特定的記錄。 |
| /lu | 開啟記錄，並在輸出記錄檔中包含使用者要求資訊。 |
| /lc | 開啟記錄，並在輸出記錄檔中包含初始 UI 參數。 |
| /lm | 開啟記錄，並在輸出記錄檔中包含記憶體不足或嚴重的離開資訊。 |
| /lo | 開啟記錄，並在輸出記錄檔中包含磁碟空間不足的訊息。 |
| /lp | 開啟記錄，並在輸出記錄檔中包含終端機屬性。 |
| /lp | 開啟記錄，並在輸出記錄檔中包含終端機屬性。 |
| /lv | 開啟記錄，並在輸出記錄檔中包含詳細資訊輸出。 |
| /lp | 開啟記錄，並在輸出記錄檔中包含終端機屬性。 |
| /lx | 開啟記錄，並在輸出記錄檔中包含額外的調試資訊。 |
| /l + | 開啟記錄，並將資訊附加至現有的記錄檔。 |
| /l! | 開啟記錄，並將每一行排清到記錄檔。 |
| /l | 會開啟記錄並記錄所有資訊，但詳細資訊 (**/lv**) 或 (**/lx**) 的額外調試資訊。 |
| `<path_to_logfile>` | 指定輸出記錄檔的位置和名稱。 |

#### <a name="examples"></a>範例

若要安裝封裝 *C:\example.msi*，請使用一般安裝程式並提供所有提供的記錄資訊，包括詳細資訊輸出，以及將輸出記錄檔儲存在 *C:\package.log*，輸入：

```
msiexec.exe /i "C:\example.msi" /L*V "C:\package.log"
```

## <a name="update-options"></a>更新選項

您可以使用安裝套件來套用或移除更新。

### <a name="syntax"></a>語法

```
msiexec.exe [/p][/update][/uninstall[/package<product_code_of_package>]] <path_to_package>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /p | 安裝修補程式。 如果您要以無訊息方式安裝，則也必須將 REINSTALLMODE 屬性設定為 *ecmus* ，並重新安裝為 *ALL*。 否則，修補程式只會更新目標裝置上快取的 MSI。 |
| /update | 安裝修補程式選項。 如果您要套用多個更新，您必須使用分號 (; ) 來分隔它們。 |
| /package | 安裝或設定產品。 |

#### <a name="examples"></a>範例

```
msiexec.exe /p "C:\MyPatch.msp"
msiexec.exe /p "C:\MyPatch.msp" /qb REINSTALLMODE="ecmus" REINSTALL="ALL"
msiexec.exe /update "C:\MyPatch.msp"
```

```
msiexec.exe /uninstall {1BCBF52C-CD1B-454D-AEF7-852F73967318} /package {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

其中第一個 GUID 是 patch GUID，而第二個 GUID 是套用修補程式的 MSI 產品代碼。

## <a name="repair-options"></a>修復選項

您可以使用此命令來修復已安裝的套件。

### <a name="syntax"></a>語法

```
msiexec.exe [/f{p|o|e|d|c|a|u|m|s|v}] <product_code>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /fp | 如果遺失檔案，則修復封裝。 |
| /fo | 如果檔案遺失或已安裝較舊的版本，請修復封裝。 |
| /fe | 如果遺失檔案，或安裝了相等或較舊的版本，請修復封裝。 |
| /fd | 如果檔案遺失或安裝不同的版本，請修復封裝。 |
| /fc | 如果遺失檔案，或總和檢查碼與計算值不符，則修復封裝。 |
| /fa | 強制重新安裝所有檔案。 |
| /fu | 修復所有必要的使用者特定登錄專案。 |
| /fm | 修復所有必要的電腦特定登錄專案。 |
| /fs | 修復所有現有的快捷方式。 |
| /fc | 從來源執行，然後重新快取本機封裝。 |

#### <a name="examples"></a>範例

若要強制根據要修復的 MSI 產品代碼重新安裝所有檔案， *{AAD3D77A-7476-469F-ADF4-04424124E91D}*，請輸入：

```
msiexec.exe /fa {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

## <a name="set-public-properties"></a>設定公用屬性

您可以透過這個命令設定公用屬性。 如需可用屬性和設定方式的詳細資訊，請參閱 [公用屬性](/windows/win32/msi/public-properties)。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Msiexec.exe 命令列選項](/windows/win32/msi/command-line-options)

- [標準安裝程式命令列選項](/windows/win32/msi/standard-installer-command-line-options)

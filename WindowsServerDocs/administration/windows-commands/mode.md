---
title: 模式
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 528277075f7448c86ca2d660c5e65c59098afbc0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839431"
---
# <a name="mode"></a>模式



顯示系統狀態、變更系統設定，或重新設定埠或裝置。 如果使用時不含參數，**模式**會顯示主控台的所有可控制屬性和可用的 COM 裝置。

您可以使用**模式**來執行下列工作，每個工作都使用不同的語法：
-   [設定序列通訊埠](#BKMK_1)
-   [顯示所有裝置或單一裝置的狀態](#BKMK_2)
-   [將平行埠的輸出重新導向至序列通訊埠](#BKMK_3)
-   [若要選取、重新整理或顯示主控台的字碼頁編號](#BKMK_4)
-   [若要變更命令提示字元螢幕緩衝區的大小](#BKMK_5)
-   [設定鍵盤的按鍵速度](#BKMK_6)

## <a name="to-configure-a-serial-communications-port"></a><a name=BKMK_1></a>設定序列通訊埠

### <a name="syntax"></a>語法

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>參數

|  參數  |                                                                                                                                                                                     描述                                                                                                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Com\<M > [：]  |                                                                                                                                                      指定非同步 Prncnfg vbshronous 通訊埠的編號。                                                                                                                                                      |
|  串列傳輸速率 =\<B >  | 指定傳輸速率（以每秒位數為單位）。 下表列出*B*的有效縮寫和其相關費率。</br>-   **11** = 110 波特</br>-   **15** = 150 波特</br>-   **30** = 300 波特</br>-   **60** = 600 波特</br>-   **12** = 1200 波特</br>-   **24** = 2400 波特</br>-   **48** = 4800 波特</br>-   **96** = 9600 波特</br>-   **19** = 19200 波特 |
| 同位檢查 =\<P > |                              指定系統如何使用同位檢查單位來檢查傳輸錯誤。 下表列出*P*的有效值。預設值為**e**。 並非所有電腦都支援**m**和**s**的值。</br>-   **n** = 無</br>-   **e** = 偶數</br>-   **o** = 奇數</br>-   **m** = mark</br>-   **s** = 空格                              |
|  data =\<D >  |                                                                                                    指定字元中的資料位數目。 **D**的有效值範圍為5到8。 預設值為 7。 並非所有電腦都支援5和6的值。                                                                                                     |
|  停止 =\<S >  |                                                                                  指定定義字元結尾的停止位數目：1、1.5 或2。 如果傳輸速率為110，預設值為2。 否則，預設值為1。 並非所有電腦都支援值1.5。                                                                                   |
|   to = {on    |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   xon = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  odsr = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  octs = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   dtr = {on   |                                                                                                                                                                                         off                                                                                                                                                                                         |
|   rts = {on   |                                                                                                                                                                                         off                                                                                                                                                                                         |
|  idsr = {on   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|     /?      |                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                         |

## <a name="to-display-the-status-of-all-devices-or-of-a-single-device"></a><a name=BKMK_2></a>顯示所有裝置或單一裝置的狀態

### <a name="syntax"></a>語法

```
mode [<Device>] [/status]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<裝置 >|指定您想要顯示其狀態之裝置的名稱。|
|/status|要求任何重新導向之平行印表機的狀態。 您可以將 **/status**命令列選項縮寫為 **/sta**。|
|/?|在命令提示字元顯示說明。|

### <a name="remarks"></a>備註

如果使用時不含參數，**模式**會顯示系統上安裝之所有裝置的狀態。

## <a name="to-redirect-output-from-a-parallel-port-to-a-serial-communications-port"></a><a name=BKMK_3></a>將平行埠的輸出重新導向至序列通訊埠

### <a name="syntax"></a>語法

```
mode lpt<N>[:]=com<M>[:]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|lpt\<N > [：]|必要。 指定平行埠。 *N*的有效值在1到3的範圍內。|
|com\<M > [：]|必要。 指定序列埠。 *M*的有效值為1到4的範圍。|
|/?|在命令提示字元顯示說明。|

### <a name="remarks"></a>備註

您必須是 Administrators 群組的成員，才能重新導向列印。

### <a name="examples"></a>範例

若要設定您的系統，讓它將平行印表機輸出傳送至序列印表機，您必須使用**模式**命令兩次。 第一次，使用**模式**來設定序列埠。 第二次，使用**模式**將平行印表機輸出重新導向至您在第一個**模式**命令中指定的序列埠。

例如，如果您的序列印表機在4800傳輸上使用偶位檢查來運作，而且它連線到 COM1 埠（您電腦上的第一個序列連線），請輸入：
```
mode com1 48,e,,,b
mode lpt1=com1
```
如果您將平行印表機輸出從 LPT1 重新導向到 COM1，但接著決定要使用 LPT1 來列印檔案，請在列印檔案之前輸入下列命令：
```
mode lpt1
```
此命令可防止將檔案從 LPT1 重新導向到 COM1。

## <a name="to-select-refresh-or-display-the-numbers-of-the-code-pages-for-the-console"></a><a name=BKMK_4></a>若要選取、重新整理或顯示主控台的字碼頁編號

### <a name="syntax"></a>語法

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<裝置 >|必要。 指定您要選取字碼頁的裝置。 CON 是唯一有效的裝置名稱。|
|字碼頁選取 =|必要。 指定要搭配指定裝置使用的字碼頁。 您可以將**字碼頁** **選取**為**cp**選擇**的縮寫。**|
|\<YYY >|必要。 指定要選取的字碼頁編號。 下列清單顯示每個支援的字碼頁，以及其國家/地區或語言。</br>437：美國</br>850：多語系（拉丁 I）</br>852：斯拉夫語（拉丁 II）</br>855：斯拉夫文（俄文）</br>857：土耳其文</br>860：葡萄牙文</br>861：冰島文</br>863：加拿大-法文</br>865：北歐</br>866：俄文</br>869：新式希臘文|
|頁|必要。 顯示針對指定裝置選取之字碼頁的數目（如果有的話）。|
|/status|顯示針對指定裝置所選取的目前字碼頁數目。 您可以將 **/status**縮寫為 **/sta**。 無論您是否指定 **/status**，**模式代碼**頁都會顯示針對指定裝置選取的字碼頁編號。|
|/?|在命令提示字元顯示說明。|

## <a name="to-change-the-size-of-the-command-prompt-screen-buffer"></a><a name=BKMK_5></a>若要變更命令提示字元螢幕緩衝區的大小

### <a name="syntax"></a>語法

```
mode con[:] [cols=<C>] [lines=<N>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|con [：]|必要。 指出變更適用于 [命令提示字元] 視窗。|
|cols =\<C >|指定命令提示字元螢幕緩衝區中的資料行數目。|
|行 =\<N >|指定命令提示字元螢幕緩衝區中的行數。|
|/?|在命令提示字元顯示說明。|

## <a name="to-set-the-keyboard-typematic-rate"></a><a name=BKMK_6></a>設定鍵盤的按鍵速度

### <a name="syntax"></a>語法

```
mode con[:] [rate=<R> delay=<D>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|con [：]|必要。 指的是鍵盤。|
|速率 =\<R >|指定當您按住按鍵時，在螢幕上重複字元的速率。|
|delay =\<D >|指定在字元輸出重複之前，按住按鍵後所經過的時間量。|
|/?|在命令提示字元顯示說明。|

### <a name="remarks"></a>備註

- 輸入速率是當您按住該字元的索引鍵時，字元重複的速率。 [按鍵速率] 有兩個元件： [速率] 和 [延遲]。 有些鍵盤無法辨識此命令。
- 使用**速率 =** <em>R</em>

  有效值的範圍介於1到32之間。 這些值大約等於每秒2到30個字元。 IBM 相容鍵盤的預設值為20，而 IBM PS/2 相容鍵盤則為21。 如果您設定了速率，則也必須設定延遲。
- 使用**delay**=*D*

  *D*的有效值為1、2、3和4（代表0.25、0.50、0.75 和1秒）。 預設值為2。 如果您設定延遲，則也必須設定速率。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
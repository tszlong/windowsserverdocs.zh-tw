---
title: KMS 用戶端安裝識別碼
description: 從 KMS 伺服器啟動 Windows 產品所需的識別碼
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 11/12/2019
ms.topic: how-to
ms.openlocfilehash: 6e39590e1581056f741585fa1123d8db9ac764cf
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949244"
---
# <a name="kms-client-setup-keys"></a>KMS 用戶端安裝識別碼

>適用於：Windows Server 2019、Windows Server 半年通道、Windows Server 2016、Windows 10

根據預設，執行 Windows Server、Windows 10、Windows 8.1、Windows Server 2012 R2、Windows 8、Windows Server 2012、Windows 7、Windows Server 2008 R2、Windows Vista 以及 Windows Server 2008 大量授權版本的電腦為不需要另外設定的 KMS 用戶端。

> [!NOTE]
> 在下列表格中，"LTSC" 代表「長期維護通道」，而 "LTSB" 是指「長期維護分支」。

**若要使用此處所列的金鑰 (它們是 GVLK)，您的部署中必須已有執行中的 KMS 主機。** 如果您尚未設定 KMS 主機，請參閱 [Deploy KMS Activation](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502531(v=ws.11)) 的步驟來安裝一部 KMS 主機。

如果您將電腦從 KMS 主機、MAK 或 Windows 零售版轉換成 KMS 用戶端，請從下表安裝適用的安裝識別碼 (GVLK)。 若要安裝用戶端安裝識別碼，請在用戶端開啟系統管理命令提示字元，輸入 **slmgr /ipk \<setup key\>** ，然後按 **Enter** 鍵。

| 如果您想要...    | ...使用這些資源   |
|--------------------|------------------------|
| 在大量啟用情況之外啟用 Windows (也就您嘗試啟用零售版的 Windows)，**這些金鑰將無法運作**。 | 在零售版的 Windows 中使用下列連結： |
| 修正您嘗試啟用 Windows 8.1、Windows Server 2012 R2 或更新版本的系統時收到的這個錯誤：「錯誤：0xC004F050 軟體授權服務報告指出產品金鑰無效」… | 如果 KMS 主機執行 Windows 8.1、Windows Server 2012 R2、Windows 8 或 Windows Server 2012，請在 KMS 主機上[安裝此更新](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8-1-and-windows-server-2012-r2) 。 |

-   [取得 Windows 10](https://www.microsoft.com/windows/get-windows-10)

-   [取得新的 Windows 產品金鑰](https://support.microsoft.com/help/10749/windows-product-key)

-   [正版 Windows 說明與使用方法](https://support.microsoft.com/help/15087/windows-genuine)


>   如果您是執行 Windows Server 2008 R2 或 Windows 7，請留意以 KMS 主機作為 Windows 10 用戶端的支援更新。

## <a name="windows-server-semi-annual-channel-versions"></a>Windows Server 半年通道版本

### <a name="windows-server-version-1909-version-1903-and-version-1809"></a>Windows Server 版本1909、版本 1903 和版本 1809

| 作業系統版本  | KMS 用戶端安裝識別碼          |
|---------------------------|-------------------------------|
| Windows Server Datacenter | 6NMRW-2C8FM-D24W7-TQWMY-CWH2D |
| Windows Server Standard   | N2KJX-J94YW-TQVFB-DG9YT-724CC |

## <a name="windows-server-ltscltsb-versions"></a>Windows Server LTSC/LTSB 版本

### <a name="windows-server-2019"></a>Windows Server 2019
| 作業系統版本       | KMS 用戶端安裝識別碼          |
|--------------------------------|-------------------------------|
| Windows Server 2019 Datacenter | WMDGN-G9PQG-XVVXX-R3X43-63DFG |
| Windows Server 2019 Standard   | N69G4-B89J2-4G8F4-WWYCC-J464C |
| Windows Server 2019 Essentials | WVDHN-86M7X-466P6-VHXV7-YY726 |

### <a name="windows-server-2016"></a>Windows Server 2016

| 作業系統版本       | KMS 用戶端安裝識別碼          |
|--------------------------------|-------------------------------|
| Windows Server 2016 Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| Windows Server 2016 Standard   | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| Windows Server 2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |

## <a name="windows-10-all-supported-semi-annual-channel-versions"></a>Windows 10 所有受支援的半年通道版本

如需支援的版本和服務終止日期的相關資訊，請參閱 [Windows 生命週期資料表](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)。

| 作業系統版本          | KMS 用戶端安裝識別碼          |
|-----------------------------------|-------------------------------|
| Windows 10 Pro                    | W269N-WFGWX-YVC9B-4J6C9-T83GX |
| Windows 10 Pro N                  | MH37W-N47XK-V7XM9-C7227-GCQG9 |
| Windows 10 工作站專業版   | NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J |
| Windows 10 工作站專業版 N | 9FNHH-K3HBT-3W4TD-6383H-6XYWF |
| Windows 10 專業教育版          | 6TP4R-GNPTD-KYYHQ-7B7DP-J447Y |
| Windows 10 專業教育版 N        | YVWGF-BXNMC-HTQYQ-CPQ99-66QFC |
| Windows 10 Education              | NW6C2-QMPVW-D7KKK-3GKT6-VCFB2 |
| Windows 10 Education N            | 2WH4N-8QGBV-H22JP-CT43Q-MDWWJ |
| Windows 10 Enterprise             | NPPR9-FWDCX-D2C8J-H872K-2YT43 |
| Windows 10 Enterprise N           | DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4 |
| Windows 10 企業版 G           | YYVX9-NTFWV-6MDM3-9PT4T-4M68B |
| Windows 10 企業版 G N         | 44RPN-FTY23-9VTTB-MP9BX-T84FV |

## <a name="windows-10-ltscltsb-versions"></a>Windows 10 LTSC/LTSB 版本

### <a name="windows-10-ltsc-2019"></a>Windows 10 LTSC 2019

| 作業系統版本          | KMS 用戶端安裝識別碼          |
|-----------------------------------|-------------------------------|
| Windows 10 企業版 LTSC 2019   | M7XTQ-FN8P6-TTKYV-9D4CC-J462D |
| Windows 10 企業版 N LTSC 2019 | 92NFX-8DJQP-P6BBQ-THF9C-7CG2H |

### <a name="windows-10-ltsb-2016"></a>Windows 10 LTSB 2016

| 作業系統版本          | KMS 用戶端安裝識別碼          |
|-----------------------------------|-------------------------------|
| Windows 10 Enterprise LTSB 2016   | DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ |
| Windows 10 Enterprise N LTSB 2016 | QFFDN-GRT3P-VKWWX-X7T3R-8B639 |

### <a name="windows-10-ltsb-2015"></a>Windows 10 LTSB 2015

| 作業系統版本          | KMS 用戶端安裝識別碼          |
|-----------------------------------|-------------------------------|
| Windows 10 Enterprise 2015 LTSB   | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Enterprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |

## <a name="earlier-versions-of-windows-server"></a>舊版 Windows Server

### <a name="windows-server-version-1803"></a>Windows Server 1803 版

| 作業系統版本  | KMS 用戶端安裝識別碼          |
|---------------------------|-------------------------------|
| Windows Server Datacenter | 2HXDN-KRXHB-GPYC7-YCKFJ-7FVDG |
| Windows Server Standard   | PTXN8-JFHJM-4WC78-MPCBR-9W4KR |

### <a name="windows-server-version-1709"></a>Windows Server 1709 版

| 作業系統版本  | KMS 用戶端安裝識別碼          |
|---------------------------|-------------------------------|
| Windows Server Datacenter | 6Y6KB-N82V8-D8CQV-23MJW-BWTG6 |
| Windows Server Standard   | DPCNP-XQFKJ-BJF7R-FRC8D-GF6G4 |

### <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

| 作業系統版本               | KMS 用戶端安裝識別碼          |
|----------------------------------------|-------------------------------|
| Windows Server 2012 R2 Server Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| Windows Server 2012 R2 Datacenter      | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| Windows Server 2012 R2 Essentials      | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

### <a name="windows-server-2012"></a>Windows Server 2012

| 作業系統版本                | KMS 用戶端安裝識別碼          |
|-----------------------------------------|-------------------------------|
| Windows Server 2012                     | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| Windows Server 2012 N                   | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| Windows Server 2012 單一語言     | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| Windows Server 2012 國家/地區特定    | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| Windows Server 2012 Server Standard     | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| Windows Server 2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| Windows Server 2012 MultiPoint Premium  | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |
| Windows Server 2012 Datacenter          | 48HP8-DN98B-MYWDG-T2DCC-8W83P |


### <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

| 作業系統版本                         | KMS 用戶端安裝識別碼          |
|--------------------------------------------------|-------------------------------|
| Windows Server 2008 R2 Web                       | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| Windows Server 2008 R2 HPC Edition               | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| Windows Server 2008 R2 Standard                  | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| Windows Server 2008 R2 Enterprise                | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| Windows Server 2008 R2 Datacenter                | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| Windows Server 2008 R2 for Itanium-based Systems | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |

### <a name="windows-server-2008"></a>Windows Server 2008

| 作業系統版本                       | KMS 用戶端安裝識別碼          |
|------------------------------------------------|-------------------------------|
| Windows Web Server 2008                        | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
| Windows Server 2008 Standard                   | TM24T-X9RMF-VWXK6-X8JC9-BFGM2 |
| Windows Server 2008 Standard (無 Hyper-V)   | W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ |
| Windows Server 2008 Enterprise                 | YQGMW-MPWTJ-34KDK-48M3W-X4Q6V |
| Windows Server 2008 Enterprise (無 Hyper-V) | 39BXF-X8Q23-P2WWT-38T2F-G3FPG |
| Windows Server 2008 HPC                        | RCTX3-KWVHP-BR6TB-RB6DM-6X7HP |
| Windows Server 2008 Datacenter                 | 7M67G-PC374-GR742-YH8V4-TCBY3 |
| Windows Server 2008 Datacenter (無 Hyper-V) | 22XQ2-VRXRG-P8D42-K34TD-G3QQC |
| Windows Server 2008 for Itanium-Based Systems  | 4DWFP-JF3DJ-B7DTH-78FJB-PDRHK |

## <a name="earlier-versions-of-windows"></a>舊版 Windows

### <a name="windows-81"></a>Windows 8.1

| 作業系統版本 | KMS 用戶端安裝識別碼          |
|--------------------------|-------------------------------|
| Windows 8.1 專業版          | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 專業版 N        | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows 8.1 Enterprise   | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows 8.1 Enterprise N | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |

### <a name="windows-8"></a>Windows 8

| 作業系統版本 | KMS 用戶端安裝識別碼          |
|--------------------------|-------------------------------|
| Windows 8 Pro            | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 專業版 N          | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows 8 企業版     | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows 8 企業版 N   | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |


### <a name="windows-7"></a>Windows 7

| 作業系統版本 | KMS 用戶端安裝識別碼          |
|--------------------------|-------------------------------|
| Windows 7 專業版   | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows 7 專業版 N | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows 7 專業版 E | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows 7 Enterprise     | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows 7 企業版 N   | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows 7 企業版 E   | C29WB-22CC8-VJ326-GHFJW-H9DH4 |


另請參閱

• [大量啟用規劃](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134042(v=ws.11))

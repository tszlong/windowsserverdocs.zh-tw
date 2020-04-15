---
title: Nano 伺服器快速入門
description: 快速在實體或虛擬機器上部署基本 Nano Server 的步驟
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/05/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 7729b853f2e54c7f99d428fcb821a68d7a22aef0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826811"
---
# <a name="nano-server-quick-start"></a>Nano 伺服器快速入門

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

遵循本節中的步驟即可使用 DHCP 取得 IP 位址，以快速開始進行 Nano Server 的基本部署。 您可以在虛擬機器中執行 Nano Server VHD，或在實體電腦上開機到此 VHD；這兩者的步驟稍有不同。

當您透過這些快速入門步驟試用基本概念之後，您可以在[部署 Nano Server](Deploy-Nano-Server.md) 中找到有關建立您自己的映像、透過幾個方法建立套件管理、網域作業等詳細資訊。
  
**虛擬機器中的 Nano Server**  
  
請遵循下列步驟，建立將在虛擬機器中執行的 Nano Server VHD。  
  
## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>在虛擬機器中快速部署 Nano Server  
  
1. 將 *NanoServerImageGenerator* 資料夾從 Windows Server 2016 ISO 中的 \NanoServer 資料夾複製到您硬碟上的資料夾。  
  
2. 以系統管理員身分啟動 Windows PowerShell，將目錄變更為您放置 NanoServerImageGenerator 資料夾的資料夾，然後使用 `Import-Module .\NanoServerImageGenerator -Verbose` 匯入模組  
   >[!NOTE]  
   >您可能需要調整 Windows PowerShell 執行原則。 `Set-ExecutionPolicy RemoteSigned` 應該適用。  
  
3. 執行下列命令為 Standard Edition 建立設定電腦名稱並包含 Hyper-V **客體驅動程式**的 VHD，該命令會提示您輸入新 VHD 的系統管理員密碼：  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>`，其中  
  
   -   **-MediaPath <媒體的根目錄路徑\>** 指定 Windows Server 2016 ISO 內容的根目錄路徑。 例如，如果您已將 ISO 內容複製到 d:\TP5ISO，您將使用該路徑。  
  
   -   **-BasePath** (選擇性) 指定將要建立以複製 Nano Server WIM 和套件的目的地資料夾。  
  
   -   **-TargetPath** 指定將要建立產生之 VHD 或 VHDX 的路徑，其中包含檔名和副檔名。  
  
   -   **Computer_name** 指定您要建立之 Nano Server 虛擬機器將具有的電腦名稱。  
  
   **範例：** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`  
  
   此範例會從掛接為 f:\\ 的 ISO 建立 VHD。 建立 VHD 時，它會使用您執行 New-NanoServerImage 所在的相同目錄中稱為 Base 的資料夾；它會將 VHD (稱為 Nano.vhd) 放在執行命令的來源資料夾中稱為 Nano1 的資料夾。 電腦名稱會是 Nano1。 產生的 VHD 將會包含 Windows Server 2016 Standard Edition，並且適用於 Hyper-V 虛擬機器部署。 如果您需要第 1 代虛擬機器，請為 -TargetPath 指定 **.vhd** 副檔名來建立 VHD 映像。 針對第 2 代虛擬機器，請為 -TargetPath 指定 **.vhdx** 副檔名來建立 VHDX 映像。 您也可以為 -TargetPath 指定 **.wim** 副檔名來直接產生 WIM 檔案。  
  
   > [!NOTE]  
   > Windows 8.1、Windows 10、Windows Server 2012 R2 和 Windows Server 2016 支援 New-NanoServerImage。  
  
4. 在 Hyper-V 管理員中，建立新的虛擬機器並使用步驟 3 中所建立的 VHD。  
  
5. 啟動虛擬機器，然後在 Hyper-V 管理員中連線到虛擬機器。  
  
6. 使用您在步驟 3 中執行指令碼時所提供的系統管理員和密碼來登入修復主控台 (請參閱本指南中的＜Nano Server 修復主控台＞一節)。  
   > [!NOTE]  
   > 修復主控台只支援基本鍵盤功能。 不支援鍵盤背光、10 個按鍵區段和鍵盤配置切換 (例如 Caps Lock 和 Number Lock)。
  
7. 取得 Nano Server 虛擬機器的 IP 位址，並使用 Windows PowerShell 遠端執行功能或其他遠端管理工具來連線到虛擬機器及從遠端進行管理。  
  
**實體電腦上的 Nano Server**  
  
您也可以使用預先安裝的裝置驅動程式，建立將要在實體電腦上執行 Nano Server 的 VHD。 如果您的硬體為了開機或連線到網路所需的驅動程式尚未提供，請遵循本指南之＜新增其他驅動程式＞一節中的步驟。  
  
## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>在實體電腦上快速部署 Nano Server  
  
1.  將 *NanoServerImageGenerator* 資料夾從 Windows Server 2016 ISO 中的 \NanoServer 資料夾複製到您硬碟上的資料夾。  
  
2.  以系統管理員身分啟動 Windows PowerShell，將目錄變更為您放置 NanoServerImageGenerator 資料夾的資料夾，然後使用 `Import-Module .\NanoServerImageGenerator -Verbose` 匯入模組  
  
>[!NOTE]  
>您可能需要調整 Windows PowerShell 執行原則。 `Set-ExecutionPolicy RemoteSigned` 應該適用。  
  
3. 執行下列命令來建立設定電腦名稱並包含 OEM 驅動程式和 Hyper-V 的 VHD，該命令會提示您輸入新 VHD 的系統管理員密碼：  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering`，其中  
  
   -   **-MediaPath <媒體的根目錄路徑\>** 指定 Windows Server 2016 ISO 內容的根目錄路徑。 例如，如果您已將 ISO 內容複製到 d:\TP5ISO，您將使用該路徑。  
  
   -   **-BasePath** 指定將要建立以複製 Nano Server WIM 和套件的目的地資料夾 (此為選擇性參數)。  
  
   -   **TargetPath** 指定將要建立產生之 VHD 或 VHDX 的路徑，其中包含檔名和副檔名。  
  
   -   **Computer_name** 是您要建立之 Nano Server 的電腦名稱。  
  
   **範例：** `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`  
  
   此範例會從掛接為 F:\\ 的 ISO 建立 VHD。 建立 VHD 時，它會使用您執行 New-NanoServerImage 所在的相同目錄中稱為 Base 的資料夾；它會將 VHD 放在執行命令的來源資料夾中稱為 Nano1 的資料夾。 電腦名稱會是 Nano-srv1，而且會安裝最常見硬體的 OEM 驅動程式、啟用 Hyper-V 角色和叢集功能， 並使用標準 Nano 版本。  
  
4. 以系統管理員身分登入您要執行 Nano Server VHD 的實體伺服器。  
  
5. 將此指令碼建立的 VHD 複製到實體電腦，並將它設定為從這個新的 VHD 開機。 若要這樣做，請遵循下列步驟：  
  
   1.  掛接產生的 VHD。 在此範例中是掛接在 D:\\ 下。  
  
   2.  執行 **bcdboot d:\windows**。  
  
   3.  取消掛接 VHD。  
  
6. 將實體電腦開機到 Nano Server VHD。  
  
7. 使用您在步驟 3 中執行指令碼時所提供的系統管理員和密碼來登入修復主控台 (請參閱本指南中的＜Nano Server 修復主控台＞一節)。
   > [!NOTE]  
   > 修復主控台只支援基本鍵盤功能。 不支援鍵盤背光、10 個按鍵區段和鍵盤配置切換 (例如 Caps Lock 和 Number Lock)。 
  
8. 取得 Nano Server 電腦的 IP 位址，並使用 Windows PowerShell 遠端執行功能或其他遠端管理工具來連線到虛擬機器及從遠端進行管理。  

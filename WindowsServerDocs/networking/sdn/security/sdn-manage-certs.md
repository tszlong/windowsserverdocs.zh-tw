---
title: 管理軟體定義網路的憑證
description: 當您在 Windows Server 2016 Datacenter 中部署軟體定義網路功能（SDN）時，您可以使用本主題來瞭解如何管理網路控制站 Northbound 和 Southbound 通訊的憑證。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: b1cff080630c68ee8c4b7f0904f8fd0978330edc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405982"
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理軟體定義網路的憑證

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解當您在 Windows Server 2016 Datacenter 中部署軟體定義的網路\(SDN\) ，而且您使用的是 System 時，如何管理網路控制站 Northbound 和 Southbound 通訊的憑證將 SCVMM \( \) Virtual Machine Manager 為您的 SDN 管理用戶端。

>[!NOTE]
>如需網路控制卡的總覽資訊，請參閱[網路控制](../technologies/network-controller/Network-Controller.md)卡。

如果您未使用 Kerberos 來保護網路控制站通訊，您可以使用 x.509 憑證來進行驗證、授權和加密。

Windows Server 2016 Datacenter 中的 SDN 同時支援\-自我簽署和證書\(頒發\)機構單位 CA 簽署的 x.509 憑證。 本主題提供逐步指示，說明如何建立這些憑證，並將其套用至使用管理用戶端和網路裝置（例如軟體）的 Southbound 通訊來保護網路控制站 Northbound 通道Load Balancer \(SLB\)。
.
當您使用以憑證\-為基礎的驗證時，您必須在以下列方式使用的網路控制卡節點上註冊一個憑證。

1. 在網路控制卡節點\(與\)管理用戶端（例如 System Center Virtual Machine Manager）之間，使用安全通訊端層 SSL 來加密 Northbound 通訊。
2. 網路控制站節點與 Southbound 裝置和服務（例如 hyper-v 主機和軟體負載平衡\(器 SLBs\)）之間的驗證。

## <a name="creating-and-enrolling-an-x509-certificate"></a>建立和註冊 x.509 憑證

您可以建立並註冊自我\-簽署憑證或 CA 所發行的憑證。

>[!NOTE]
>當您使用 SCVMM 來部署網路控制站時，您必須指定在設定網路控制站服務範本期間用來加密 Northbound 通訊的 x.509 憑證。

憑證設定必須包含下列值。

- **RestEndPoint**文字方塊的值必須是網路控制站的完整功能變數名稱\(FQDN\)或 IP 位址。 
- **RestEndPoint**值必須符合 x.509 憑證的主體\(名稱一般名稱，\) CN。

### <a name="creating-a-self-signed-x509-certificate"></a>建立自我\-簽署的 x.509 憑證

您可以遵循下列步驟來建立自我簽署的 x.509 憑證，並將其匯出為\(使用密碼\)保護的私密金鑰，方法是針對單一\-節點和網路\-控制站的多重節點部署.

當您建立自我\-簽署的憑證時，您可以使用下列指導方針。

- 您可以針對 DnsName 參數使用網路控制器 REST 端點的 IP 位址，但不建議這麼做，因為它要求網路控制站節點都位於單一管理子網\(內，例如在單一機架上\)
- 針對多節點 NC 部署，您指定的 DNS 名稱會變成網路控制\(站叢集 dns 主機的 FQDN 自動建立記錄。\) 
- 針對單一節點網路控制站部署，DNS 名稱可以是網路控制站的主機名稱，後面接著完整功能變數名稱。

#### <a name="multiple-node"></a>多個節點

您可以使用[SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>單一節點

您可以使用[SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>建立 CA\-簽署的 x.509 憑證

若要使用 CA 建立憑證，您必須已使用 Active Directory \(憑證服務\(AD CS\)來部署公開金鑰\)基礎結構 PKI。 

>[!NOTE]
>您可以使用協力廠商 Ca 或工具（例如 openssl）來建立要與網路控制站搭配使用的憑證，不過本主題中的指示僅適用于 AD CS。 若要瞭解如何使用協力廠商 CA 或工具，請參閱您所使用之軟體的說明文件。

使用 CA 建立憑證包含下列步驟。

1. 您或您組織的網域或安全性系統管理員設定憑證範本
2. 您或您組織的網路控制站系統管理員或 SCVMM 系統管理員向 CA 要求新的憑證。

#### <a name="certificate-configuration-requirements"></a>憑證設定需求

當您在下一個步驟中設定憑證範本時，請確定您設定的範本包含下列必要元素。

1. 憑證主體名稱必須是 Hyper-v 主機的 FQDN
2. 憑證必須放在本機電腦的個人存放區中（My – cert： \ localmachine\my）
3. 憑證必須具有伺服器驗證（EKU：1.3.6.1.5.5.7.3.1）和用戶端驗證（EKU：1.3.6.1.5.5.7.3.2）應用程式原則。

>[!NOTE]
>如果在 hyper-v \( \) \( \)主機上的個人 My – cert： \ localmachine\my 憑證存放區有一個以上的 x.509 憑證，主體名稱（CN）是主機的完整功能變數名稱 FQDN，\-確定 SDN 使用的憑證有額外的自訂 [增強金鑰使用方法] 屬性和 [OID] 1.3.6.1.4.1.311.95.1.1.1。 否則，網路控制站與主機之間的通訊可能無法正常執行。

#### <a name="to-configure-the-certificate-template"></a>設定憑證範本
  
>[!NOTE]
>執行此程式之前，您應該先參閱憑證範本主控台中的憑證需求和可用的憑證範本。 您可以修改現有的範本，或建立現有範本的複本，然後修改範本的複本。 建議您建立現有範本的複本。

1. 在安裝 AD CS 的伺服器上，按一下伺服器管理員中的 [**工具**]，然後按一下 [**憑證授權單位**單位]。 [憑證授權單位單位] \(Microsoft\) Management Console MMC 隨即開啟。 
2. 在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下 [**憑證範本**]，然後按一下 [**管理**]。
3. [憑證範本] 主控台隨即開啟。 所有憑證範本都會顯示在詳細資料窗格中。
4. 在詳細資料窗格中，按一下您想要複製的範本。
5.  按一下 [**動作**] 功能表，然後按一下 [**複製範本**]。 [範本**屬性**] 對話方塊隨即開啟。
6.  在 [範本**屬性**] 對話方塊的 [**主體名稱**] 索引標籤上，按一下 **[在要求中提供**]。 \(這是網路控制站 SSL 憑證的必要設定。\)
7.  在 [範本**屬性**] 對話方塊的 [**要求處理**] 索引標籤上，確定已選取 [**允許匯出私密金鑰**]。 也請確定已選取 [簽章]**和 [加密**] 目的。
8.  在 [範本**屬性**] 對話方塊的 [**擴充**功能] 索引標籤上，選取 [**金鑰使用**方式]，然後按一下 [**編輯**]。
9.  在 [簽章 **] 中，** 確定已選取 [**數位簽章**]。
10.  在 **[範本內容**] 對話方塊的 [**擴充**功能] 索引標籤上，選取 [**應用程式原則**]，然後按一下 [**編輯**]。
11.  在 [**應用程式原則**] 中，確定已列出 [**用戶端驗證**] 和 [**伺服器驗證**]。
12.  使用唯一的名稱來儲存憑證範本的複本，例如**網路控制卡範本**。

#### <a name="to-request-a-certificate-from-the-ca"></a>向 CA 要求憑證

您可以使用 [憑證] 嵌入式管理單元來要求憑證。 您可以要求任何已預先設定且可由處理憑證要求之 CA 的系統管理員使用的憑證類型。

**使用者**或本機系統**管理員**是完成此程式所需的最小群組成員資格。

1. 開啟電腦的 [憑證] 嵌入式管理單元。
2. 在主控台樹中，按一下 **[ \(憑證]\)[本機電腦**]。 選取 [**個人**] 憑證存放區。
3. 在 [**動作**] 功能表上，指向 **[所有工作]<strong>，然後按一下 **[要求新憑證</strong>] 以啟動 [憑證註冊嚮導]。 按一下 [下一步]。
4. 選取**您的系統管理員**憑證註冊原則所設定的，然後按 **[下一步]** 。
5. 根據您在上一節\)中設定的 CA 範本，選取 [ **Active Directory 註冊原則** \(]。
6. 展開 [**詳細資料**] 區段，並設定下列專案。
   1. 請確定**金鑰使用**方式同時包含<strong>數位簽章 * * 和 * * 金鑰加密</strong>。
   2. 請確定**應用程式原則**同時**包含伺服器驗證** \(1.3.6.1.5.5.7.3.1\)和\)**用戶端驗證** \(1.3.6.1.5.5.7.3.2。
7. 按一下 [內容]。
8. 在 [**主體**] 索引標籤的 [**主體名稱**] 中，于 [**類型**] 選取 [**一般名稱**]。 在 [值] 中，指定**網路控制卡 REST 端點**。
9. 按一下 [套用]，然後按一下 [確定]。
10. 按一下 **\[註冊\]** 。

在 [憑證] MMC 中，按一下 [個人] 存放區，以查看您已從 CA 註冊的憑證。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>將憑證匯出並複製到 SCVMM 程式庫

建立\-自我簽署或 CA \( \-簽署的憑證之後，您必須以 .pfx 格式\)匯出具有私密金鑰的憑證，而不使用 Base-64 .cer 格式\(的私密金鑰。\)從 [憑證] 嵌入式管理單元。 

接著，您必須將兩個匯出的檔案複製到您在匯入 NC 服務範本時所指定的**ServerCertificate.cr**和**NCCertificate.cr**資料夾。

1. 開啟 [憑證] 嵌入式管理單元（certlm.msc），並在本機電腦的 [個人] 憑證存放區中尋找憑證。
2. 以\-滑鼠右鍵按一下憑證，按一下 [**所有**工作]，然後按一下 [**匯出**]。 \[憑證匯出精靈\] 隨即開啟。 按一下 [下一步]。
3. 選取 **[是**，匯出私密金鑰] 選項，然後按 **[下一步]** 。
4. 選擇 [**個人資訊交換-PKCS #12] （。PFX）** ，並接受預設值以**包含憑證路徑中的所有憑證**（如果可能的話）。
5. 為您要匯出的憑證指派使用者/群組和密碼，然後按 **[下一步]** 。
6. 在 [要匯出的檔案] 頁面上，流覽要放置匯出檔案的位置，並為其命名。
7. 同樣地，匯出中的憑證。CER 格式。 注意：要匯出到的。CER 格式，取消核取 [是，匯出私密金鑰] 選項。
8. 複製。PFX 至 ServerCertificate.cr 資料夾。
9. 複製。CER 檔案至 NCCertificate.cr 資料夾。

當您完成時，請重新整理 SCVMM 程式庫中的這些資料夾，並確定您已複製這些憑證。 繼續進行網路控制站服務範本設定和部署。

## <a name="authenticating-southbound-devices-and-services"></a>驗證 Southbound 裝置和服務 

與主機和 SLB MUX 裝置的網路控制站通訊會使用憑證進行驗證。 與這些主機的通訊是透過 OVSDB 通訊協定，而與 SLB MUX 裝置通訊則是透過 WCF 通訊協定。

### <a name="hyper-v-host-communication-with-network-controller"></a>Hyper-v 主機與網路控制站的通訊

若要透過 OVSDB 與 Hyper-v 主機通訊，網路控制站必須向主機電腦出示憑證。 根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來與主機進行 southbound 通訊。

這就是 SSL 憑證必須設定用戶端驗證 EKU 的原因。 此憑證是在「伺服器」 REST 資源\(上設定的，hyper-v 主機在網路控制站中是以伺服器資源\)表示，而且可以藉由執行 Windows PowerShell 命令**Get-NetworkControllerServer 來查看**.

以下是伺服器 REST 資源的部分範例。

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

針對相互驗證，Hyper-v 主機也必須有憑證才能與網路控制站通訊。 

您可以從憑證授權單位\(單位 CA\)註冊憑證。 如果在主機電腦上找不到 CA 型憑證，SCVMM 會建立自我簽署的憑證，並在主機電腦上進行布建。

網路控制站和 Hyper-v 主機憑證必須彼此信任。 Hyper-v 主機憑證的根憑證必須存在於本機電腦的網路控制器 [受信任的根憑證授權單位] 存放區中，反之亦然。 

當您使用自我\-簽署的憑證時，SCVMM 會確保本機電腦的「信任的根憑證授權單位」存放區中有所需的憑證。 

如果您使用 Hyper-v 主機的 CA 型憑證，您必須確定 CA 根憑證存在於本機電腦的網路控制器的 [受信任的根憑證授權單位] 存放區中。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>與網路控制卡通訊的軟體 Load Balancer MUX

軟體 Load Balancer 多重\(通訊協定\) MUX 和網路控制站，會使用憑證進行驗證，以透過 WCF 通訊協定進行通訊。

根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來 southbound 與 Mux 裝置的通訊。 此憑證會在 "NetworkControllerLoadBalancerMux" REST 資源上設定，而且可以藉由執行 Powershell Cmdlet **NetworkControllerLoadBalancerMux**來查看。

MUX REST 資源\(的範例部分\)：

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

針對相互驗證，您也必須在 SLB MUX 裝置上有憑證。 當您使用 SCVMM 部署軟體負載平衡器時，SCVMM 會自動設定此憑證。

>[!IMPORTANT]
>在 [主機] 和 [SLB] 節點上，「信任的根憑證授權單位」憑證存放區不包含任何憑證，其中「發行至」與「發行者」不同。 如果發生這種情況，網路控制站與 southbound 裝置之間的通訊將會失敗。

網路控制卡和 slb mux 憑證必須彼此\(信任，slb mux 憑證的根憑證必須存在於網路控制站電腦的「信任的根憑證授權」存放區中，\)反之亦然。 當您使用自我\-簽署憑證時，SCVMM 會確保所需的憑證會出現在本機電腦的 [受信任的根憑證授權單位] 存放區中。




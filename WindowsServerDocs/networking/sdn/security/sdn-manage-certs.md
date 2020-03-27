---
title: 管理軟體定義網路的憑證
description: 當您在 Windows Server 2016 Datacenter 中部署軟體定義網路功能（SDN）時，您可以使用本主題來瞭解如何管理網路控制站 Northbound 和 Southbound 通訊的憑證。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: lizross
author: eross-msft
ms.date: 08/22/2018
ms.openlocfilehash: 20448e9bdef41f676bf5422c811be535bcf30ca2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317361"
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理軟體定義網路的憑證

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何管理網路控制站 Northbound 和 Southbound 通訊的憑證。當您在 Windows Server 2016 Datacenter 中部署軟體定義的網路功能 \(SDN\)，並使用 System Center Virtual Machine Manager \(SCVMM\) 做為 SDN 管理用戶端。

>[!NOTE]
>如需網路控制卡的總覽資訊，請參閱[網路控制](../technologies/network-controller/Network-Controller.md)卡。

如果您未使用 Kerberos 來保護網路控制站通訊，您可以使用 x.509 憑證來進行驗證、授權和加密。

Windows Server 2016 Datacenter 中的 SDN 支援自我\-簽署和憑證授權單位單位 \(CA\)簽署的 x.509 憑證。 本主題提供逐步指示，說明如何建立這些憑證，並將其套用至使用管理用戶端和網路裝置的 Southbound 通訊來保護網路控制站 Northbound 通道，例如軟體 Load Balancer \(SLB\)。
。
當您使用以憑證\-為基礎的驗證時，您必須在用來以下列方式使用的網路控制卡節點上註冊一個憑證。

1. 使用網路控制卡節點與管理用戶端之間的安全通訊端層 \(SSL\) 來加密 Northbound 通訊，例如 System Center Virtual Machine Manager。
2. 網路控制站節點與 Southbound 裝置和服務（例如 Hyper-v 主機和軟體負載平衡器）之間的驗證 \(SLBs\)。

## <a name="creating-and-enrolling-an-x509-certificate"></a>建立和註冊 x.509 憑證

您可以建立並註冊自我\-簽署的憑證，或 CA 所簽發的憑證。

>[!NOTE]
>當您使用 SCVMM 來部署網路控制站時，您必須指定在設定網路控制站服務範本期間用來加密 Northbound 通訊的 x.509 憑證。

憑證設定必須包含下列值。

- **RestEndPoint**文字方塊的值必須是網路控制站的完整功能變數名稱 \(FQDN\) 或 IP 位址。 
- **RestEndPoint**值必須與 x.509 憑證的 [主體名稱] \([一般名稱]、[CN\)]。

### <a name="creating-a-self-signed-x509-certificate"></a>建立自我\-簽署的 x.509 憑證

您可以針對單一\-節點和網路控制站的多個\-節點部署，遵循下列步驟來建立自我簽署的 x.509 憑證，並將私密金鑰匯出 \(以密碼\) 保護。

當您建立自我\-簽署的憑證時，您可以使用下列指導方針。

- 您可以針對 DnsName 參數使用網路控制器 REST 端點的 IP 位址，但不建議這麼做，因為它要求網路控制站節點都位於單一管理 \(子網內，例如在單一機架上\)
- 針對多節點 NC 部署，您指定的 DNS 名稱會成為網路控制站叢集的 FQDN \(DNS 主機 A 記錄會自動建立。\) 
- 針對單一節點網路控制站部署，DNS 名稱可以是網路控制站的主機名稱，後面接著完整功能變數名稱。

#### <a name="multiple-node"></a>多個節點

您可以使用[SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署的憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>單一節點

您可以使用[SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署的憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>建立 CA\-帶正負號的 x.509 憑證

若要使用 CA 建立憑證，您必須已使用 Active Directory 憑證服務 \(AD CS\)部署 \(PKI\) 的公開金鑰基礎結構。 

>[!NOTE]
>您可以使用協力廠商 Ca 或工具（例如 openssl）來建立要與網路控制站搭配使用的憑證，不過本主題中的指示僅適用于 AD CS。 若要瞭解如何使用協力廠商 CA 或工具，請參閱您所使用之軟體的說明文件。

使用 CA 建立憑證包含下列步驟。

1. 您或您組織的網域或安全性系統管理員設定憑證範本
2. 您或您組織的網路控制站系統管理員或 SCVMM 系統管理員向 CA 要求新的憑證。

#### <a name="certificate-configuration-requirements"></a>憑證設定需求

當您在下一個步驟中設定憑證範本時，請確定您設定的範本包含下列必要元素。

1. 憑證主體名稱必須是 Hyper-v 主機的 FQDN
2. 憑證必須放在本機電腦的個人存放區中（My – cert： \ localmachine\my）
3. 憑證必須同時具有伺服器驗證（EKU：1.3.6.1.5.5.7.3.1）和用戶端驗證（EKU：1.3.6.1.5.5.7.3.2）應用程式原則。

>[!NOTE]
>如果超\-V 主機上的 Personal \(My – cert： \ localmachine\my\) 憑證存放區中有一個以上的 x.509 憑證具有主體名稱（CN），做為主機完整功能變數名稱 \(FQDN\)，請確定 SDN 使用的憑證具有額外的自訂 [增強金鑰使用方法] 屬性和 [OID 1.3.6.1.4.1.311.95.1.1.1]。 否則，網路控制站與主機之間的通訊可能無法正常執行。

#### <a name="to-configure-the-certificate-template"></a>設定憑證範本
  
>[!NOTE]
>執行此程式之前，您應該先參閱憑證範本主控台中的憑證需求和可用的憑證範本。 您可以修改現有的範本，或建立現有範本的複本，然後修改範本的複本。 建議您建立現有範本的複本。

1. 在安裝 AD CS 的伺服器上，按一下伺服器管理員中的 [**工具**]，然後按一下 [**憑證授權單位**單位]。 [憑證授權單位單位] Microsoft Management Console \(MMC\) 隨即開啟。 
2. 在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下 [**憑證範本**]，然後按一下 [**管理**]。
3. [憑證範本] 主控台隨即開啟。 所有的憑證範本都會顯示在詳細資料窗格中。
4. 在詳細資料窗格中，按一下您想要複製的範本。
5.  按一下 [**動作**] 功能表，然後按一下 [**複製範本**]。 [範本**屬性**] 對話方塊隨即開啟。
6.  在 [範本**屬性**] 對話方塊的 [**主體名稱**] 索引標籤上，按一下 **[在要求中提供**]。 \(網路控制卡 SSL 憑證需要此設定。\)
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
2. 在主控台樹中，按一下 [**憑證 \(本機電腦\)** ]。 選取 [**個人**] 憑證存放區。
3. 在 [**動作**] 功能表上，指向 [所有工作]<strong>，然後按一下 [要求新憑證</strong>] 以啟動 [憑證註冊嚮導]。 按 [下一步]。
4. 選取**您的系統管理員**憑證註冊原則所設定的，然後按 **[下一步]** 。
5. 根據您在上一節\)中設定的 CA 範本，選取 [ **Active Directory 註冊原則**] \(。
6. 展開 [**詳細資料**] 區段，並設定下列專案。
   1. 請確定**金鑰使用**方式同時包含<strong>數位簽章 * * 和 * * 金鑰加密</strong>。
   2. 請確定**應用程式原則**同時包含**伺服器驗證**\(1.3.6.1.5.5.7.3.1\) 和**用戶端驗證**\(1.3.6.1.5.5.7.3.2\)。
7. 按一下 **[屬性]** 。
8. 在 [**主體**] 索引標籤的 [**主體名稱**] 中，于 [**類型**] 選取 [**一般名稱**]。 在 [值] 中，指定**網路控制卡 REST 端點**。
9. 按一下 [套用]，然後按一下 [確定]。
10. 按一下 **\[註冊\]** 。

在 [憑證] MMC 中，按一下 [個人] 存放區，以查看您已從 CA 註冊的憑證。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>將憑證匯出並複製到 SCVMM 程式庫

建立自我\-簽署或 CA\-簽署的憑證之後，您必須以 .pfx 格式匯出具有私密金鑰 \(的憑證，\) 且不含私密金鑰 \(來自憑證嵌入式管理單元的 64 .cer 格式\)。 

接著，您必須將兩個匯出的檔案複製到您在匯入 NC 服務範本時所指定的**ServerCertificate.cr**和**NCCertificate.cr**資料夾。

1. 開啟 [憑證] 嵌入式管理單元（certlm.msc），並在本機電腦的 [個人] 憑證存放區中尋找憑證。
2. 以滑鼠\-按右鍵憑證，按一下 [**所有**工作]，然後按一下 [**匯出**]。 \[憑證匯出精靈\] 隨即開啟。 按 [下一步]。
3. 選取 **[是**，匯出私密金鑰] 選項，然後按 **[下一步]** 。
4. 選擇 [**個人資訊交換-PKCS #12] （。PFX）** ，並接受預設值以**包含憑證路徑中的所有憑證**（如果可能的話）。
5. 為您要匯出的憑證指派使用者/群組和密碼，然後按 **[下一步]** 。
6. 在 [要匯出的檔案] 頁面上，流覽要放置匯出檔案的位置，並為其命名。
7. 同樣地，匯出中的憑證。CER 格式。 注意：要匯出到。CER 格式，取消核取 [是，匯出私密金鑰] 選項。
8. 複製。PFX 至 ServerCertificate.cr 資料夾。
9. 複製。CER 檔案至 NCCertificate.cr 資料夾。

當您完成時，請重新整理 SCVMM 程式庫中的這些資料夾，並確定您已複製這些憑證。 繼續進行網路控制站服務範本設定和部署。

## <a name="authenticating-southbound-devices-and-services"></a>驗證 Southbound 裝置和服務 

與主機和 SLB MUX 裝置的網路控制站通訊會使用憑證進行驗證。 與這些主機的通訊是透過 OVSDB 通訊協定，而與 SLB MUX 裝置通訊則是透過 WCF 通訊協定。

### <a name="hyper-v-host-communication-with-network-controller"></a>Hyper-v 主機與網路控制站的通訊

若要透過 OVSDB 與 Hyper-v 主機通訊，網路控制站必須向主機電腦出示憑證。 根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來與主機進行 southbound 通訊。

這就是 SSL 憑證必須設定用戶端驗證 EKU 的原因。 此憑證設定于「伺服器」 REST 資源上 \(Hyper-v 主機在網路控制卡中是以伺服器資源\)表示，而且可以藉由執行 Windows PowerShell 命令**NetworkControllerServer**來查看。

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

您可以從憑證授權單位單位 \(CA\)註冊憑證。 如果在主機電腦上找不到 CA 型憑證，SCVMM 會建立自我簽署的憑證，並在主機電腦上進行布建。

網路控制站和 Hyper-v 主機憑證必須彼此信任。 Hyper-v 主機憑證的根憑證必須存在於本機電腦的網路控制器 [受信任的根憑證授權單位] 存放區中，反之亦然。 

當您使用自我\-簽署的憑證時，SCVMM 會確保本機電腦的「信任的根憑證授權單位」存放區中有所需的憑證。 

如果您使用 Hyper-v 主機的 CA 型憑證，您必須確定 CA 根憑證存在於本機電腦的網路控制器的 [受信任的根憑證授權單位] 存放區中。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>與網路控制卡通訊的軟體 Load Balancer MUX

軟體 Load Balancer 多重通訊協定 \(MUX\)，而網路控制器會使用憑證進行驗證，透過 WCF 通訊協定進行通訊。

根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來 southbound 與 Mux 裝置的通訊。 此憑證會在 "NetworkControllerLoadBalancerMux" REST 資源上設定，而且可以藉由執行 Powershell Cmdlet **NetworkControllerLoadBalancerMux**來查看。

MUX REST 資源 \(部分\)的範例：

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

網路控制卡和 SLB MUX 憑證必須彼此信任 \(SLB MUX 憑證的根憑證必須存在於網路控制站電腦的「受信任的根憑證授權單位」存放區中，反之亦然，\)。 當您使用自我\-簽署的憑證時，SCVMM 會確保在本機電腦的 [受信任的根憑證授權單位] 存放區中有所需的憑證。




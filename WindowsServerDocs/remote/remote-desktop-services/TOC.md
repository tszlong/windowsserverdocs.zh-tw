# [遠端桌面服務](welcome-to-rds.md)
## [開始使用](rds-get-started.md)
### [RDS 中的新功能](rds-whats-new.md)
### [RDS 支援的設定](rds-supported-config.md)
### [Windows 10 VDI 支援的安全性設定](rds-vdi-supported-config.md)
### [規劃遠端桌面服務的海報](rds-poster.md)
### [遠端桌面服務代管合作夥伴](rds-hosting-partners.md)
## [規劃和設計](rds-plan-and-design.md)
### [隨處建置](rds-plan-build-anywhere.md)
### [遠端桌面工作負載](remote-desktop-workloads.md)
### [網路指導方針](network-guidance.md)
### [虛擬機器縮放指導方針](virtual-machine-recs.md)
### [隨處存取](rds-plan-access-from-anywhere.md)
### [高可用性](rds-plan-high-availability.md)
### [多重要素驗證](rds-plan-mfa.md)
### [安全資料存放區](rds-plan-secure-data-storage.md)
### [GPU 加速](rds-graphics-virtualization.md)
### [從任何裝置連線](rds-plan-connect-from-any-device.md)
### [選擇支付方式](rds-plan-choose-how-you-pay.md)
### [RDS 和 VDI 部署中的 Office 2016](/deployoffice/rds-office-vdi-rdsh)
#### [處理非持續環境中的 Outlook 搜尋](/deployoffice/rds-outlook-search)
#### [商務用 OneDrive 和 VDI 環境](/deployoffice/rds-onedrive-business-vdi)
### [桌面代管參考架構](Desktop-Hosting-Reference-Architecture.md)
#### [遠端桌面服務架構](Desktop-hosting-logical-architecture.md)
#### [桌面代管服務](Desktop-hosting-service.md)
#### [遠端桌面服務角色](rds-roles.md)
#### [桌面代管的 Azure 服務與考量](Azure-services-and-considerations-for-desktop-hosting.md)
## [建置和部署](rds-build-and-deploy.md)
### [使用 ARM 和 Azure Marketplace 部署概念證明 RDS 環境](rds-in-azure.md)
### [將遠端桌面服務部署移轉至 Windows Server 2016](migrate-rds-role-services.md)
### [移轉遠端桌面服務用戶端存取使用權 (RDS CAL)](migrate-rds-cals.md)
### [將遠端桌面服務部署升級至 Windows Server 2016](upgrade-to-rds.md)
#### [升級遠端桌面工作階段主機伺服器](upgrade-to-rdsh.md)
#### [升級遠端桌面虛擬主機伺服器](upgrade-to-rdvh.md)
### [部署遠端桌面服務基礎結構](rds-deploy-infrastructure.md)
### [建立和部署遠端桌面服務集合](rds-create-collection.md)
### [為您的使用者設定遠端桌面 Web 用戶端](clients/remote-desktop-web-client-admin.md)
### [為您的使用者設定電子郵件探索](rds-email-discovery.md)
### [授權遠端桌面部署](rds-client-access-license.md)
#### [啟用授權伺服器](rds-activate-license-server.md)
#### [在授權伺服器上安裝 RDS CAL](rds-install-cals.md)
#### [追蹤您的部署中使用的 CAL](rds-track-cals.md)
### [整合 Azure 服務](rds-integrate-with-azure.md)
#### [深入了解如何搭配 RDS 使用多重要素驗證](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
#### [將 Azure AD Domain Services 與您的 RDS 部署整合](rds-azure-adds.md)
#### [使用 Azure AD 應用程式 Proxy 發佈遠端桌面](/azure/active-directory/application-proxy-publish-remote-desktop)
### 延伸 RDS 環境以用於高可用性
#### [使用 RD 工作階段主機伺服器陣列向外延展現有的 RDS 集合](rds-scale-rdsh-farm.md)
#### [新增高可用性到 RD 連線代理人基礎結構](rds-connection-broker-cluster.md)
#### [新增高可用性到 RD Web 和 RD 閘道 Web 前端](rds-rdweb-gateway-ha.md)
#### [部署雙節點儲存空間直接存取檔案系統以儲存 UPD](rds-storage-spaces-direct-deployment.md)
#### [部署和管理個人工作階段桌面環境](rds-personal-session-desktops.md)
#### [為 RDS 建立 VM](rds-prepare-vms.md)
### [為您的 RDS 環境設定災害復原](rds-disaster-recovery.md)
#### [建立地理備援 RDS 部署](rds-multi-datacenter-deployment.md)
#### [為 RDS 設定 Azure Site Recovery](rds-disaster-recovery-with-azure.md)
##### [啟用 RDS 元件的災害復原](rds-enable-dr-with-asr.md)
##### [建立您的災害復原計劃](rds-disaster-recovery-plan.md)

## [執行並微調](rds-run-and-tune.md)
### [管理個人桌面工作階段集合](rds-manage-personal-collection.md)
### [建議的 VDI 桌面設定](rds-vdi-recommendations.md)
### [針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 2004 最佳化](rds-vdi-recommendations-2004.md)
### [針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 1909 最佳化](rds_vdi-recommendations-1909.md)
### [針對虛擬桌面基礎結構 (VDI) 角色最佳化 Windows 10 版本 1803](rds-vdi-recommendations-1803.md)
### [管理 RDS 集合中的使用者](rds-user-management.md)
### [在 Windows Server 上使用 PowerShell 自訂 RDS 標題「工作資源」](rds-work-resources.md)
### [使用效能計數器來診斷應用程式效能](rds-rdsh-performance-counters.md)

## 存取您的遠端桌面資源
### [可用的遠端桌面用戶端](clients/remote-desktop-clients.md)
### Windows 桌面用戶端
#### [開始使用 Windows 桌面用戶端](clients/windowsdesktop.md)
#### [適用於系統管理員的 Windows 桌面用戶端](clients/windowsdesktop-admin.md)
#### [Windows 桌面用戶端的新功能](clients/windowsdesktop-whatsnew.md)
#### [快速鍵](/windows/win32/termserv/terminal-services-shortcut-keys)
### Microsoft Store 用戶端
#### [開始使用 Microsoft Store 用戶端](clients/windows.md)
#### [Microsoft Store 用戶端的新功能](clients/windows-whatsnew.md)
### Android 用戶端
#### [開始使用 Android 用戶端](clients/remote-desktop-android.md)
#### [Android 用戶端中的新功能](clients/android-whatsnew.md)
### iOS 用戶端
#### [開始使用 iOS 用戶端](clients/remote-desktop-ios.md)
#### [iOS 用戶端中的新功能](clients/ios-whatsnew.md)
### macOS 用戶端
#### [開始使用 macOS 用戶端](clients/remote-desktop-mac.md)
#### [ 用戶端中的新功能](clients/mac-whatsnew.md)
### 網頁用戶端
#### [開始使用 Web 用戶端](clients/remote-desktop-web-client.md)
#### [Web 用戶端中的新功能](clients/web-client-whatsnew.md)
### 設定您電腦的遠端桌面
#### [支援的電腦](clients/remote-desktop-supported-config.md)
#### [授權您電腦的遠端桌面存取](clients/remote-desktop-allow-access.md)
#### [授與從網路外部存取電腦的權限](clients/remote-desktop-allow-outside-access.md)
#### [變更電腦上的 RD 接聽連接埠](clients/change-listening-port.md)
### 進階資訊
#### [比較用戶端功能](clients/remote-desktop-features.md)
#### [比較用戶端重新導向](clients/remote-desktop-app-compare.md)
#### [支援的遠端桌面 RDP 檔案設定](clients/rdp-files.md)
#### [遠端桌面 URI 配置](clients/remote-desktop-uri.md)
#### [遠端桌面用戶端常見問題集](clients/remote-desktop-client-faq.md)
#### [受控應用程式和桌面的隱私權設定](clients/remote-privacy-settings.md)
### 已知問題
#### [針對一般遠端桌面連線進行疑難排解](troubleshoot/rdp-error-general-troubleshooting.md)
#### [每個應用程式的認證限制](troubleshoot/credential-limit.md)
#### [用戶端無法連線並收到「類別未登錄」錯誤](troubleshoot/rdp-error-class-not-registered.md)
#### [用戶端無法連線並看到「沒有可用的授權」錯誤](troubleshoot/rdp-error-no-licenses-available.md)
#### [使用者無法驗證，或必須驗證兩次](troubleshoot/cannot-authenticate-or-must-authenticate-twice.md)
#### [在連線時收到「遠端桌面服務目前忙碌中」錯誤](troubleshoot/remote-desktop-service-currently-busy.md)
#### [遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段](troubleshoot/rdp-client-disconnects-cannot-reconnect-same-session.md)
#### [遠端膝上型電腦從無線網路中斷連線](troubleshoot/remote-laptop-disconnects-wireless-network.md)
#### [遠端桌面連線期間效能不佳或應用程式發生問題](troubleshoot/poor-performance-or-application-problems.md)
#### [RemoteApp 案例中的輸入方法編輯器問題](troubleshoot/input-method-editor-error.md)

## [其他資源](rds-get-support.md)

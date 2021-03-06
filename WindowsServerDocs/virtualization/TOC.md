# [虛擬化](virtualization.yml)

## [受防護網狀架構與受防護的 VM](../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)

## [Hyper-V](hyper-v/Hyper-V-on-Windows-Server.md)
### [技術概觀](hyper-v/Hyper-V-Technology-Overview.md)
### [Hyper-V 中的新功能](hyper-v/What-s-new-in-Hyper-V-on-Windows.md)
### [系統需求](hyper-v/System-requirements-for-Hyper-V-on-Windows.md)
### [支援的 Windows 客體作業系統](hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)
### [支援的 Linux 和 Free BSD VM](hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
#### [CentOS 和 Red Hat Enterprise Linux VM](hyper-v/Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)
#### [Debian VM](hyper-v/Supported-Debian-virtual-machines-on-Hyper-V.md)
#### [Oracle Linux VM](hyper-v/Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)
#### [SUSE VM](hyper-v/Supported-SUSE-virtual-machines-on-Hyper-V.md)
#### [Ubuntu VM](hyper-v/Supported-Ubuntu-virtual-machines-on-Hyper-V.md)
#### [FreeBSD VM](hyper-v/Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
#### [Linux 和 FreeBSD VM 的功能描述](hyper-v/Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
#### [執行 Linux 的最佳做法](hyper-v/Best-Practices-for-running-Linux-on-Hyper-V.md)
#### [執行 FreeBSD 的最佳做法](hyper-v/Best-practices-for-running-FreeBSD-on-Hyper-V.md)
### [世代與客體各自的功能相容性](hyper-v/Hyper-V-feature-compatibility-by-generation-and-guest.md)
### [開始使用](hyper-v/get-started/Get-started-with-Hyper-V-on-Windows.md)
#### [安裝 Hyper-V](hyper-v/get-started/Install-the-Hyper-V-role-on-Windows-Server.md)
#### [建立虛擬交換器](hyper-v/get-started/create-a-virtual-switch-for-Hyper-V-virtual-machines.md)
#### [建立虛擬機器](hyper-v/get-started/create-a-virtual-machine-in-Hyper-V.md)
### [規劃](hyper-v/plan/Plan-Hyper-V-on-Windows-Server.md)
#### [VM 世代](hyper-v/plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)
#### [網路功能](hyper-v/plan/plan-hyper-v-networking-in-windows-server.md)
#### [延展性](hyper-v/plan/plan-hyper-v-scalability-in-windows-server.md)
#### [安全性](hyper-v/plan/plan-hyper-v-security-in-windows-server.md)
#### [GPU 加速](hyper-v/plan/plan-for-gpu-acceleration-in-windows-server.md)
#### [不同的裝置指派](hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)
### [部署](hyper-v/deploy/Deploy-Hyper-V-on-Windows-Server.md)
#### [匯出和匯入虛擬機器](hyper-v/deploy/Export-and-import-virtual-machines.md)
#### [設定可進行即時移轉的主機](hyper-v/deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)
#### [升級虛擬機器版本](hyper-v/deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)
#### [使用 DDA 部署圖形裝置](hyper-v/deploy/deploying-graphics-devices-using-dda.md)
#### [使用 RemoteFX vGPU 部署圖形裝置](hyper-v/deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
#### [使用 DDA 部署存放裝置](hyper-v/deploy/deploying-storage-devices-using-dda.md)

### [管理](hyper-v/manage/Manage-Hyper-V-on-Windows-Server.md)
#### [設定 Hyper-V VM 的持續性記憶體裝置](hyper-v/manage/persistent-memory-cmdlets.md)
#### [選擇標準或生產檢查點](hyper-v/manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)
#### [建立 VHD 集合](hyper-v/manage/Create-VHDSet-file.md)
#### [啟用或停用檢查點](hyper-v/manage/Enable-or-disable-checkpoints-in-Hyper-V.md)
#### [使用 Hyper-V 管理員管理主機](hyper-v/manage/Remotely-manage-Hyper-V-hosts.md)
#### [管理主機 CPU 資源控制項](hyper-v/manage/manage-hyper-v-minroot-2016.md)
#### [使用 VM CPU 群組](hyper-v/manage/manage-hyper-v-cpugroups.md)
#### [管理 Hypervisor 排程器類型](hyper-v/manage/manage-hyper-v-scheduler-types.md)
#### [關於 Hyper-V 排程器類型選項](hyper-v/manage/about-hyper-v-scheduler-type-selection.md)
#### [管理整合服務](hyper-v/manage/Manage-Hyper-V-integration-services.md)
#### [使用 PowerShell Direct 管理 Windows VM](hyper-v/manage/Manage-Windows-virtual-machines-with-powershell-direct.md)
#### [設定 Hyper-V 複本](hyper-v/manage/Set-up-Hyper-V-Replica.md)
#### [啟用 Intel 效能監視硬體](hyper-v/manage/Performance-Monitoring-Hardware.md)
#### [使用即時移轉移動 VM](hyper-v/manage/Live-migration-overview.md)
##### [即時移轉概觀](hyper-v/manage/Live-migration-overview.md)

##### [設定可進行即時移轉的主機](hyper-v/deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md) 
##### [在不使用容錯移轉叢集的情況下使用即時移轉](hyper-v/manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md)


### [Hyper-V 效能調整](../administration/performance-tuning/role/hyper-v-server/index.md)
## [Hyper-V 虛擬交換器](hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)
### [遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)](hyper-v-virtual-switch/rdMA-and-Switch-Embedded-Teaming.md)
### [管理 Hyper-V 虛擬交換器](hyper-v-virtual-switch/Manage-Hyper-V-Virtual-Switch.md)
#### [設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定](hyper-v-virtual-switch/Configure-and-View-VLAN-Settings-on-Hyper-V-Virtual-Switch-Ports.md)
#### [使用延伸連接埠存取控制清單建立安全性原則](hyper-v-virtual-switch/create-Security-Policies-with-extended-Port-Access-Control-lists.md)

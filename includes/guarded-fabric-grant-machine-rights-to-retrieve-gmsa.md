1. 具有目錄服務的系統管理員，將您的新節點的電腦帳戶新增至包含您所有的 HGS 伺服器，以允許這些伺服器使用的 HGS gMSA 帳戶授與權限的安全性群組。

2. 重新啟動新的節點，以取得新的 Kerberos 票證中該安全性群組包含電腦的成員資格。 重新啟動完成之後，使用屬於本機 administrators 群組的電腦上的網域身分識別登入。

3. 在節點上安裝 HGS 群組受控服務帳戶。

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```

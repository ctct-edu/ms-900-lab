---
lab:
  title: 'ラボ : Windows Server でのネットワーク インフラストラクチャ サービスの実装と構成'
  module: 'Module 7: Network Infrastructure services in Windows Server'
---

# <a name="lab-implementing-and-configuring-network-infrastructure-services-in-windows-server"></a>Lab7a : DHCP の展開と構成

## <a name="scenario"></a>シナリオ

Contoso の リサーチ グループの下位部門には、約 50 人のユーザーのみが存在する別のオフィスがあります。 現在、下位部門のユーザーは、各自 IP アドレスを手動で構成しているため、DHCP の使用を開始したいと考えています。 DHCP は、リサーチグループ サイトのスコープを持つ **SEA-SVR1** にインストールします。 さらに、**SEA-DC1** での高可用性を実現するため、DHCP フェールオーバーを構成します。

## <a name="objectives"></a>目標とタスク

このラボを完了すると、次のことができるようになります。

- DHCP を展開および構成する。

この演習の主なタスクは次のとおりです。

1. DHCP 役割をインストールします。
1. DHCP サーバーを承認します。
1. スコープを作成します。
1. DHCP フェールオーバーを構成します。
1. DHCP 機能を確認します

## <a name="estimated-time-60-minutes"></a>予想所要時間: 20 分

## <a name="architecture"></a>アーキテクチャの図

![AZ-800_Lab07_architecture1](./media/AZ-800_Lab07_architecture1.png)

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **SEA-DC1、SEA-SVR1、SEA-ADM1を使用します。**

1. **SEA-ADM1** を選択します。
1. 次の資格情報を使用してサインインします。

   - ユーザー名: **Administrator**
   - パスワード: **Pa55w.rd**
   - ドメイン: **CONTOSO**

このラボでは、仮想マシンのみを使用します。



### <a name="task-1-install-the-dhcp-role"></a>タスク 1: DHCP 役割をインストールする

1. SEA-ADM1で、Microsoft Edge を起動し、Windows Admin Center の`https://SEA-ADM1.contoso.com ` を参照します。

1. セキュリティダイアログボックスが表示されたら、次の資格情報を入力し、**[OK]** をクリックします。

   | 項目       | 値                         |
   | ---------- | -------------------------- |
   | ユーザー名 | **CONTOSO\\Administrator** |
   | パスワード | **Pa55w.rd**               |

   ※Windows Admin Center に接続したら、必要に応じ、右上の歯車マーク(設定) から言語を日本語に変更できます。

1. **[すべての接続]** ページで、 **[ + 追加]** をクリックします。

1. **[リソースの追加または作成]** ページの **[サーバー]** タイルで、 **[追加]** をクリックします。

1. **[サーバー名]** のテキスト ボックスに、 **sea-svr1.contoso.com** と入力します。

1. **[資格情報が必要です]** と表示されたら、以下のオプションを設定し、 **[資格情報を含めて追加]** をクリックします。

   | 設定                                 | 値                        |
   | ------------------------------------ | ------------------------- |
   | この接続では別のアカウントを使用する | **チェックする**          |
   | ユーザー名                           | **Contoso\Administrator** |
   | パスワード                           | **Pa55w.rd**              |

   <img src="./media/AZ-800_Lab7_01.png" alt="AZ-800_Lab7_01" style="zoom:67%;" />

7. すべてのサーバー一覧から、**sea-svr1.contoso.com** を選択し、 左ナビゲーションペインの **[ツール]** を下にスクロールし、 **[役割と機能]** をクリックします。

<img src="./media/AZ-800_Lab7_02.png" alt="AZ-800_Lab7_02" style="zoom:67%;" />

8. **[役割と機能]** 一覧から、 **[DHCP サーバー]** のチェックボックスを選択し、**[ + インストール]** をクリックします。

<img src="./media/AZ-800_Lab7_03.png" alt="AZ-800_Lab7_03" style="zoom:80%;" />

9. **[役割と機能のインストール]** ウィンドウが表示されたら、**[はい]** を選択します。

※ **[必要に応じてサーバーを自動的に再起動する]** のチェックボックスはオフのままで構いません。

10. Microsoft Edge で表示されている Windows Admin Center のページを更新します。

> **注 : 資格情報の入力を求められたら、以下を設定して [続行] をクリックしてください。(これらの資格情報をすべての接続でしようします。のチェックボックスはオフのままで構いません。)**
>
> | 設定                                 | 値                        |
> | ------------------------------------ | ------------------------- |
> | この接続では別のアカウントを使用する | **チェックを入れる**      |
> | ユーザー名                           | **Contoso\Administrator** |
> | パスワード                           | **Pa55w.rd**              |



11. Windows Admin Center の **[すべての接続]** を展開し、 **[All Connections(すべての接続)]** を選択した後、 **sea-svr1.contoso.com** に再度接続します。

![AZ-800_Lab7_04](./media/AZ-800_Lab7_04.png)

12.  **sea-svr1.contoso.com** ページの **[ツール]** 一覧から **[ DHCP ]** を選択し、詳細ウィンドウで **[インストール]** を選択して **DHCP PowerShell** ツールをインストールします。

<img src="./media/AZ-800_Lab7_05.png" alt="AZ-800_Lab7_05" style="zoom: 67%;" />

13. DHCP PowerShell ツールがインストールされたという通知が確認できてから、タスク2に進んでください。

> **注: 通知メッセージが表示されない場合は、Windows Admin Center の右上にあるベルのマークをクリックして通知メッセージの一覧を確認できます。**

### <a name="task-2-authorize-the-dhcp-server"></a>タスク 2: DHCP サーバーを承認する

1. **SEA-ADM1** で、**[スタート]** メニューから、**[Server Manager (サーバー マネージャー)]** を選択します。

1. Server Manager が起動したら、右上の旗のマーク **[通知]** をクリックし、**[Complete DHCP configuration (DHCP 構成の完了)]** を選択します。

   <img src="./media/AZ-800_Lab7_06.png" alt="AZ-800_Lab7_06" style="zoom:80%;" />

3. DHCP インストール後の構成ウィザードウィンドウの **[Description (説明)]** 画面で、**[Next (次へ)]** をクリックします。

   <img src="./media/AZ-800_Lab7_07.png" alt="AZ-800_Lab7_07" style="zoom: 67%;" />

4.  **[Authorization (承認)]** 画面で、**[User the following user's credentials]** にチェックが入り、**CONTOSO\Administrator** が選択されていることを確認し、 [**コミット**] をクリックします。

   <img src="./media/AZ-800_Lab7_08.png" alt="AZ-800_Lab7_08" style="zoom:67%;" />

5.  **[Summary (概要)]** 画面が表示されたら、**[close]** をクリックしてウィザードを終了します。

   <img src="./media/AZ-800_Lab7_09.png" alt="AZ-800_Lab7_09" style="zoom:67%;" />

### <a name="task-3-create-a-scope"></a>タスク 3: スコープを作成する

1. **SEA-ADM1**  の Windows Admin Center で、**sea-svr1.contoso.com** に接続し、左ナビゲーションペインの一覧から **[DHCP]** を選択します。

1. **[ DHCP ]** ページで、 **[ + 新しいスコープ]** を選択します。

   <img src="./media/AZ-800_Lab7_10.png" alt="AZ-800_Lab7_10" style="zoom: 67%;" />

3. **[新しいスコープの作成]** ウィンドウで、次の設定を指定し、 **[作成]** をクリックします。特に指示がないものは規定値のままで構いません。

| 設定                                                         | 値                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| プロトコル(Protocol)                                         | **IPv4**                                               |
| 名前 (Name)                                                  | **ContosoClients**                                     |
| 開始 IP アドレス (Starting IP Address)                       | **10.100.150.50**                                      |
| 終了 IP アドレス (End IP Address)                            | **10.100.150.254**                                     |
| DHCP クライアントのサブネット マスク (DHCP client subner mask) | **255.255.255.0**                                      |
| ルーター (default gateway)                                   | **[＋ 追加] をクリックし、 10.100.150.1 を設定します** |
| DHCP クライアントのリース期間 (Lease duration for DHCP clients) | **[Days] に [4] と設定してください**                   |



4. **SEA-ADM1** のサーバー マネージャーに切り替え、 **[Tools (ツール)]  - [ DHCP ]** の順に選択します。

<img src="./media/AZ-800_Lab7_11.png" alt="AZ-800_Lab7_11" style="zoom:80%;" />

5. **[DHCP]** の **[Actions (操作)]** ウィンドウで、**[More Actions (その他の操作)]** を選択し、 **[Manage Authorized Servers (承認済みサーバーの管理)]** を選択します。

![AZ-800_Lab7_12](./media/AZ-800_Lab7_12.png)

6. **[Manage Authorized Servers (承認済みサーバーの管理)]** ウィンドウで **[Refresh (更新)]** を選択し、**Authorized DHCP Servers の一覧**に、 **sea-dc1.contoso.com** と **sea-svr1.contoso.com** の両方が表示されていることを確認してから、 **[Manage Authorized Servers (承認済みサーバーの管理)]** ウィンドウを閉じます。

![AZ-800_Lab7_13](./media/AZ-800_Lab7_13.png)

7. **[DHCP]** の **[Actions (操作)]** ウィンドウで、**[More Actions (その他の操作)]** を選択し、**[Add Server (サーバーの追加)]** を選択します。

![AZ-800_Lab7_14](./media/AZ-800_Lab7_14.png)

8. **[Add Server (サーバーの追加)]** ダイアログ ボックスで、**[This authorized DHCP server (承認された DHCP サーバー)]** のラジオボタンを選択、**[ sea-svr1.contoso.com ]** を選択してから、**[ OK ]** をクリックします。

![AZ-800_Lab7_15](./media/AZ-800_Lab7_15.png)

9. DHCP ウィンドウで、**[172.16.10.12] - [IPv4] Scope - [10.100.150.0] ContosoClients** の順に展開し、 **[Scope Options]** を選択します。

![AZ-800_Lab7_16](./media/AZ-800_Lab7_16.png)

10. **[Actions (操作)]** ウィンドウで、**[More Actions (その他の操作)]** を選択し、**[Configure Options (オプションの構成)]** を選択します。

![AZ-800_Lab7_17](./media/AZ-800_Lab7_17.png)

11. **[Scope Options (スコープ オプション)]** ダイアログ ボックスで、**[ 006 DNS Server]** チェックボックスをオンにし、 **[Data entry]**  に以下を設定した後、[Add (追加)]をクリックしてから、 [OK] を選択します。

| 設定                     | 値                      |
| ------------------------ | ----------------------- |
| Server name (サーバー名) | **sea-dc1.contoso.com** |
| IP address (IPアドレス)  | **172.16.10.10**        |

![AZ-800_Lab7_18](./media/AZ-800_Lab7_18.png)



### <a name="task-4-configure-dhcp-failover"></a>タスク 4: DHCP フェールオーバーを構成する

1. **SEA-ADM1** の **[ DHCP ]** ウィンドウで **[ IPv4 ]** を選択し、**[Actions (操作)] - [More Actions (その他の操作)]** の順に展開してから、**[Configure Failover (フェールオーバーの構成)]** を選択します。

   ![AZ-800_Lab7_19](./media/AZ-800_Lab7_19.png)

2. **[Configure Failover (フェイルオーバーの構成)]** ウィンドウで、**[Select all (すべて選択)]** チェックボックスにチェックが入っていることを確認し、**[Next (次へ)]** を選択します。

![AZ-800_Lab7_20](./media/AZ-800_Lab7_20.png)

3. **[Specify the partner server to use for failover (フェールオーバーに使用するパートナー サーバーの指定)]** 画面で、 **[Add Server (サーバーの追加)]** をクリックします。

![AZ-800_Lab7_21](./media/AZ-800_Lab7_21.png)

4. **[Add Server (サーバーの追加)]** ダイアログ ボックスで、**[This authorized DHCP server (承認された DHCP サーバー)]** を選択し、**[ sea-dc1.contoso.com ]** を選択してから、**[ OK ]** をクリックします。

![AZ-800_Lab7_22](./media/AZ-800_Lab7_22.png)

5. **[Specify the partner server to use for failover (フェールオーバーに使用するパートナー サーバーの指定)]** 画面に戻り、 **[パートナー サーバー]** ドロップダウン リストに **sea-dc1** が表示されていることを確認し、**[Next (次へ)]** を選択します。

![AZ-800_Lab7_23](./media/AZ-800_Lab7_23.png)

6. **[Create a new failover relationship (新しいフェールオーバー関係の作成)]** 画面で、次の情報を入力し、 **[Next (次へ)]** を選択します。指示がないものは規定値のままで構いません。

| 設定                                                         | 項目                               |
| ------------------------------------------------------------ | ---------------------------------- |
| Relationship Name (レプリケーションの名前)                   | **SEA-SVR1 to SEA-DC1**            |
| Maximum Client Lead Time (クライアントの最大リードタイム)    | **1 hours (1時間)**                |
| Mode (モード)                                                | **Hot standby (ホットスタンバイ)** |
| Role Partner Server (パートナー サーバーの役割)              | **Standby**                        |
| Address reserved for standby server (スタンバイ サーバー用に予約されたアドレス) | **5％**                            |
| State Switchover Interval (状態切り替え間隔)                 | **チェックオフ(無効)**             |
| Enable Message Authentication (メッセージ認証を有効にする)   | **チェックオン(有効)**             |
| Shared Secret (秘密共有)                                     | **DHCP-Failover**                  |

![AZ-800_Lab7_24](./media/AZ-800_Lab7_24.png)



7. **[Configure Failover]** ウィザード画面が表示されたら、 **[Finish (終了)]** をクリックします。

   **※結果が [Successful] と表示されることを確認したら [close] で閉じて構いません。**

![AZ-800_Lab7_25](./media/AZ-800_Lab7_25.png)



8. **[ DHCP ]** を選択し、**[Actions (操作)] - [More Actions (その他の操作)]** の順に展開してから、**[Add Server(サーバーの追加)]** を選択します。

![AZ-800_Lab7_26](./media/AZ-800_Lab7_26.png)



9. **[ Add Server (サーバーの追加)]** ダイアログ ボックスで、**[This authorized DHCP server (承認された DHCP サーバー)]** を選択し、**[sea-dc1.contoso.com]** を選択して、**[ OK ]** をクリックします。

10. **DHCP** ウィンドウで、**sea-dc1 ノード** を展開し、 **IPv4** を選択して、2 つのスコープがリストされていることを確認します。

![AZ-800_Lab7_27](./media/AZ-800_Lab7_27.png)

11. **[Scope [172.16.0.0] Contoso]** を選択し、**[Actions (操作)]** ウィンドウで **[More Actions (その他の操作)]** を選択し、**[Configure Failover (フェールオーバーの構成)]** を選択します。

12. **[Configure Failover (フェールオーバーの構成)]** ウィンドウで、**[Select all]** にチェックが入っていることを確認したら、 **[ Next (次へ)]** をクリックします。

13. **[ Specify the partner server to use for failover (フェールオーバーに使用するパートナー サーバーを指定してください)]** 画面の **[Partner Server (パートナー サーバー)] のプルダウンから**に **172.16.10.12** を選択します。 **[Reuse existing failover relationships configured with this server (if any exist) (このサーバーで構成された既存のフェールオーバー関係を再利用する (存在する場合))]** チェックボックスにチェックが入っていることを確認して、 **[Next (次へ)]** をクリックします。

![AZ-800_Lab7_28](./media/AZ-800_Lab7_28.png)

14. **[Select from failover relationships which are already configured on this server  (このサーバーで既に構成されているフェールオーバー関係から選択する)]** 画面で、**[Next (次へ)]** を選択し、**[Finish (完了)]**をクリックします。

15. **[Configure Failover]** ウィザード画面が表示されたら、 **[Finish (終了)]** をクリックします。

   **※結果が [Successful] と表示されることを確認したら [close] で閉じて構いません。**

16. **172.16.10.12 (sea-svr1)** の下で **IPv4** を選択し、2つのスコープが表示されていることを確認します。

> **注 : スコープが確認できない場合は、F5キーを押して更新してください。**

![AZ-800_Lab7_29](./media/AZ-800_Lab7_29.png)

17. **sea-dc1** のサーバー エントリを右クリックし、 **[ Authorize(承認する) ]** をクリックします。

![AZ-800_Lab7_30](./media/AZ-800_Lab7_30.png)

### <a name="task-5-verify-dhcp-functionality"></a>タスク 5: DHCP 機能を確認する

1. **SEA-ADM1** の **[スタート]** を選択し、**歯車マーク(settings)** を選択します。

   ![AZ-800_Lab7_31](./media/AZ-800_Lab7_31.png)

2. **[Windows Settings (設定)]** ウィンドウで、**[Network & Internet (ネットワークとインターネット)]** を選択し、**[Network and Sharing Center (ネットワークと共有センター)]** を選択します。

![AZ-800_Lab7_32](./media/AZ-800_Lab7_32.png)



3. **[Network and Sharing Center (ネットワークと共有センター)]** で、 **[Ethernet (イーサネット)]** を選択した後、 **[Properties (プロパティ)]** を選択します。

![AZ-800_Lab7_33](./media/AZ-800_Lab7_33.png)

![AZ-800_Lab7_34](./media/AZ-800_Lab7_34.png)

4. **[Ethernet Properties (イーサネットのプロパティ)]**  ダイアログ ボックスで、**[Internet Protocol Version 4 (TCP/IPv4) ]** を選択した後、**[Properties (プロパティ)]** をクリックします。

![AZ-800_Lab7_35](./media/AZ-800_Lab7_35.png)

5. **[Internet Protocol Version 4 (TCP/IPv4) Properties]** ダイアログ ボックスで、**[ Obtain an IP address automatically (IP アドレスを自動的に取得する)]**  、**[Obtain DNS server address automatically(DNS サーバーのアドレスを自動的に取得する)]** を選択した後、**[ OK ]** をクリックします。

![AZ-800_Lab7_36](./media/AZ-800_Lab7_36.png)

6. **[Ethernet Properties (イーサネットのプロパティ)]**  ダイアログ ボックスを、 [Close] で閉じます。

7. **[Ethernet Status (イーサネット ステータス)]** ウィンドウで  **[Details (詳細)]** を選択します。

![AZ-800_Lab7_37](./media/AZ-800_Lab7_37.png)

8. **[Network Connection Details (ネットワーク接続の詳細)]** ダイアログ ボックスで、DHCP が有効(Enable) になっていること、IP アドレスが取得されていること、および **SEA-SVR1 (172.16.10.12)** DHCP サーバーがリースを発行したことを確認します。

![AZ-800_Lab7_38](./media/AZ-800_Lab7_38.png)

9. 確認出来たら、[Close (閉じる)] を選択して、**[Ethernet Status (イーサネット ステータス)]** ウィンドウに戻ります。

10. **SEA-ADM1** の DHCPウィンドウに戻り、**[172.16.10.12] - [IPv4ノード]** の順に展開した後、**Scope  [172.16.0.0] Contosoノード**を展開して、 **[Address Leases (アドレス リース)]** を選択します。

![AZ-800_Lab7_39](./media/AZ-800_Lab7_39.png)

11. **SEA-ADM1.contoso.com リース**を表すエントリがあることを確認します。

12. **sea-dc1** を展開し、**[IPv4] - [スコープ [172.16.0.0] Contoso]** の順に展開してから、 **[Address Leases]** を選択します。

13. SEA-ADM1.contoso.com リースを表すエントリがあることが確認できます。

![AZ-800_Lab7_40](./media/AZ-800_Lab7_40.png)



14.  **sea-svr1 (172.16.10.12)**  を選択し、**[Actions (アクション)]** ウィンドウで **[More Actions (その他のアクション)]** を選択し、**[All tasks (すべてのタスク)]** を選択してから、**[Stop (停止)]** を選択します。

![AZ-800_Lab7_41](./media/AZ-800_Lab7_41.png)



15.  **[Ethernet Status (イーサネット ステータス)]**  ウィンドウに戻り、 **[(Disable) 無効]** を選択します。

![AZ-800_Lab7_42](./media/AZ-800_Lab7_42.png)



16. **[Network and Sharing Center (ネットワークと共有センター)]** ウィンドウに戻り、**[Change adapter settings (アダプターの設定を変更する)] - [Ethernet]** の順に選択し、**Ethernet** を右クリックして  **[Enable(有効)]** をクリックします。

![AZ-800_Lab7_43](./media/AZ-800_Lab7_43.png)

![AZ-800_Lab7_45](./media/AZ-800_Lab7_45.png)

17. **[有効なイーサネット接続]** をダブルクリックして、ステータス ウィンドウを表示します。

18. **[Ethernet Status]** ウィンドウで、**[Details (詳細)]** を選択します。

![AZ-800_Lab7_47](./media/AZ-800_Lab7_47.png)



19. **[Network Connection Details (ネットワーク接続の詳細)]** ダイアログ ボックスで、DHCP が有効 (Enable) になり、IP アドレスが取得されていること、 **SEA-DC1 (172.16.10.10)**  DHCP サーバーがリースを発行したことを確認します。

![AZ-800_Lab7_48](./media/AZ-800_Lab7_48.png)



20. **[close]** をクリックして、**[Ethernet Status]** ウィンドウに戻ります。

21. **[Ethernet Status (イーサネット ステータス)]** ウィンドウで、 **[Properties (プロパティ)]** をクリックします。

22. **[Ethernet Properties (イーサネットのプロパティ)]**  ダイアログ ボックスで、**[Internet Protocol Version 4 (TCP/IPv4)]** を選択した後、**[Properties (プロパティ)]** をクリックします。

23. **[Internet Protocol Version 4 (TCP/IPv4)]** ダイアログ ボックスで、 **[Use the following IP address (次の IP アドレスを使用する)]** を選択し、次の設定を指定します。

| 設定                                     | 値               |
| ---------------------------------------- | ---------------- |
| IP address (IP アドレス)                 | **172.16.10.11** |
| Subnet mask (サブネットマスク)           | **255.255.0.0**  |
| Default gateway (デフォルトゲートウェイ) | **172.16.10.1**  |

**[Use the following DNS server addresses (次の DNS サーバー アドレスを使用する)]** を選択し、**[Preferred DNS server (優先 DNS サーバー)]** を **172.16.10.10** に設定して、**[ OK ]** をクリックします。

![AZ-800_Lab7_49](./media/AZ-800_Lab7_49.png)



**※ [Ethernet Status] ウィンドウは次の演習で必要になります。閉じずに次の演習に進んでください。**



結果

このラボを完了すると、 DHCPサーバーを構築し、2台のDHCPサーバーでフェールオーバーが実装できたことが確認できます。


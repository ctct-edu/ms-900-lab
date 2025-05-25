# <a name="lab-answer-key-Microsoft 365-organization-settings"></a>Lab01: Microsoft 365 の組織設定

## <a name="scenario"></a>シナリオ

あなたは、Microsoft 365 テナントを契約した直後に行う組織の設定を確認することにしました。

## <a name="objectives"></a>目標とタスク

このラボを完了すると、次のことができるようになります。

- Microsoft 365 テナントの設定を確認する。

- Microsoft 365 管理センターの組織プロファイルの設定を変更する。

  

この演習の主なタスクは次のとおりです。

1. Microsoft 365 ポータルの日本語化
2. Microsoft 365 管理センターで組織プロファイルを設定する

## <a name="estimated-time-45-minutes"></a>予想所要時間: 20 分



### <a name="task-1-Japanese-localization-of-Microsoft 365-portal"></a>タスク 1: Microsoft 365 ポータルの日本語化

1. InPrivate ウィンドウ(Microsoft Edge) もしくは、シークレットウィンドウ (Google Chrome) で、Microsoft 365 ポータル (https://portal.office.com/) にアクセスし、事前に取得した資格情報でサインインします。

   | 項目           | 値                                                           |
   | -------------- | ------------------------------------------------------------ |
   | **ユーザーID** | `admin@XXXXXXXXXXX.onmicrosoft.com`<br />@マーク以降のXXXXXXXXXは各自異なります。 |
   | **パスワード** | 事前に取得したパスワード                                     |

1.  **[サインインの状態を保持しますか]** ページでは、 **[いいえ]** をクリックします 。

1.  **[Welcome to your new Office,Mod]** ページが表示された場合は、右側に表示される **[>]** ボタンを3回クリックします。(表示されない場合は、そのままMicrosoft 365 ポータルが表示されます)

1. ポータル画面が表示されたら、左下の歯車マーク **[Settings]** をクリックします。

   ![lab1-1](./media/lab1-1.png)

1. **[Setting]** の一覧から、 **[Language and time zone]** の **[Change your language]** をクリックします。

   <img src="./media/lab1-2.png" alt="lab1-2" style="zoom: 67%;" />

6. **[My Account]** の **[Settings ＆ Privacy]** ページが表示されます。

7. **[Language]** セクションの **[Display Language]** をクリックして、 **[Change display language]** をクリックします。

<img src="./media/lab1-3.png" alt="lab1-3" style="zoom:80%;" />

8. **[Language]** ページで **[Japanese]** を選択し、 **[Select]** をクリックします。

9. **[Display language and regional format will be updated to Japanese]** とメッセージが表示されたら、 **[Update]** をクリックします。

![lab1-4](./media/lab1-4.png)

10. **[表示言語と地域設定が Japanese に更新されました。新しい言語を完全に適用するには、サインアウトする必要があります。]** とメッセージが表示されたら、 **[キャンセル]** をクリックします。

<img src="./media/lab1-5.png" alt="lab1-5" style="zoom:80%;" />

11. **[設定およびプライバシー]** ページで、 **[地域]** の **[タイムゾーン] - [タイムゾーンの変更]** をクリックします。

<img src="./media/lab1-6.png" alt="lab1-6" style="zoom:80%;" />

12. **[ (UTC ＋ 09:00)大阪、札幌、東京]** を選択して、 **[選択]** をクリックします。

13. Microsoft 365 ポータル の右上にあるイニシャルのアイコンをクリックしてから、 **[sign out]** をクリックします。

![lab1-7](./media/lab1-7.png)

14. **[どのアカウントをサインアウトしますか?]** というメッセージが表示されたら、サインインしていたアカウントを選択します。

15. サインアウトが完了したら、一度ブラウザーを×で閉じます。

16. 再度、InPrivate ウィンドウ(Microsoft Edge) もしくは、シークレットウィンドウ (Google Chrome) で、Microsoft 365 管理センター (https://admin.microsoft.com) にアクセスし、Lab0 で取得した資格情報でサインインします。

17. **[サインインの状態を維持しますか]** ページが表示された場合は、 **[いいえ]** をクリックします 。

18. **[Microsoft 365 管理センター]** のメニュー表記が日本語となっていることが確認できます。

19. **[Microsoft 365 管理センター]** が日本語に変更されていない場合は、一度サインアウトし、16の手順を繰り返します。

　　**※日本語化されたことが確認出来たら、タスク2に進んでください。**



### <a name="task-1-Japanese-localization-of-Microsoft 365-portal"></a>タスク 2: Microsoft 365 管理センターで組織プロファイルを設定する

1. Microsoft 365 管理センターの左ペインから、 **[・・・すべて表示]** をクリックします。

   <img src="./media/lab1-8.png" alt="lab1-8" style="zoom:80%;" />

1. **[設定] - [組織設定]** をクリックしてから、 **[組織のプロファイル(Organization profile)]** タブを選択します。

<img src="./media/lab1-9.png" alt="lab1-9" style="zoom:67%;" />

3. **[組織のプロファイル(Organization profile)]** タブ を下にスクロールし、 **[組織の情報(Organization Infomation)]** をクリックします。

4. **[組織の情報(Organization Infomation)]** ページで組織名が **[Contoso]** と表示されていることを確認したら、以下の項目を設定します。<br>
   **※特に指示がないものは既定値のままで構いません。**
   入力したら、 **[保存]** をクリックします。

   | 項目                                              | 値                                                           |
   | ------------------------------------------------- | ------------------------------------------------------------ |
   | **名前(Name)**                                    | Adatum Corporation<br />※Adatum と Corporation の間に半角スペースを入れてください。 |
   | **郵便番号(ZIP or postal code)**                  | 98052-7329                                                   |
   | **都道府県(State or providence)**                 | ワシントン                                                   |
   | **市区町村(City)**                                | 既定値                                                       |
   | **番地(Street address)**                          | 1                                                            |
   | **建物名(Apartment or suite)**                    | なし                                                         |
   | **国または地域(Country or Region)**               | 既定値                                                       |
   | **電話(Phone)**                                   | 425-555-1234                                                 |
   | **技術的な事項に関する連絡先(Technical contact)** | 既定値                                                       |
   | **優先する言語(Preferred language)**              | English                                                      |

   <img src="./media/lab1-10.png" alt="lab1-10" style="zoom:80%;" />

5. **[組織の情報(Organization information)]** ページ上部に **「保存されました」** と表示されたことを確認し、 **[組織の情報(Organization information)]** ページを × で閉じます。

   ![lab1-11](./media/lab1-11.png)

6. **[組織のプロファイル(Organization profile)]** タブが表示されていることを確認し、 **[カスタムテーマ(Custom themes)]** をクリックします。

   <img src="./media/lab1-15.png" alt="lab1-15" style="zoom: 67%;" />

11.  **[組織のMicrosoft 365のカスタマイズ(Customize ‎Microsoft 365‎ for your organization)]** ページで、 **[ ＋ テーマを追加する(Add theme)]** をクリックします。 

12.  **[既定のテーマ(Default theme)]** ページで、 **[色(Colors)]** タブをクリックし、 **[ナビゲーションバーの色(Navigation bar color)]** の黒いボタンをクリックします。

13. カラーパレットから任意の色を選択します。 

    **※注意※以下のようにパレット内に警告マークが表示された場合、その色は推奨されないため、使用可能な色 (濃色)を選択してください。**

    <img src="./media/lab1-17.png" alt="lab1-17" style="zoom:80%;" />

    色を選択したら、 **[保存(Save)]** ボタンをクリックします。

14.  **「変更内容が保存されました。」** というメッセージが表示されたことを確認して、 **[既定のテーマ(Default theme)]** ページを画面右上の **[x]** をクリックして閉じます。 

15.  **[F5] キー** を押して、ページを更新します。 

16. 画面上部のナビゲーションバーの色が変更されたことを確認します。

    

### **結果 : この演習を完了すると、Microsoft 365 管理ポータルの言語設定の変更や、組織プロファイルの設定変更ができるようになります。**

---
lab:
  title: 'ラボ 01: Microsoft Entra ID を管理する'
  module: Administer Identity
---

# ラボ 01 - Microsoft Entra ID を管理する

## ラボ概要

このラボでは、ユーザーとグループについて説明します。 ユーザーとグループは、IDソリューションの基本的な構成要素です。 

## 推定時間:30 分

## ラボシナリオ

あなたの組織は、アプリとサービスの運用前テスト用に新しいラボ環境を構築しています。  仮想マシンを含むラボ環境を管理するために、数名のエンジニアが雇用されています。 エンジニアがMicrosoft Entra IDを使って認証できるよう、ユーザーとグループをプロビジョニングするという仕事があなたに課せられました。 管理のオーバーヘッドを最小限に抑えるには、役職に基づいてグループのメンバーシップを自動的に更新する必要があります。 

## アーキテクチャ図
![ラボ 01 アーキテクチャの図。](./media/az104-lab01-architecture.png)

## 実施するタスク

+ タスク 1:ユーザーアカウントを作成して構成します。
+ タスク 2:グループを作成し、メンバーを追加します。

## タスク 1:ユーザー アカウントを作成して構成する

このタスクでは、ユーザーアカウントを作成して構成します。ユーザーアカウントには、名前、部署、場所、連絡先情報などのユーザーデータが格納されます。

1. [**Azure portal**](https://portal.azure.com)にサインインします。

    >**注:** ツアーが開始された（"Microsoft Azureへようこそ"等の表示）場合は、"キャンセル"をクリックしてAzure Portalへ移動します。

1. 画面上部の検索ボックスを使用し、`Microsoft Entra ID` を検索して選択します。 Microsoft Entra ID は、Azure のクラウドベースの ID およびアクセス管理ソリューションです。 

1. **[概要]** ブレードから **[テナントの管理]** をクリックします。 現状参加しているテナントが確認できます。

    >テナントとは、アカウントとグループを含む Microsoft Entra ID の特定のインスタンスです。 状況に応じて、さらにテナントを作成し、テナント間を**切り替える**ことができます。 

1. **[+作成]** をクリックして新しいテナントを作成します。 **[基本]** タブでは、**テナントの種類** として **[Microsoft Entra ID]** を選択します。

1. **[構成]** タブで、次のフィールドを構成してから、 **[確認および作成]** 、 **[作成]** の順にクリックします。

    ※テナントの作成完了まで待ちます。作成には数分かかることが予想されます。

    | 設定           | 値                                                           |
    | -------------- | ------------------------------------------------------------ |
    | 組織名         | **First AAD**                                                |
    | 初期ドメイン名 | **firstaadXXXXXXXX**<br />※XにはLabUser-XXXXXXXXと同じ8桁の数字を入力します。(あるいはランダムで一意な数字) |
    | 場所           | **米国**                                                     |

1. ディレクトリの作成が完了したら、Azure portal の上部にあるツールバーで**歯車状のアイコン** をクリックして **[ポータルの設定 | ディレクトリとサブスクリプション]** に移動します。

1. 作成したFirst AADの横に表示されている **[切り替え]** をクリックして、テナントをスイッチします。
### 新しいユーザーを作成する

1. First AADに移動してから、再度Microsoft Entra IDを検索して画面に移動します。**[ユーザー]** → **[新しいユーザー]** の順にクリックし、ドロップダウンリストから **[新しいユーザーの作成]** を選びます。 

1. 次の設定を入力して新しいユーザーを作成します（指定のない箇所は既定値を使用）。 **[プロパティ]** タブには、ユーザー アカウントに含めることができるさまざまな情報がすべて表示されます。 

    | 設定 | 値 |
    | --- | --- |
    | ユーザープリンシパル名 | `az104-user1` |
    | 表示名 | `az104-user1` |
    | パスワードの自動生成 | チェックを入れる |
    | 有効なアカウント | チェックを入れる       |
    | 役職 ([プロパティ] タブ) | `IT Lab Administrator` |
    | 部署 ([プロパティ] タブ) | `IT` |
    | 利用場所 ([プロパティ] タブ) | **米国** |

1. 入力が完了したら、**[確認および作成]**、**[作成]** の順に選びます。

1. ページを更新して、新しいユーザーが作成されたことを確認します。 

### 外部ユーザーを招待する

1. **[新しいユーザー]** ドロップダウンで **[外部ユーザーの招待]** を選びます。 

    | 設定 | 値 |
    | --- | --- |
    | メール | 自分のメール アドレス |
    | 表示名 | Lab User |
    | 招待メッセージの送信 | **チェックボックスをオンにします** |
    | メッセージ | `Welcome to Azure and our group project` |

1. **[プロパティ]** タブに移動します。これらのフィールドを含む基本情報を入力します。 

    | 設定 | 値 |
    | --- | --- |
    | 役職 | `IT Lab Administrator` |
    | 部署  | `IT` |
    | 利用場所 | **米国** |

1. **[レビューと招待]**、**[招待]** の順に選びます。
## タスク 2:グループを作成し、メンバーを追加する

このタスクでは、グループ アカウントを作成します。 グループ アカウントにはユーザー アカウントまたはデバイスを含めることができます。 メンバーをグループに割り当てるには、2 つの基本的な方法があります。静的と動的です。 静的グループの場合、管理者がメンバーを手動で追加および削除する必要があります。  動的グループは、ユーザー アカウントまたはデバイスのプロパティに基づいて自動的に更新されます。 たとえば、役職です。 

1. Azure portal で、**[グループ]** を検索して選択します。

1. **[すべてのグループ]** ブレードで **[+ 新しいグループ]** をクリックし、新しいグループを作成します。     

    | 設定 | 値 |
    | --- | --- |
    | グループの種類 | **セキュリティ** |
    | グループ名 | `IT Lab Administrators` |
    | グループの説明 | `Administrators that manage the IT lab` |
    | メンバーシップの種類 | **割り当て済み** |

    >**注**: メンバーシップの種類に動的メンバーシップを適用するにはEntra ID Premium P1 または P2 ライセンスが必要です。 他の **[メンバーシップの種類]** を使用できる場合は、ドロップダウンにオプションが表示されます。 
    
1. **[所有者が選択されていません]** をクリックしてグループのオーナーを設定します。**[所有者の追加]** ページで、自分自身を検索してチェックボックスにチェックを入れて選択します。所有者は複数設定することも可能です。 

1. **[メンバーが選択されていません]** を選択します。

1. **[メンバーの追加]** ペインで、招待した **az104-user1** を検索してチェックボックスにチェックを入れて選択します。これでユーザーがグループに追加されました。 

1. **[作成]** を選んでグループをデプロイします。

1. ページを **[更新]** し、グループが作成されたことを確認します。

1. 新しいグループを選び、**[メンバー]** と **[所有者]** の情報を確認します。

## 要点

以上でラボは完了です。 このラボの要点は次のとおりです。

+ テナントは組織を表し、社内外のユーザー向けに特定インスタンスの Microsoft クラウド サービスを管理するために役立ちます。
+ Microsoft Entra ID には、ユーザー アカウントとゲスト アカウントがあります。 各アカウントには、実行される予定の作業範囲に固有のアクセス レベルがあります。
+ グループは、関連するユーザーまたはデバイスをまとめるものです。 グループには、セキュリティと Microsoft 365 を含む 2 つの種類があります。
+ グループのメンバーシップは、静的または動的に割り当てることができます。


## 自習トレーニングでさらに学習する

+ 「[Microsoft Entra ID の概要](https://learn.microsoft.com/training/modules/understand-azure-active-directory/)」。 Microsoft Entra ID を Active Directory DS と比較し、Microsoft Entra ID P1 および P2 について学習し、クラウドでドメインに参加しているデバイスとアプリを管理するために Microsoft Entra Domain Services の詳細を確認します。
+ 「[Microsoft Entra ID で Azure のユーザーとグループを作成する](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/)」。 Microsoft Entra ID でユーザーを作成します。 さまざまな種類のグループについて理解します。 グループを作成してメンバーを追加します。 企業間のゲスト アカウントを管理します。
+ [ユーザーが Microsoft Entra のセルフサービス パスワード リセットを使用してパスワードをリセットできるようにする](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/)。 セルフサービス パスワード リセット を評価し、組織内のユーザーが自分のパスワードをリセットしたり、アカウントのロックを解除したりできるようにします。 セルフサービス パスワード リセット を設定、構成、テストします。



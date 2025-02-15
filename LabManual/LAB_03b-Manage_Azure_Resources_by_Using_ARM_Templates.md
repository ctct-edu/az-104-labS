---
lab:
  title: 'ラボ 03:Azure Resource Manager テンプレートを使用して Azure リソースを管理する'
  module: Administer Azure Resources
---

# ラボ 03 - Azure Resource Manager テンプレートを使用して Azure リソースを管理する

## ラボ概要

このラボでは、リソースのデプロイを自動化する方法について学習します。 学習するのは、Azure Resource Manager テンプレートと Bicep テンプレートについてです。 テンプレートをデプロイするさまざまな方法について学習します。 

## 推定時間:50 分

## ラボのシナリオ

あなたのチームは、リソースのデプロイを自動化して簡素化する方法を探しています。 組織では、管理のオーバーヘッドを削減し、人為的ミスを減らし、一貫性を高める方法を探しています。  

## アーキテクチャの図

![タスクの図。](./media/az104-lab03-architecture.png)

## 実施するタスク

+ タスク 1: Azure Resource Manager テンプレートを作成する。
+ タスク 2: Azure Resource Manager テンプレートを編集し、テンプレートを再デプロイする。
+ タスク 3: Cloud Shell を構成し、Azure PowerShell を使用してテンプレートをデプロイする。
+ タスク 4: CLI を使用してテンプレートをデプロイする。 
+ タスク 5:Azure Bicep を使用してリソースをデプロイします。

## タスク 1:Azure Resource Manager テンプレートの作成

このタスクでは、Azure portal でマネージド ディスクを作成します。 マネージド ディスクは、仮想マシンで使用するように設計されたストレージです。 ディスクがデプロイされたら、他のデプロイで使用できるテンプレートをエクスポートします。

1. [**Azure portal**](https://portal.azure.com) にサインインします。

1. **[ディスク]** を検索して選択します。

1. [ディスク] ブレードで **[+作成]** を選択します。

1. **[マネージド ディスクの作成]** ブレードで、ディスクを構成し、**[OK]** を選択します。 
   
    | 設定 | 値 |
    | --- | --- |
    | サブスクリプション | MOC Subscription（既定のものを使用） |
    | リソース グループ | az104-rg3（新規作成） |
    | ディスク名 | `az104-disk1` |
    | リージョン | **(US) East US** |
    | 可用性ゾーン | **インフラストラクチャ冗長は必要ありません** |
    | 変換元の型 | **なし** |
    | パフォーマンス | **[Standard HDD]** (サイズ変更) |
    | サイズ | **[32 GiB]** |

    >**注:**  テンプレートを使用して練習できるように、シンプルなマネージド ディスクを作成しています。 Azure マネージド ディスクは、Azure によって管理されるブロックレベルのストレージ ボリュームです。

1. **[確認および作成]** をクリックし、**[作成]** を選択します。

1. 通知 (画面上部のベル状アイコン) を監視し、デプロイ後に **[リソースに移動]** を選択します。 

1. **[オートメーション]** セクションから、**[テンプレートのエクスポート]** を選択します。 

1. **[ダウンロード]** をクリックし、テンプレートをローカルドライブに保存します。 これにより、圧縮された ZIP ファイルが作成されます。 

1. エクスプローラーを使用して、ダウンロードしたZIPファイルを展開します。 JSON ファイルが 2 つ (テンプレートとパラメータ) あることを確認してください。 


## タスク 2:Azure Resource Manager テンプレートを編集してテンプレートを再デプロイする

このタスクでは、ダウンロードしたテンプレートを使用して新しいマネージド ディスクをデプロイします。 このタスクでは、デプロイを迅速かつ簡単に繰り返す方法について説明します。 

1. Azure portal で、**[カスタムテンプレートのデプロイ]** を検索して選択します。

1. **[エディターで独自のテンプレートを作成する]** を選択します。

1. **[テンプレートの編集]** ブレードで、**[ファイルの読み込み]** をクリックし、ダウンロードした **[template.json]** ファイルをローカル ディスクにアップロードします。

1. エディター ペイン内で、これらの変更を行います。

    + **[disks_az104_disk1_name]** を `disk_name` に変更します (変更は 2 箇所)
    + **[az104-disk1]** を `az104-disk2` に変更します (変更は 1 箇所)

1. これが **[Standard]** ディスクであることを確認してください。 場所は **[eastus]** です。 ディスク サイズは **[32GB]** です。

1. 変更内容を**保存**します。

1. パラメータ ファイルも同様に読み込みます。 **[パラメーターの編集]** を選択し、**[ファイルの読み込み]** をクリックして **[parameters.json]** をアップロードします。 

1. この変更は、テンプレート ファイルと一致させるために行います。

    **[disks_az104_disk1_name]** を **[disk_name]** に変更します (変更は 1 箇所)

1. 変更内容を**保存**します。 

1. 次のようにカスタム デプロイ設定を完了します。

    | 設定 | 値 |
    | --- |--- |
    | サブスクリプション | MOC Subscription（既定のものを使用） |
    | リソース グループ | `az104-rg3` |
    | "リージョン" | **(US) East US** |
    | Disk_name | `az104-disk2` |

1. **[確認と作成]** を選択し、次に **[作成]** を選択します。

1. **[リソースに移動]** を選択します。 **[az104-disk2]** が作成されたことを確認します。

1. **[概要]** ブレードで、リソース グループ **[az104-rg3]** を選択します。 2 つのディスクが表示されているはずです。
   
1. **[設定]** セクションで **[デプロイ]** をクリックします。

    >**注:**  すべてのデプロイの詳細は、リソース グループに記載されています。 テンプレートを使用して大規模な操作を行う前に、先にいくつかのテンプレートベースのデプロイが成功するかどうかを確認することをお勧めします。

1. デプロイを選択し、**[入力]** と **[テンプレート]** のブレードの内容をレビューします。

## タスク 3:Cloud Shell を構成し、Azure PowerShell を使用してテンプレートをデプロイする

このタスクでは、Azure Cloud Shell と Azure PowerShell を使用します。 Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーでアクセスできる対話形式の認証されたターミナルです。 Bash または PowerShell どちらかのシェル エクスペリエンスを作業方法に合わせて柔軟に選択できます。 このタスクでは、PowerShell を使用してテンプレートをデプロイします。 

1. Azure portal で、右上の **[Cloud Shell]** アイコンを選択します。 または、[`https://shell.azure.com`](https://shell.azure.com) にジャンプすることでも使用可能です。

   ![[Cloud Shell] アイコンのスクリーンショット。](./media/az104-lab03-cloudshell-icon.png)

1. **Bash** または **PowerShell** の選択を求めるメッセージが表示されたら、**PowerShell** を選択します。 

1. **[作業の開始]** の画面で、**[ストレージアカウントをマウントする]** を選択し、 **[ストレージアカウントのサブスクリプション]** のドロップダウンリストより既存のサブスクリプションを選択後に **[適用]** をクリックします。 

    >**注:**  Cloud Shell を使用してセッション間でのデータ保持を実現する場合は、ストレージ アカウントとファイル共有が必要です。 

1. **[ストレージアカウントをマウントする]** の画面では **[ストレージアカウントが作成されます]** を選択します。これにより、名称等が自動決定される形でCloud Shell用のストレージアカウントが作成されます。これは、Cloud Shell を初めて使用する場合にのみ必要です。 ストレージがプロビジョニングされるまで数分かかります。

1. **[ファイルのアップロード/ダウンロード]** アイコンを使用して、ダウンロード ディレクトリからテンプレートとパラメータ ファイルをアップロードします。 各ファイルは別々にアップロードする必要があります。

1. ファイルが Cloud Shell ストレージにあることを確認します。 

    ```powershell
    dir
    ```
    >**注**:必要に応じて、**cls** を使用してコマンド ウィンドウをクリアすることができます。 方向キーを使用するとコマンド履歴を移動できます。
   
1. **[エディター]** (中かっこ) アイコンを選択し、テンプレート JSON ファイルに移動します。

1. 変更を加えます。 たとえば、ディスク名を **[az104-disk3]** に変更します。 **Ctrl + S** キーを押して、変更内容を保存します。 

    >**注**:リソース グループ、サブスクリプション、管理グループ、またはテナントをテンプレートのデプロイのターゲットにすることができます。 使用するコマンドは、デプロイのスコープに応じて異なります。

1. リソース グループにデプロイするには、**New-AzResourceGroupDeployment** を使用します。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. コマンドが完了し、[ProvisioningState] が **[成功]** になっていることを確認します。

1. ディスクが作成されたことを確認します。

   ```powershell
   Get-AzDisk
   ```
   
## タスク 4:CLI を使用してテンプレートをデプロイする 

1. 引き続き、**[Cloud Shell]** で **[Bash]** を選択します。 

1. ファイルが Cloud Shell ストレージにあることを確認します。 前のタスクを完了した場合は、テンプレート ファイルが使用できるはずです。 

    ```sh
    ls
    ```

1. **[エディター]** (中かっこ) アイコンを選択し、テンプレート JSON ファイルに移動します。

1. 変更を加えます。 たとえば、ディスク名を **[az104-disk4]** に変更します。 **Ctrl + S** キーを押して、変更内容を保存します。 

    >**注**:リソース グループ、サブスクリプション、管理グループ、またはテナントをテンプレートのデプロイのターゲットにすることができます。 使用するコマンドは、デプロイのスコープに応じて異なります。

1. リソース グループにデプロイするには、**az deployment group create** を使用します。

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. コマンドが完了し、[ProvisioningState] が **[成功]** になっていることを確認します。

1. ディスクが作成されたことを確認します。

     ```sh
     az disk list --output table
     ```

## タスク 5:Azure Bicep を使用してリソースをデプロイする

このタスクでは、Bicep ファイルを使用してマネージド ディスクをデプロイします。 Bicep は、ARM テンプレート上に構築された宣言型オートメーション ツールです。

1. **[Bash]** セッションの **[Cloud Shell]** で作業を続けます。

1. **[\Allfiles\Labs\03\azuredeploydisk.bicep]** ファイルを見つけてダウンロードします。

    ※ラボで使用するファイル群は[ここから](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)ダウンロード可能です。

1. Cloud Shell に Bicep ファイルを**アップロード**します。 

1. **[エディター]** (中かっこ) アイコンを選択し、そのファイルに移動します。

1. Bicep テンプレート ファイルの読み取りには少し時間がかかります。 ディスク リソースがどのように定義されているかに注目してください。 

1. 次の変更を行います。

    + **[managedDiskName]** の値を `Disk4` に変更します。
    + **[SKU 名]** の値を `StandardSSD_LRS` に変更します。
    + **[diskSizeinGiB]** の値を `32` に変更します。

1. **Ctrl + S** キーを押して、変更内容を保存します。

1. ここで、テンプレートをデプロイします。

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. ディスクが作成されたことを確認します。

    ```sh
    az disk list --output table
    ```

    >**注:**  5 つのマネージド ディスクが、それぞれ異なる方法で正常にデプロイされました。 お疲れさまでした。

## 要点

以上でラボは完了です。 このラボの要点は次のとおりです。 

+ Azure Resource Manager テンプレートではソリューションのすべてのリソースをグループとしてデプロイ、管理、監視でき、リソースを個別に扱う必要がありません。
+ Azure Resource Manager テンプレートは JavaScript Object Notation (JSON) ファイルであり、スクリプトではなく宣言によってインフラストラクチャを管理できます。
+ テンプレート内のインライン値としてパラメータを渡すのではなく、パラメータ値を含む別の JSON ファイルを使用することができます。
+ Azure Resource Manager テンプレートは、Azure portal、Azure PowerShell、および CLI などのさまざまな方法でデプロイすることができます。
+ Bicep は、Azure Resource Manager テンプレートの代替手段です。 Bicep は宣言型の構文を使用して Azure リソースをデプロイします。 

Bicep では、簡潔な構文、信頼性の高いタイプ セーフ、およびコードの再利用のサポートが提供されます。 Bicep により、Azure のコードとしてのインフラストラクチャ ソリューションに最適な作成エクスペリエンスをオファーします。

## 自習トレーニングでさらに学習する

+ [JSON ARM テンプレートを使用して Azure インフラストラクチャをデプロイする](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)。 Visual Studio Code を使用して、JSON Azure Resource Manager テンプレート (ARM テンプレート) を作成し、一貫して信頼性の高い方法で Azure にインフラストラクチャをデプロイします。
+ [Azure Cloud Shell の機能とツールを確認する](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/)。 Cloud Shell の機能とツール。 
+ [Windows PowerShell を使用して Azure リソースを管理する](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/)。 このモジュールでは、クラウド サービス管理に必要なモジュールをインストールし、PowerShell コマンドを使用して、Azure 仮想マシン、Azure サブスクリプション、Azure ストレージ アカウントなどのクラウド リソースに対して簡単な管理タスクを実行する方法について説明します。
+ [Bash の概要](https://learn.microsoft.com/training/modules/bash-introduction/)。 Bash を使って IT インフラストラクチャを管理します。
+ [初めての Bicep テンプレートを作成する](https://learn.microsoft.com/training/modules/build-first-bicep-template/)。 Bicep テンプレート内で Azure リソースを定義します。 デプロイの一貫性と信頼性を向上させ、必要な手作業の労力を減らし、環境間でデプロイを拡張します。 テンプレートは柔軟性があり、パラメーター、変数、式、モジュールを使用して再利用できます。


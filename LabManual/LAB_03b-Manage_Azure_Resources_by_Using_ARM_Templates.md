# ラボ 03 - Azure Resource Manager テンプレートを使用して Azure リソースを管理する

## ラボ概要

このラボでは、リソースのデプロイを自動化する方法について学習します。学習するのは、Azure Resource Manager テンプレートと Bicep テンプレートについてです。テンプレートをデプロイするさまざまな方法について学習します。

## 推定時間: 50 分

## ラボのシナリオ

あなたのチームは、リソースのデプロイを自動化して簡素化する方法を探しています。組織では、管理のオーバーヘッドを削減し、人為的ミスを減らし、一貫性を高める方法を探しています。

## アーキテクチャの図



## 実施するタスク

- タスク 1: Azure Resource Manager テンプレートを作成する。
- タスク 2: Azure Resource Manager テンプレートを編集し、テンプレートを再デプロイする。
- タスク 3: Cloud Shell を構成し、Azure PowerShell を使用してテンプレートをデプロイする。
- タスク 4: CLI を使用してテンプレートをデプロイする。
- タスク 5: Azure Bicep を使用してリソースをデプロイする。

## タスク 1: Azure Resource Manager テンプレートの作成

このタスクでは、Azure portal でマネージド ディスクを作成します。マネージド ディスクは、仮想マシンで使用するように設計されたストレージです。ディスクがデプロイされたら、他のデプロイで使用できるテンプレートをエクスポートします。

1. [**Azure portal**](https://portal.azure.com) にサインインします。

2. **[ディスク]** を検索して選択します。

3. **[ディスク]** ブレードで **[+ 作成]** を選択します。

4. **[マネージド ディスクの作成]** ブレードで、次のように構成します。

   | 設定 | 値 |
   | --- | --- |
   | サブスクリプション | MOC Subscription（既定のものを使用） |
   | リソース グループ | `az104-rg3`（新規作成） |
   | ディスク名 | `az104-disk1` |
   | リージョン | **(US) East US** |
   | 可用性ゾーン | **インフラストラクチャ冗長は必要ありません** |
   | ソースの種類 | **なし** |

5. **[サイズの変更]** をクリックします。

6. **[ディスク サイズの選択]** 画面で、次の設定を指定します。

   | 設定 | 値 |
   | --- | --- |
   | ストレージの種類 | **Standard HDD** |
   | サイズ | **32 GiB** |

7. 設定後、**[OK]** または **[保存]** をクリックして、マネージド ディスクの作成画面に戻ります。

8. **[確認および作成]** をクリックし、検証に成功したことを確認して **[作成]** をクリックします。
   > **注:** テンプレートを使用して練習できるように、シンプルなマネージド ディスクを作成しています。Azure マネージド ディスクは、Azure によって管理されるブロックレベルのストレージ ボリュームです。

5. **[確認および作成]** を選択し、検証が完了したら **[作成]** を選択します。

6. 通知（画面上部のベル状アイコン）を監視し、デプロイ後に **[リソースに移動]** を選択します。

7. **[オートメーション]** セクションから、**[テンプレートのエクスポート]** を選択します。

8. **[ダウンロード]** を選択し、テンプレートをローカル ドライブに保存します。

9. ダウンロードしたファイルを確認し、**`template.json`** が保存されていることを確認します。

## タスク 2: Azure Resource Manager テンプレートを編集してテンプレートを再デプロイする

このタスクでは、ダウンロードした `template.json` を使用して新しいマネージド ディスクをデプロイします。このタスクでは、デプロイを迅速かつ簡単に繰り返す方法について説明します。

1. Azure portal で、**[カスタム テンプレートのデプロイ]** を検索して選択します。

2. **[エディターで独自のテンプレートを作成する]** を選択します。

3. **[テンプレートの編集]** ブレードで、**[ファイルの読み込み]** を選択し、タスク 1 でダウンロードした **`template.json`** をアップロードします。

4. エディター ペイン内で、次の変更を行います。

   - `disks_az104_disk1_name` を `disk_name` に変更します（2 箇所）。
   - `az104-disk1` を `az104-disk2` に変更します（1 箇所）。

   変更後、パラメーター定義は次のようになります。

   ```json
   "parameters": {
     "disk_name": {
       "defaultValue": "az104-disk2",
       "type": "String"
     }
   }
   ```

   また、ディスク リソースの `name` が次のようになっていることを確認します。

   ```json
   "name": "[parameters('disk_name')]"
   ```
   
5. 変更内容を **[保存]** します。

6. 次のようにカスタム デプロイ設定を完了します。

   | 設定 | 値 |
   | --- | --- |
   | サブスクリプション | MOC Subscription（既定のものを使用） |
   | リソース グループ | `az104-rg3` |
   | リージョン | **(US) East US** |
   | Disk_name | `az104-disk2` |

7. **[確認と作成]** を選択し、次に **[作成]** を選択します。

8. デプロイが完了したら **[リソースに移動]** を選択し、**`az104-disk2`** が作成されたことを確認します。

9. **[設定]** セクションで **[デプロイ]** を選択します。

    > **注:** すべてのデプロイの詳細は、リソース グループに記録されています。テンプレートを使用して大規模な操作を行う前に、いくつかのテンプレート ベースのデプロイが正常に完了することを確認することをお勧めします。

10. リスト内のいずれかのデプロイ名を選択し、**[入力]** と **[テンプレート]** の内容を確認します。

## タスク 3: Cloud Shell を構成し、Azure PowerShell を使用してテンプレートをデプロイする

このタスクでは、Azure Cloud Shell と Azure PowerShell を使用します。Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーでアクセスできる対話形式の認証されたターミナルです。Bash または PowerShell のどちらかのシェル エクスペリエンスを作業方法に合わせて選択できます。このタスクでは、PowerShell を使用してテンプレートをデプロイします。

1. Azure Portal 右上のツールバーにある、**「>_」のようなターミナル形の [Cloud Shell] アイコン**をクリックします。  
   または、`https://shell.azure.com` にアクセスします。

1. **Bash** または **PowerShell** の選択を求められた場合は、**PowerShell** を選択します。

3. **[作業の開始]** の画面で **[ストレージ アカウントをマウントする]** を選択し、**[ストレージ アカウントのサブスクリプション]** のドロップダウン リストから既存のサブスクリプションを選択した後、**[適用]** を選択します。

   > **注:** Cloud Shell でセッション間のデータ保持を行う場合は、ストレージ アカウントとファイル共有が必要です。

4. **[ストレージ アカウントをマウントする]** の画面では **[ストレージ アカウントが作成されます]** を選択します。これにより、名称などが自動決定された Cloud Shell 用のストレージ アカウントが作成されます。これは、Cloud Shell を初めて使用する場合にのみ必要です。ストレージがプロビジョニングされるまで数分かかる場合があります。

5. **[ファイルのアップロード/ダウンロード]** アイコンを使用して、タスク 1 でダウンロードした **`template.json`** を Cloud Shell にアップロードします。

6. ファイルが Cloud Shell ストレージにあることを確認します。

   ```powershell
   dir
   ```

   > **注:** 必要に応じて、`cls` を使用してコマンド ウィンドウをクリアできます。方向キーを使用するとコマンド履歴を移動できます。

7. **[エディター]**（中かっこ）アイコンを選択し、`template.json` を開きます。

   ※ UI を切り替えるメッセージが表示された場合は、そのまま **[確認]** を選択して進めます。

8. `disk_name` パラメーターの `defaultValue` を **`az104-disk3`** に変更します。

   ```json
   "parameters": {
     "disk_name": {
       "defaultValue": "az104-disk3",
       "type": "String"
     }
   }
   ```

   **Ctrl + S** キーを押して変更内容を保存します。

   > **注:** リソース グループ、サブスクリプション、管理グループ、またはテナントをテンプレート デプロイのターゲットにできます。使用するコマンドは、デプロイのスコープに応じて異なります。

9. リソース グループにデプロイするには、**`New-AzResourceGroupDeployment`** を使用します。

   ```powershell
   New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json
   ```

10. コマンドが完了し、`ProvisioningState` が **Succeeded** になっていることを確認します。

11. ディスクが作成されたことを確認します。

    ```powershell
    Get-AzDisk -ResourceGroupName az104-rg3
    ```

    `az104-disk3` が表示されることを確認します。

## タスク 4: CLI を使用してテンプレートをデプロイする

1. 引き続き **[Cloud Shell]** で **[Bash]** を選択します。

2. ファイルが Cloud Shell ストレージにあることを確認します。前のタスクを完了している場合は、`template.json` が使用できるはずです。

   ```sh
   ls
   ```

3. 先ほどと同様に、**[エディター]**（中かっこ）アイコンを選択し、`template.json` を開きます。

4. `disk_name` パラメーターの `defaultValue` を **`az104-disk4`** に変更します。

   ```json
   "parameters": {
     "disk_name": {
       "defaultValue": "az104-disk4",
       "type": "String"
     }
   }
   ```

   **Ctrl + S** キーを押して変更内容を保存します。

   > **注:** リソース グループ、サブスクリプション、管理グループ、またはテナントをテンプレート デプロイのターゲットにできます。使用するコマンドは、デプロイのスコープに応じて異なります。

5. リソース グループにデプロイするには、**`az deployment group create`** を使用します。

   ```sh
   az deployment group create --resource-group az104-rg3 --template-file template.json
   ```

6. コマンドが完了し、`provisioningState` が **Succeeded** になっていることを確認します。

7. ディスクが作成されたことを確認します。

   ```sh
   az disk list --output table --resource-group az104-rg3
   ```

   `az104-disk4` が表示されることを確認します。

## タスク 5: Azure Bicep を使用してリソースをデプロイする

このタスクでは、Bicep ファイルを使用してマネージド ディスクをデプロイします。Bicep は、ARM テンプレート上に構築された宣言型オートメーション ツールです。

1. **[Bash]** セッションの **[Cloud Shell]** で作業を続けます。

2. **`Allfiles/Labs/03/azuredeploydisk.bicep`** ファイルを見つけてダウンロードします。

   ※ ラボで使用するファイル群は[ここから](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)ダウンロードできます。ダウンロードした ZIP ファイルは展開してから使用します。

3. Cloud Shell に Bicep ファイルを **アップロード** します。

4. **[エディター]**（中かっこ）アイコンを選択し、そのファイルを開きます。

5. Bicep テンプレート ファイルの読み取りには少し時間がかかる場合があります。ディスク リソースがどのように定義されているか確認してください。

6. 次の変更を行います。

   - `managedDiskName`の値を`diskname`から`Disk4` に変更します。
   - SKU 名の値を `UltraSSD_LRS`から`StandardSSD_LRS` に変更します。
   - `diskSizeinGiB` の値を`8`から`32` に変更します。

7. **Ctrl + S** キーを押して変更内容を保存します。

8. テンプレートをデプロイします。

   ```sh
   az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
   ```

9. ディスクが作成されたことを確認します。

   ```sh
   az disk list --output table --resource-group az104-rg3
   ```

   > **注:** 5 つのマネージド ディスクが、それぞれ異なる方法で正常にデプロイされました。お疲れさまでした。

## 要点

以上でラボは完了です。このラボの要点は次のとおりです。

- Azure Resource Manager テンプレートでは、ソリューションのリソースをグループとしてデプロイ、管理、監視でき、リソースを個別に扱う必要がありません。
- Azure Resource Manager テンプレートは JavaScript Object Notation（JSON）ファイルであり、スクリプトではなく宣言によってインフラストラクチャを管理できます。
- テンプレートではパラメーターを定義でき、値をテンプレート内の `defaultValue` として指定したり、必要に応じて別のパラメーター ファイルから渡したりできます。
- Azure Resource Manager テンプレートは、Azure portal、Azure PowerShell、および Azure CLI などのさまざまな方法でデプロイできます。
- Bicep は Azure Resource Manager テンプレートの代替手段です。Bicep は宣言型の構文を使用して Azure リソースをデプロイします。
- Bicep では、簡潔な構文、型チェック、コード再利用などの機能が提供され、Azure の Infrastructure as Code を効率的に記述できます。

## 自習トレーニングでさらに学習する

- [JSON ARM テンプレートを使用して Azure インフラストラクチャをデプロイする](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)。Visual Studio Code を使用して、JSON Azure Resource Manager テンプレート（ARM テンプレート）を作成し、一貫して信頼性の高い方法で Azure にインフラストラクチャをデプロイします。
- [Azure Cloud Shell の機能とツールを確認する](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/)。Cloud Shell の機能とツールについて学習します。
- [Windows PowerShell を使用して Azure リソースを管理する](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/)。クラウド サービス管理に必要なモジュールをインストールし、PowerShell コマンドを使用して Azure リソースを管理する方法について学習します。
- [Bash の概要](https://learn.microsoft.com/training/modules/bash-introduction/)。Bash を使って IT インフラストラクチャを管理します。
- [初めての Bicep テンプレートを作成する](https://learn.microsoft.com/training/modules/build-first-bicep-template/)。Bicep テンプレート内で Azure リソースを定義し、デプロイの一貫性と信頼性を向上させる方法を学習します。

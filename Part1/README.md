# iPhone単体で脱獄アプリ、Tweakの作り方

## 目次

1. [インストール前に](#インストール前の準備及び確認)
2. [依存パッケージのインストール](#依存パッケージのインストール)
3. [動作確認](#とりあえずtweakを作る動作確認)
<br>
<br>

## インストール前の準備及び確認

自分がどの脱獄を使っているのか、どの環境なのかによっていろいろ変わってきます。以下を確認してください。  
- iOS12以上であること  
- Theosをインストールする端末がRootful, Rootless, Roothideどの環境か
- Procursus環境か、他の環境か
- ストレージ空き容量は余裕を持って7GB以上はほしい
<br>

今回はProcursusリポジトリが最初から入っている環境を想定しています。  
checkra1n及びunc0verは対象外ですのでご了承ください。  

```checkra1n及びunc0ver環境にProcursusリポジトリを追加しないでください。環境が壊れます。```  

<br>
<br>
<br>

## 依存パッケージのインストール

まずSileoから```Theos Dependencies```と```MTerminal```を"Procursusリポジトリから"インストールします。Bigbossリポジトリにも同じ名前のものがありますが間違えないでください。  
<br>
<img width=300px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Theos-Dependencies.png"></img>
<br>
<br>
結構容量が大きいのでインストールに時間がかかります（環境によりますが5分くらい）  
これで依存パッケージ及びターミナル環境が整いました。

<br>
<br>
<br>

## Theosのインストール  

今回はRoothide環境に対応したtheosをインストールしたいので、以下のコマンドをMTerminalで実行します。  
<br>
```bash -c "$(curl -fsSL https://raw.githubusercontent.com/roothide/theos/master/bin/install-theos)```  
<br>
ここで以下のようにProcursusリポジトリはある？と聞かれますので"y"と入力してEnterを押します。（checkra1n/unc0ver環境の場合は"n"）
<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Theos-1.png"></img>  
<br>
<br>


次にパスワードを聞かれますので設定したパスワードを入力します。  
\* パスワードは入力中表示されませんがちゃんと入力されてます。  
\* 特にパスワードを変えてない場合は"alpine"がデフォルトのパスワードです。
<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Theos-2.png"></img>
<br>
<br>

次にswiftを使うか？と聞かれますので"y"か"n"を入力してEnterを押します。  
今回はswiftは使わないのでnにします。（個人的な意見ですがをTweak作るのにswiftはオススメしません）
<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Theos-3.png"></img>
<br>
<br>


その後大量のログが流れてきますが最終的に  
```===>Theos Installer: Theos has been successfully installed! ... ```  
となればインストール完了です。
<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Theos-4.png"></img>
<br>
<br>

Rootful環境の場合、```/var/mobile/theos```にインストールされます。  
インストール場所を確認したい場合、```echo $THEOS```で確認できます。  
ここで一旦MTerminalを再起動します。
<br>
<br>
<br>

## とりあえずTweakを作る（動作確認）  
Theosは基本的にターミナルでプロジェクトの作成、ビルドをします。  
コーディングは別のアプリかvimやnanoで書きます。  
<br>
まずファイルが整理しやすいように作業場を作ります。  
今回は```/var/mobile/Documents/workspace```にプロジェクトをまとめるようにします。
<br>
<br>
MTerminalで```/var/mobile/Documents/workspace```フォルダを作ります。  
<br>
```cd /var/mobile/Documents/```
<br>

でDocumentsフォルダに移動し、
<br>

```mkdir workspace```  
<br>
でworkspaceフォルダを作り、続けて  
    
```cd workspace```  
    
でworkspaceフォルダに移動します。  

続けて  

```$THEOS/bin/nic.pl```  

と入力します。するとTheosのプロジェクト作成画面が出てきます。

<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Build-1.png"></img>
<br>
<br>
この画面で何を作るかを番号で入力します。これは環境によって番号が変わりますので自身の画面を確認してください。  
<br>
<br>

今回の場合Tweakは17番なので17と入力してEnterを押します。  

**（以下のプロジェクト設定は全てあとから変更可能です。）**

次に```Project Name :```と聞かれるのでプロジェクト名を決めます。  

今回は例として"helloworld"とします。  

次に```Package Name :```と聞かれるのでパッケージ名を決めます。これは他の脱獄アプリと同じにならないようにします。一般的には  

```\(国名\).\(自分の名前\).\(Tweakの名前\)```  

となります。今回の場合は"jp.dfuku.helloworld"にします。

次に```Author/Maintainer Name```と聞かれるので作者の名前を入力します。  

ここでは"Dfuku"とします。

次に```MobileSubstrate Bundle filter```と聞かれるので「Tweakを適用したいアプリのbundle ID」を入力します。bundle IDはFilzaのアプリマネージャから確認できます。また、全てのアプリに適用したい場合は```com.apple.UIKit```にすると全てのアプリに適用されます。  

今回はなんでもいいので適当に"com.apple.springboard"にします。

次に```List of applications to terminate upon installation...```と聞かれますが、これは基本的に「-」で問題ありません。インストール時に終了させるアプリ名を書くのですが、わざわざ書く必要はないです。

これで```Done.```と表示されればプロジェクト作成完了です。  

```cd [プロジェクト名]```  

でプロジェクトフォルダに移動しましょう。

今回は動作確認のため、特にコードは書かずビルドします。  

rootful向けのTweakの場合:  

```make package```

rootless向けのTweakの場合:  

```make package THEOS_PACKAGE_SCHEME=rootless```

roothide向けのTweakの場合:  

```make package THEOS_PACKAGE_SCHEME=roothide```

でビルドが正常に完了し、プロジェクトフォルダ内の"packages"というフォルダの中にdebファイルができていれば成功です。

また、そのままではデバッグ情報が残ってしまうのでリリース時には追加で  

```FINALPACKAGE=1```  

をつけるとデバッグに必要な情報が消えます。

<br>
<img width=400px src="https://raw.githubusercontent.com/DFuku/HowToMakeiOSTweaks2024/refs/heads/main/Part1/img/Build-2.png"></img>
<br>
<br>

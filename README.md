# はじめに
後期プロジェクトメンバーでは、研究室で扱うWebシステムの一部を作っていただきます。<br>
研究室で制作されているシステムのほどんどは、Apache Wicket というフレームワークを使用しています。<br>
そのため、後期のプロジェクトメンバー講習ではそれのコンポーネントをいくつか使用して、<br>
To-Doリストを作成します。<br>

# お知らせ

### 2018/11/02
第1回プロメンの講習会でまさかの活動内容の変更が余儀なくされたため、<br>
WebServletをやっている場合ではなくなりました。<br>
そのため、急に講習の内容をWicket講習に変更します。<br>
それにあたり、お試しの期間を第2回目まで延ばします。<br>

### 2018/10/30
第2回プロメン講習会は11/6(火)の5講時目にG202教室で行います。<br>
また、次回の講習の頭に、Slackという連絡アプリのセットアップをして頂きます。<br>
学校のWi-fiを信じていない人は、あらかじめPCかスマホにインストールをしておいてください。<br>
また、このSlackでトークルームに招待する際に、プロメン参加希望フォームに入力されたメールアドレスに招待を送ります。<br>
そのため、参加希望フォームの入力は、締め切り厳守でお願いいたします。

### 2018/10/21
後期プロジェクトメンバー参加希望フォームを作成しました。<br>
参加希望の方は、[こちら](https://docs.google.com/forms/d/e/1FAIpQLSdMgzGvoDvXcGwg72v8p1sCaPHdCFpGv7xGUD0RTmHacENaxQ/viewform?usp=sf_link)にアクセスし、アンケートに回答してください。<br>
締め切りは、11/5(月)の23:59とします。

# 前準備

### 環境の構築
このプロジェクトでは以下のソフトウェアが必要になります。

- Java 8
- Maven
- IntelliJ IDEA (推奨)

インストールが必要な場合は各自でお願いします。（余裕があればインストール手順を書くかも）<br>

### プロジェクトの準備
Apache Wicket の Quick Start ページから、Mavenプロジェクトをダウンロードしてください。<br>
[Create a Wicket Quickstart](https://wicket.apache.org/start/quickstart.html) に移動し、フォームを以下のように変更してください。

- Group ID : `com.example`
- Artifact ID : `wicket_handson`
- Wicket Version : 8.1.0

generated command line に生成されたコマンドをコピーしてください。<br>
コマンドプロンプトを開き、Idea Projectsディレクトリ に移動し、<br>
先ほどコピーしたコマンドを貼り付けて実行してください。<br>

実行結果の例
```
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> maven-archetype-plugin:3.0.1:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO]
[INFO] <<< maven-archetype-plugin:3.0.1:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO]
[INFO]
[INFO] --- maven-archetype-plugin:3.0.1:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Batch mode
[INFO] Archetype repository not defined. Using the one from [org.apache.wicket:wicket-archetype-quickstart:8.1.0] found in catalog remote
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: wicket-archetype-quickstart:8.1.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.example
[INFO] Parameter: artifactId, Value: wicket_handson
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.example
[INFO] Parameter: packageInPathFormat, Value: com/example
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.example
[INFO] Parameter: groupId, Value: com.example
[INFO] Parameter: artifactId, Value: wicket_handson
[INFO] Project created from Archetype in dir: C:\Users\k-oketa\IdeaProjects\wicket_handson
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 30.883 s
[INFO] Finished at: 2018-11-04T05:42:39+09:00
[INFO] Final Memory: 11M/52M
[INFO] ------------------------------------------------------------------------
```
IdeaProjects に wicket_handson フォルダが生成されているので<br>
その中にある`pom.xml`を IntelliJ でインポートしてください。<br>
（この手順も余裕があれば書きます。)
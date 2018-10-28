# はじめに
今回はお試し回です。<br />
そこまで深い話はしません。<br />

## 今回の範囲
- 静的と動的

# 演習
[こちら](https://www.dropbox.com/sh/14w4sehczc5oxzq/AADmvKnEvh49uAFUTYIUpAUya?dl=0)から、prmn_servletをダウンロードしてください。<br>
ダウンロードしたら、IntelliJで`pom.xml`をインポートしてください。<br>
## 演習1
パッケージ`lec01`内の`hello.html`に以下のコードを入力してください。<br>
<br>

hello.html
```hello.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    </head>
  <body>
    <p>
      こんにちは！<br />
      見えてますか？<br />
      もし表示されていなければ3年生を呼んでください。
    <p>
  </body>
<html>
```

入力したら、Webブラウザで見てみましょう。<br>
IntelliJ上で右クリックをして、「ブラウザーで開く」の「Chrome」をクリックしてください。<br>
下図のように表示されたら、次へ進んでください。

### 実行結果
![演習1の実行結果](https://i.imgur.com/NijX2zG.png) 

## 演習2
パッケージ`lec01`内の`HelloServlet.java`に以下のコードを入力してください。<br>
<br>

HelloServlet.java
```HelloServlet.java
package lec01;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.time.LocalTime;

@WebServlet("/HelloServlet")
public class HelloServlet extends HttpServlet {

    LocalTime localTime;
    int second;
    String message;
    
    LocalDate locaDate;
    String dayOfWeek;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        // 使用する文字コードの宣言です。
        // これが無いと文字化けするので必ず記述してください。
        resp.setCharacterEncoding("UTF-8");
        // 出力するデータの種類の宣言です。
        resp.setContentType("text/html"); 

        localTime = LocalTime.now();
        second = localTime.getSecond();

        if (second % 3 == 0) {
            message = "コードはジョークみたいなもの。その内容を説明しようとしたらダメ。  -Cory House";
        } else if (second % 5 == 0) {
            message = "コンピュータはあなたの命令を忠実に実行する。  -Ted Nelson";
        } else if (second % 7 == 0) {
            message = "最高のエラーメッセージはそれがでないこと。  -Thomas Fuchs";
        } else {
            message = "";
        }

        localDate = LocalDate.now();
        dayOfWeek = localDate.getDayOfWeek().toString();

        // ページ上に表示する内容を記述しています。
        try (PrintWriter out = resp.getWriter()) {
            out.println("<!DOCTYPE html>");
            out.println("<html>");
            out.println("<meta charset=\"UTF-8\">");
            out.println("<body>");
            out.println("<p>");
            out.println("こんにちは！<br>");
            out.println("このページはWebServletで作成されたページです。<br>");
            out.println(message + "<br>");
            out.println("</p>");
            out.println("</body>");
            out.println("</html>");
        }
    }
}
```
`TomcatServer.java`を右クリックして「実行」を選択してください。<br>
実行したら、http://localhost:8080/prmn_servlet/HelloServlet にアクセスし、複数回更新してください。<br>

### 実行例
![演習2の実行結果](https://i.imgur.com/NnZeYKu.png)

## 解説
演習1で作成したページと、演習2で作成したページを比較してみましょう。<br>
<br>
演習1のページは、何度更新しても、内容が変化することはありません。<br>
このように、状況を通じて一貫して内容が維持されるページのことを**静的なページ**といいます。<br>
<br>
比べて、演習2で作成したページは、更新すると内容が変化することがあります。<br>
このように、状況に応じて内容が変化するページのことを**動的なページ**といいます。<br>
<br>
今期のプロジェクトメンバーでは、この動的なページの動作の仕組みについて紐解いていきたいと思います。<br>
理解できないところが出てきたら、遠慮せずに3年生に聞いてください。<br>

# 課題
## 課題1
`HelloServlet.java`を書き換えて、実行した日の曜日が表示されるようにしてください。<br>
ちなみに、すでに`dayOfWeek = localDate.getDayOfWeek().toString()`の<br>
部分で曜日名を英語で**取得は**できています。（実行例のMONDAYの部分）<br>
### 実行例
![課題1実行結果例](https://i.imgur.com/hgVRgrm.png)<br>

# 発展問題
発展問題は、調べたり聞いたりしないとできない前提の問題です。<br>
そのため、問題文も命令口調です。<br>
心に余裕があったり、家に帰りたくないときに挑戦してみてください。<br>
## 問題1(★☆☆)
パッケージ`lec01`に`DateTimeServlet.java`を作成しなさい。<br>
DateTimeServlet.javaを、http://localhost:8080/prmn_servlet/DateTime に<br>
アクセスしたら、実行例のようにページを表示した日付と時間が表示されるようにしなさい。<br>
### 実行例
![問題1の実行例](https://i.imgur.com/uE5rSbA.png)
## 問題2(★★☆)
`HelloServlet.java`に、`DateTimeServlet.java`のページに移動するリンクを、<br>
`DateTimeServlet.java`に、`HelloServlet.java`のページに移動するリンクを作成しなさい。<br>
### 実行例
- HelloServletのページ<br>
![問題2の実行例1](https://i.imgur.com/0HA9bnj.png)<br>
<br>

- DateTimeServletのページ<br>
![問題2の実行例2](https://i.imgur.com/58yVs2W.png)<br>

## 問題3(★★★)
演習2で作成したページを表示するまでの、Webクライアント（Webブラウザ）の動作を説明しなさい。<br>

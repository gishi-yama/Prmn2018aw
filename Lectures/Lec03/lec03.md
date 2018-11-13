# 関連
- [出席フォーム](https://docs.google.com/forms/d/e/1FAIpQLSfv196Cb4AMlbu9TTVrQ_PQhiqbLRlzFvgXforuB__RJWDSSg/viewform?usp=sf_link)
# 演習
`com.example`パッケージ の中に `lec02`パッケージを作成してください。

## フォームの内容を確認するページを作ろう
`lec02`パッケージ の中に `ConfirmationPage.html` と `ConfirmationPage.java` を作成し、<br>
以下のように内容を変更してください。<br>
<br>

ConfirmationPage.html
```html:ConfirmationPage.html
<!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
    <meta charset="UTF-8" />
    <title>Confirmation Page</title>
</head>
<body>
    <h2>入力内容の確認</h2>
    <p>
        タイトル: <span wicket:id="title"></span><br>
        場所: <span wicket:id="place"></span><br>
        <a wicket:id="toFormPage">フォームページに戻る</a>
    </p>
</body>
</html>
```

ConfirmationPage.java
```java:ConfirmationPage.java
package com.example.lec02;

import com.example.lec01.FormPage;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.link.Link;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;

public class ConfirmationPage extends WebPage {

    private IModel<String> titleModel;
    private IModel<String> placeModel;

    public ConfirmationPage(String title, String place) {
        titleModel = Model.of(title);
        placeModel = Model.of(place);

        Label titleLabel = new Label("title", titleModel);
        add(titleLabel);

        Label placeLabel = new Label("place", placeModel);
        add(placeLabel);

        Link<Void> toFormPageLink = new Link<Void>("toFormPage") {
            private static final long serialVersionUID = 1L;

            @Override
            public void onClick() {
                setResponsePage(new FormPage());
            }
        };
        add(toFormPageLink);
    }
}
```
次に、FormPage の 「データ送信」を押したら、ConfirmationPage に移れるように変更を加えます。<br>
`lec01`パッケージ の中の `FormPage.html` と `FormPage.java` を以下のように変更してください。<br>

FormPage.html
```html:FormPage.html
<!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
    <meta charset="UTF-8" />
    <title>FormPage</title>
</head>
<body>
<h2>入力フォーム</h2>
<form wicket:id="form">
    タイトル:<input type="text" wicket:id="title"><br>
    場所<input type="text" wicket:id="place"><br>
    <button type="submit">データ送信</button>
</form>
</body>
</html>
```

FormPage.java
```java:FormPage.java
package com.example.lec01;

import com.example.lec02.ConfirmationPage;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;

public class FormPage extends WebPage {
    private static final long serialVersionUID = 1L;

    private IModel<String> titleModel;

    private IModel<String> placeModel;

    public FormPage() {
        titleModel = Model.of("");
        placeModel = Model.of("");

        Form<Void> form = new Form<Void>("form") {
            private static final long serialVersionUID = 1L;

            @Override
            protected void onSubmit() {
                super.onSubmit();
                String title = titleModel.getObject();
                String place = placeModel.getObject();
                setResponsePage(new ConfirmationPage(title, place));
            }
        };
        add(form);

        TextField<String> titleField = new TextField<>("title", titleModel);
        form.add(titleField);

        TextField<String> placeField = new TextField<>("place", placeModel);
        form.add(placeField);
    }
}
```
http://localhost:8080 にアクセスし、「データ送信」をクリックした後、<br>
FormPage で入力した内容を ConfirmationPage で確認できることを確認してください。<br>
### 実行例
![](https://i.imgur.com/mlhDBiZ.png)

## 仕様変更に強くする
現状では、フォームの入力内容を増やしたとき、ConfirmationPage も変更しなければいけない点が多数存在します。<br>
具体的に書くと
- 引数の追加
- Model の作成
- Label の追加

最低限でもこの3つをこなさなければなりません。拡張しにくく、変更に弱いシステムの出来上がりです。<br>
これでは、オブジェクト指向の設計指針である、オープン・クローズドの原則に反している気もするので、変更を加えます。<br>
<br>
`com.example`パッケージ の中に、`classes`パッケージ を作成してください。<br>
その中に、`Task.java` を作成し、以下のコードを入力してください。<br>
<br>

Task.java
```java:Task.java
package com.example.classes;

import java.io.Serializable;

public class Task implements Serializable {

    private String title;
    private String place;

    public Task() {
        this.title = "";
        this.place = "";
    }

    public String getTitle() {
        return title;
    }

    public String getPlace() {
        return place;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public void setPlace(String place) {
        this.place = place;
    }
}

```

`lec02`パッケージの中に、`CPMFormPage.html` と `CPMFormPage.java` を作成してください。<br>
`CPMFormPage.html` は、`FormPage.html` を コピーしてください。<br>
`CPMFormPage.java` に以下のコードを入力してください。<br>
<br>
CPMFormPage.java
```java:CPMFormPage.java
package com.example.lec02;

import com.example.classes.Task;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.model.CompoundPropertyModel;
import org.apache.wicket.model.IModel;

public class CPMFormPage extends WebPage {
    private static final long serialVersionUID = 1L;

    IModel<Task> taskModel;

    public CPMFormPage() {
        taskModel = new CompoundPropertyModel<>(new Task());

        Form<Task> form = new Form<Task>("form", taskModel) {
            private static final long serialVersionUID = 1L;

            @Override
            protected void onSubmit() {
                super.onSubmit();
                setResponsePage(new CPMConfirmationPage(getModel()));
            }
        };
        add(form);

        TextField<String> titleField = new TextField<>("title");
        form.add(titleField);

        TextField<String> placeField = new TextField<>("place");
        form.add(placeField);
    }
}
```

`lec02`パッケージの中に、`CPMConfirmationPage.html` と `CPMConfirmationPage.java` を作成してください。<br>
`CPMConfirmationPage.html` は、`ConfirmationPage.html` を コピーしてください。<br>
`CPMConfirmationPage.java` に以下のコードを入力してください。<br>
<br>

CPMConfirmationPage.java
```java:CPMConfirmationPage.java
package com.example.lec02;

import com.example.classes.Task;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.link.Link;
import org.apache.wicket.model.CompoundPropertyModel;
import org.apache.wicket.model.IModel;

public class CPMConfirmationPage extends WebPage {

    public CPMConfirmationPage(IModel<Task> model) {
        setDefaultModel(CompoundPropertyModel.of(model));
        add(new Label("title"));
        add(new Label("place"));

        Link<Void> toFormPageLink = new Link<Void>("toFormPage") {
            @Override
            public void onClick() {
                setResponsePage(new CPMFormPage());
            }
        };
        add(toFormPageLink);
    }
}
```
最後に、HomePageにCPMFormPageにアクセスするリンクを作成しましょう。<br>
`<dl>...</dl>`の中に、以下のコードを追加してください。<br>
<br>
HomePage.html
```html:HomePage.html
<dd><a wicket:id="toCPMFormPage">CPMFormPageへ</a></dd>
```
続いて、コンストラクタに以下のコードを追加してください。<br>
<br>
HomePage.java
```java:HomePage.java
Link<Void> toCPMFormPageLink = new Link<Void>("toCPMFormPage") {
    private static final long serialVersionUID = 1L;

    @Override
    public void onClick() {
        setResponsePage(new CPMFormPage());
    }
};
add(toCPMFormPageLink);
```
[http://localhost:8080](http://localhost:8080/) にアクセスし、FormPage、ConfirmationPage と同じ動作であることを確認してください。<br>
<br>
ここのポイントは、モデルのクラスを Model から CompoundPropertyModel に切り替えることで、<br>
コンポーネントのデータの管理場所が変っている点です。<br>
<br>
コンポーネントの wicket:id と Taskインスタンスのフィールド変数名 が一緒であれば、<br>
コンポーネントは自動的にTaskのフィールド変数をデータの取り出し・保存場所として使うように変わっています。<br>
<br>
こうしておけば、もしフォームの入力欄が増えても、増えたwicket:idと同じ名前のフィールド変数をTaskに作るだけで、プログラムの変更が完了します。<br>
<br>
Modelには他にも種類があり、データベースからデータを取り出す場合や、読み取り専用のデータを使う時など、より最適なモデルに切り替えることがあります。<br>
# 課題
実行例のように、種類を入力する項目を追加してください。<br>
<br>
CPMFormPage
<br>
![](https://i.imgur.com/cp3ZlP4.png)<br>
<br>
CPMConfirmationPage<br>
<br>
![](https://i.imgur.com/b4MhbgS.png)<br>
<br>
[ヒント](https://scrapbox.io/Prmn2018aw/%E8%AA%B2%E9%A1%8C%E3%81%AE%E3%83%92%E3%83%B3%E3%83%88)
# 発展問題
今回のは、調べないとできません。<br>
更に、Wicket はマイナーすぎて出てこないかもしれません。<br>
辛抱してください。
## 問題1
課題の内容を変更し、種類を選択できるように変更してください。<br>
### 実行例
CPMFormPage<br>
![](https://i.imgur.com/cHKagva.png)<br>
CPMConfirmationPage<br>
![](https://i.imgur.com/EhmmSlT.png)<br>
## 問題2
タイトル欄を入力必須項目にしてください。<br>
入力せずに「データ送信」をクリックしたら、以下の画像のようになるようにしてください。<br>
### 実行例
![](https://i.imgur.com/IoEl5jc.png)<br>

# 演習
`com.example`パッケージ の中に `lec02`パッケージを作成してください。

## フォームの内容を確認するページを作ろう
`lec02`パッケージ の中に `ConfirmationPage.html` と `ConfirmationPage.java` を作成し、<br>
以下のように内容を変更してください。<br>

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
```ConfirmationPage.java
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
            private static final long serialVerionUID = 1L;

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
    日付<input type="date" wicket:id="date"><br>
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

        // title を入力する input type="text"用のコンポーネント
        TextField<String> titleField = new TextField<>("title", titleModel);
        form.add(titleField);

        // place を入力する input type="text"用のコンポーネント
        TextField<String> placeField = new TextField<>("place", placeModel);
        form.add(placeField);
    }
}
```

# 演習
## Wicket で前回のようなページを作ってみる
`com.example`パッケージの`HomePage.html`と`HomePage.java`を以下のように変更してください。<br>

HomePage.html
```java:HomePage
<!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
    <meta charset="UTF-8">
    <title>HomePage</title>
</head>
<body>
    <p>
        こんにちは！今日は<span wicket:id="dateLabel">ここに日付が表示されます</span>です<br>
        <span wicket:id="messageLabel">ここにメッセージが表示されます</span><br>
    </p>
</body>
</html>
```

HomePage.java
```HomePage.java
package com.example;

import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;
import org.apache.wicket.request.mapper.parameter.PageParameters;

import java.time.LocalDate;
import java.time.LocalTime;

public class HomePage extends WebPage {
	private static final long serialVersionUID = 1L;

	String message;
	LocalDate date = LocalDate.now();
	LocalTime localTime = LocalTime.now();

	public HomePage(final PageParameters parameters) {

		IModel<String> dateModel = Model.of(date.toString());
		Label dateLabel = new Label("dateLabel", dateModel);
		add(dateLabel);

		message = getMessage();
		IModel<String> messageModel = Model.of(message);
		Label messageLabel = new Label("messageLabel", messageModel);
		add(messageLabel);
	}

	public String getMessage() {
		int second = localTime.getSecond();
		if (second % 3 == 0) {
			message = "笑う門には福来る";
		} else if (second % 5 == 0) {
			message = "好きこそ物の上手なれ";
		} else if (second % 7 == 0) {
			message = "金の草鞋で尋ねる";
		} else if (second % 11 == 0) {
			message = "麒麟も老いては駑馬に劣る";
		} else {
			message = "今日も頑張ろう！";
		}

		return message;
	}
}
```

入力したら、`main/test/java` の `com.example` にある `Start.java` を実行しましょう。<br>
その後、http://localhost:8080 にアクセスして、実行結果のように表示されていることを確認してください。<br>

### 実行結果
![演習1の実行結果](https://i.imgur.com/M8YglSH.png)

## フォームを作成する
`com.example`パッケージ　に　`lec01`パッケージを作成し、<br>
そこに、`FormPage.html` と `FormPage.java` を作成し、以下のように入力してください。<br>

FormPage.html
```FormPage.html
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
    <button type="submit">データ送信</button>
</form>
</body>
</html>
```

FormPage.java
```FormPage.java
package com.example.lec01;

import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;

public class FormPage extends WebPage {
    private static final long serialVersionUID = 1L;

    // title の値を格納する Model
    private IModel<String> titleModel;

    public FormPage() {
        titleModel = Model.of("");

        // Formタグ 用の Formコンポーネント
        Form<Void> form = new Form<Void>("form") {
            private static final long serialVersionUID = 1L;

            @Override
            protected void onSubmit() {
                // データ送信ボタンがクリックされた時の処理
                super.onSubmit();
                String title = titleModel.getObject();
                System.out.println("タイトル: " + title);
            }
        };
        add(form);

        // title を入力する input type="text"用のコンポーネント
        TextField<String> titleField = new TextField<>("title", titleModel);
        form.add(titleField);

    }
}
```
`HomePage.html` 内の`</p>`の後に以下を追加してください。<br>
<br>
HomePage.html
```HomePage.html
<dl>
	<dt>リンク</dt>
	<dd><a wicket:id="toFormPage">FormPageへ</a></dd>
</dl>
```
その後、`HomePage.java` のコンストラクタ内に以下を追加してください。<br>
<br>
HomePage.java
```HomePage.java
Link<Void> toFormPageLink = new Link<Void>("toFormPage") {
	private static final long serialVersionUID = 1L;
	@Override
	public void onClick() {
		setResponsePage(new FormPage());
	}
};
add(toFormPageLink);
```
`Start.java`を再度実行して、フォームに何か入力して、データ送信をクリックしたら、<br>
IntelliJに実行例のように入力した内容が表示されることを確認してください。<br>

### 実行例
```
タイトル: wicket_handson
```

# 課題
`FormPage.html` と `FormPage.java` フォームに場所の入力欄を作成し、<br>
データ送信をクリックしたら実行例のように表示されるようにしてください。<br>

### 実行例
```
タイトル: wicket_handson
場所: G202
```

# 発展問題
今回の発展問題は、次回の演習で作成するページの一部です。<br>
そのため、出来なくても全然大丈夫です。<br>
## 問題1
`com.example`パッケージ に `lec02`パッケージ を作成してください。<br>
`lec02`の中に、`ConfirmationPage.html` と `ConfirmationPage.java` を作成してください。<br>
`FormPage.html` と `FormPage.java` を、「データ送信」をクリックしたら、<br>
ConfirmationPage で入力した内容を確認できるように変更してください。<br>

### 実行例
![問題1の実行例](https://i.imgur.com/qbE1CFV.png)

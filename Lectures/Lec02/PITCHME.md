### Wicketの特徴
- 1ページにつき1クラス
- コンポーネントを配置していく
- コンポーネントは入れ子にできる

---

### 1ページにつき1クラス
![](https://i.imgur.com/UfZWrDN.png)
![](https://i.imgur.com/F6Cj2Ls.png)<br>
※同名でないとダメ！

---

### コンポーネントを配置していく
```java:FormPage.java
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

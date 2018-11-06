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
public FormPage() {
    titleModel = Model.of("");

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

    TextField<String> titleField = new TextField<>("title", titleModel);
    form.add(titleField);

}
```
演習2のFormPageコンストラクタ

---

```java:FormPage.java
public FormPage() {
    titleModel = Model.of("");

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

    TextField<String> titleField = new TextField<>("title", titleModel);
    form.add(titleField);

}
```
@[17](TextFieldコンポーネント)

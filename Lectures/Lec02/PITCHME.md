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

## コンポーネントを配置していく

---

### 演習2のFormPageのコンストラクタ

```java:FormPage.java
public FormPage() {
    titleModel = Model.of("");

    Form<Void> form = new Form<Void>("form") {
        private static final long serialVersionUID = 1L;

        @Override
        protected void onSubmit() {
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

---

### Formコンポーネント
```
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

```
@[12](add()を忘れずに!)

---

## コンポーネントは入れ子にできる

---

### 演習2のForm

![](https://i.imgur.com/oPwZkfj.png)<br>



```html:FormPage.html
<form wicket:id="form">
    タイトル:<input type="text" wicket:id="title"><br>
    <button type="submit">データ送信</button>
</form>
```

---
### FormPageコンストラクタ内では

```java:FormPage.java
public FormPage() {
    titleModel = Model.of("");

    Form<Void> form = new Form<Void>("form") {
        private static final long serialVersionUID = 1L;

        @Override
        protected void onSubmit() {
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
@[16](TextFieldコンポーネントを作成）<br>
@[17](formにadd()している！)

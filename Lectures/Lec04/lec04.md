# 演習
`com.example` に `lec03`パッケージを作成してください。
## 登録内容の一覧ページを作成する
FormPage で登録した内容のタイトルを一覧できる作成します。<br>
とはいえ、実際に情報を登録・管理・操作する場合はデータベースというものについての勉強が必要になります。<br>
残念ながらそんな余裕をかましている時間はないので、今回は `偽物(Dummy)データベース.java` なるものを作成し、<br>
それがデータベースであるという体で話を進めていくので、よろしくお願いします。<br>
<br>
`classes`パッケージ に、`DummyDataBase.java` を作成し、以下のプログラムを入力してください。<br>
<br>
DummyDataBase.java
```java:DummyDataBase.java
package com.example.classes;

import java.util.ArrayList;
import java.util.List;

public class DummyDataBase {

    private List<Task> tasks;


    public DummyDataBase() {
        tasks = new ArrayList<>();
        tasks.add(new Task("wicket handson", "G202", "prmn"));
        tasks.add(new Task("algorithm & programing", "G201", "class"));
        tasks.add(new Task("Java Programing", "G202", "class"));
        tasks.add(new Task("KH introduction", "Project Room", "conference"));
    }

    /**
     * このクラスのコンストラクタで登録している4件の情報(Task)が、
     * データベースに登録されていたと仮定し、
     * その情報を得るためのメソッド
     *
     * @return tasks
     */
    public List<Task> getTasks() {
        return tasks;
    }
}
```
次に、これらの情報を取り出して、箇条書きで表示するページを作成します。<br>
演習では、タイトルだけを取り出します。<br>
<br>
`lec03`パッケージ の中に、`ViewPage.html` と `ViewPage.java` を作成し、以下のプログラムを入力してください。<br>
<br>
ViewPage.html
```html:ViewPage.html
<!DOCTYPE html>
<html xmlns:wicket="http://wicket.apache.org">
<head>
    <meta charset="UTF-8" />
    <title>ViewPage</title>
</head>
<body>
<h2>To-Do List</h2>
    <ul wicket:id="tasks">
        <li><span wicket:id="title"></span></li>
    </ul>
</body>
</html>
```
ViewPage.java
```java:ViewPage.java
package com.example.lec03;

import com.example.classes.DummyDataBase;
import com.example.classes.Task;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.markup.html.basic.Label;
import org.apache.wicket.markup.html.list.ListItem;
import org.apache.wicket.markup.html.list.ListView;
import org.apache.wicket.model.IModel;
import org.apache.wicket.model.Model;

import java.util.List;

public class ViewPage extends WebPage {

    DummyDataBase dummyDataBase;

    public ViewPage() {
        dummyDataBase = new DummyDataBase();
        List<Task> tasks = dummyDataBase.getTasks();
        IModel<List<Task>> tasksModel = Model.ofList(tasks);
        ListView<Task> tasksView = new ListView<Task>("tasks", tasksModel) {
            private static final long serialVersionUID = 1L;

            @Override
            protected void populateItem(ListItem<Task> item) {
                String title = item.getModelObject().getTitle();
                Label titleLabel = new Label("title", title);
                item.add(titleLabel);
            }
        };
        add(tasksView);
    }
}
```

## 課題
「場所」と「種類」の情報を取り出して、表示されるようにしてください。<br>
<br>
実行例<br>
![](https://i.imgur.com/w1tNme3.png)<br>
<br>
## 発展問題
課題のコードを改変して、表形式で表示されるようにしてください。<br>
<br>
実行例<br>
![](https://i.imgur.com/AQEKAz9.png);

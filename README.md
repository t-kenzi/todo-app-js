# todo-app-js
Javascriptで作成する学習様ToDoリストアプリ

## 目的
- Javascritpのみを用いてToDoアプリを作成して基本を理解する
- データはlocalstrageに保存する
- 基本的なDOM操作、イベント処理の扱いを理解する

## 機能一覧
- タスクの新規登録　　：入力フォームから新規タスクを登録
- タスクの編集　　　　：登録済みのタスクを変更可能（タイトル、優先度など）
- タスクの削除　　　　：登録したタスクの削除が可能
- 完了フラグ切り替え　：タスクの完了済みとしてチェックできるかつ表示も変更できる
- 優先度の指定　　　　：高・中・低で重要度を指定できる

## 使用技術
- HTML5 /CSS3
- Javascript

## 学習メモ（任意で追加）
プロジェクトと学んだことや注意点を追記する

- JavaScriptはDOMを操作するもの
アプリの説明
HTML：
タスクの入力フォームと追加したタスクの一覧を表示する箇所を作成した

Javascript：
タスク入力欄、フォーム（submit処理を受け持つ）、タスクリストの表示先の各要素を取得し、それぞれを変数として定義します。
```
    const taskValue = document.getElementsByClassName('task_value')[0];
    const taskForm = document.getElementById('taskForm');
    const taskList = document.getElementsByClassName('task_list')[0];
```

取得した要素にdeleteボタンを追加して一覧に表示する
- addTask関数を定義:listItem変数に新規li要素を追加
- 追加した要素をspanタグでグルーピングします。
- deleteボタンを指定したフォーマットでボタン形式で作成し、クリックされた際にdeleteTask関数を実行するようにします
- リストにタスクとdelteボタンを1セットで追加します。
```
    // タスクを追加する処理
    const addTask = (task) => {
      const listItem = document.createElement('li');

      // タスク内容（テキスト）
      const taskText = document.createElement('span');
      taskText.textContent = task;

      // 削除ボタン
      const deleteButton = document.createElement('button');
      deleteButton.textContent = 'Delete';
      deleteButton.style.marginLeft = '8px';
      deleteButton.style.color = 'red';

      deleteButton.addEventListener('click', () => {
        deleteTask(deleteButton);
      });

      // リストアイテムにテキストとボタンを追加
      listItem.appendChild(taskText);
      listItem.appendChild(deleteButton);
      taskList.appendChild(listItem);
    };
```

-deleteボタンがクリックされるとその要素が削除されます
1. ボタンの一番近くの <li>（＝タスク1つ分）を見つける
2. タスクリストからその <li> を削除する
```
    const deleteTask = (deleteButton) => {
      const listItem = deleteButton.closest('li');
      taskList.removeChild(listItem);
    };
```

フォームが送信（submit）されると、デフォルトのリロード動作を防止します。
入力されたタスクが空白の場合はアラートを表示して追加処理を中断します。
タスクが入力されていれば、addTask関数を呼び出してタスクリストに追加し、入力欄をクリアします。
```
    taskForm.addEventListener('submit', (evt) => {
      evt.preventDefault();
      const task = taskValue.value.trim();
      if (task === '') {
        alert('タスクを入力してください');
        return;
      }
      addTask(task);
      taskValue.value = '';
    });
```

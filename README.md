# PRチェックリスト
自分がPRを作成したとき、または誰かのPRをレビューするときに参照する用

- パフォーマンス
    - DBへの過剰なアクセスがされていないか
      - created_at、updated_atをORMや生SQL内で生成・更新してしまっていないか
    - メモリの過剰な利用がされていないか
    - loop周りで上記のような状態になっていないか
      - 何度も同じクエリを発行していないか。キャッシュで対応できないか
      - 何度も同じクラスをnewしていないか。一度のnewのみで対応できないか
- 可読性
    - if文の条件は読みやすいか
        - わかりずらい場合、ド・モルガンの法則を使用して改善できるか
    - 命名規則に従ってるか
        - 使用すべきでない単語を使っていないか（参考： [うまくメソッド名を付けるための参考情報](https://qiita.com/KeithYokoma/items/2193cf79ba76563e3db6)）
            - e.g. ) set, check、judge
    - 変数・関数名にタイポはないか
    - 変数・関数名において、複数形を正しく使用できているか
    - 引数と返り値において、型を指定しているか（言語にもよる）
    - マジックナンバーはないか
- 設計
    - クラス
        - 不用意に値を変更できるようになってないか (完全コンストラクタで実装できているか）
        - できるだけ疎結合な実装になっているか
        - nullを返却するメソッドを書いていないか。nullを返却するメソッドを使用している場合、その対応ができているか
          - e.g.) Laravelのfindメソッドはnullを返却するので、null返却時の対応が必要。もしくはfindOrFailに変更
        - エラーハンドリングが正しく行われてるか
        - 実装内容が正しい責務を負っているか
            - e.g.) 3層アーキテクチャにおいて、service層はビジネスロジックのみを担保しているか。
            - e.g.) データをclearするコマンド内でseedingまでしてしまっていないか
    - フロントエンドとバックエンド
        - フロントの表示内容を変更するコードはフロントで実装しているか
            - e.g) datetimeに関して、バックエンドでUTC、フロントでローカルの時刻に変更しているか（PJにもよる）
    - DBへの処理
        - 連続したDBへの処理の場合、トランザクションをはっているか。途中で失敗した場合、ロールバックしているか。
        - 連続したDBへの処理の場合、正しくロックが行われているか
    - REST API
        - 想定外のリソースを返却してしまっていないか、URIの設計が正しいか
            - e.g) 記事内のコメントを取得する際に `/article/1 ` で取得できてしまう等
    - DRY原則
        - 既存コードで同様の実装はないか
            - e.g) 消費税を計算するメソッドが複数のクラスの中に実装されている
    - YAGNI原則
        - 今現在不要なコードを書いてないか、コメントアウトで対応してしまっていないか
- セキュリティ
    - ログインユーザのIDをAPIのリクエストパラメータに含めてしまっていないか
- バグ
    - デグレはおきていないか。おきそうな場合、該当箇所のテストは実行したか
    - 使用した関数は非推奨となってないか
    - 呼び出したクラスのインスタンス変数を操作する場合、イミュータブルなクラスかどうか。ミュータブルな場合、対策できているか
    - 境界値において、正しく動作するか
- コメント
    - コード自体の説明だけのコメントになってないか
    - コードを書いた根拠が分かりづらい場合に、その根拠を書いているか
    - 変数名、関数名が省略形になっている場合、コメントに説明を書いているか
    - メソッド、クラス単位でドキュメントをかいているか
        - Laravelの場合はモデル定義の場合とモデルクラス使用時にコード補完を効かせているか
    - アノテーションを正しく活用できているか（参考：[TODO: 以外のアノテーションコメントをまとめた](https://qiita.com/taka-kawa/items/673716d77795c937d422)）
        - e.g.) TODO、FIXME、HACK、REVIEW
- テスト（E2Eテスト）
    - 冪等性を担保しているか
        - 帯域幅によって結果が変わるテストになっていないか
        - 実行する場所によって結果が変わるテストになっていないか
          - e.g.) JS dateクラスはタイムゾーンによって結果が変わる
        - 〜秒待つ、といった関数を不用意に使用していないか
        - 各環境での実行において事前準備が必要な場合、その旨をPR等に記載したか
    - 

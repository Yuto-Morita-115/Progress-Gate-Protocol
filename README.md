技術仕様書: Progress Gate (プログレスゲート) 

副題：ユーザー進捗同期型 動的ナレッジ制御アーキテクチャ  

Document ID: PG-SPEC-001  

Version: 1.0.0 (Proposed Standard) 

 Status: Request for Comments (RFC) 

 


https://github.com/user-attachments/assets/b4ddb355-9060-422d-bf3c-20a9e8c80a11


1. はじめに (Introduction) 

本ドキュメントは、生成AI（RAGシステム）における「意図しない情報の早期開示（ネタバレ、学習範囲外の回答、権限外のアクセス）」を物理的に防止するための新しいアーキテクチャ、**「Progress Gate（プログレスゲート）」**の仕様を定義・提唱するものである。 

私は、本技術を特定の企業が独占する特許としてではなく、エンターテインメント、教育、ビジネスにおけるAI利用の「安全性」と「体験価値」を最大化するための標準プロトコルとして広く公開する。 

 

2. 背景と課題 (Background & Problem) 

従来のRAG（Retrieval-Augmented Generation）システムには、構造的な欠陥が存在した。 

情報の過剰供給: ユーザーの質問に関連すれば、物語の結末や、習熟度を超えた難解な情報まで検索してしまい、LLMに渡してしまう。 

 

プロンプト制御の限界: 「ネタバレしないで」と指示しても、LLMの確率的な挙動により、文脈から事実が漏洩（Leakage）する事故が防げない 。    

体験の非同期: 既読ユーザーと未読ユーザーが混在するコミュニティにおいて、安全な対話空間を提供できない。 

結論: プロンプトエンジニアリング（指示）によるソフトな防御ではなく、**ナレッジベースへのアクセス自体を物理的に制御するハードな防御（Physical Data Isolation）**が必要である 。    

 

3. システム構成 (System Architecture) 

Progress Gateは、LLMの前段に配置される「情報の関所（Gate）」として機能する。 

<img width="393" height="852" alt="iPhone 16 - 61" src="https://github.com/user-attachments/assets/7565fda8-0551-4967-a5aa-c308a4caa784" />
<img width="393" height="852" alt="iPhone 16 - 62" src="https://github.com/user-attachments/assets/5584f0d7-4afc-4590-bbed-fd3afdc49740" />


 

3.1. コア・コンポーネント 

本システムは、以下の3つの主要モジュールで構成される。 

Progress Tracker（進捗管理モジュール） 

ユーザーごとの現在のステータス（読了話数、学習レベル、権限ランク）をリアルタイムに追跡・更新する 。    

 

Knowledge Tagger（ナレッジタグ付け） 

ベクトルデータベース内のすべてのチャンク（情報断片）に対し、開示条件となる「条件タグ（巻数、難易度、機密レベル）」を付与する 。    

The Gate（動的フィルタリング実行部） 

独自の比較アルゴリズム: 検索された情報（T）とユーザー状態（P）を照合する。 

判定ロジック: T>P （タグが進捗を超えている）場合、その情報を**物理的に遮断（Drop）またはマスク処理（Mask）**し、LLMへのコンテキスト注入を阻止する 。    

 

4. 機能仕様 (Functional Specifications) 

4.1. サプライズ保護モード (Spoiler Protection) 

Progress Gateは、情報を隠す際の挙動として以下のモードを定義する 。    

完全隠蔽モード (Hidden Mode): 該当する情報の存在自体を消去し、「その事実は存在しない」前提で回答を生成する。 

期待煽りモード (Teaser Mode): 「その先には驚くべき展開が待っています」と、未読であることを示唆しつつ期待感を高める 。    

4.2. コミュニティ同期 (Community Sync) 

複数ユーザーが参加するチャットルームにおいて、参加者の中で**「最も進捗が遅いユーザー」**を基準（Lowest Common Denominator）としてコンテキストを制限、または個別の端末ごとに表示を出し分ける（Individual Masking）機能をサポートする 。    

 

5. ユースケースと拡張性 (Use Cases & Scalability) 

Progress Gateの真価は、情報の「段階的解禁」によるユーザー体験のパーソナライズにある。 

5.1. 次世代アダプティブ・ラーニング (Education & Quiz) 

課題: 従来のAI問題集は、未習得の範囲の解法をうっかり提示してしまったり、答えそのものを即座に教えてしまうため、学習者の「考えるプロセス」を奪うリスクがあった。 

 

Progress Gateによる解決: 

習得度連動型クイズ: ユーザーの進捗（例：数学ⅠA修了）に応じて、AIが参照できる公式や解法を制限。未習得の知識を使わないと解けない問題は出題せず、そのレベルで「今解ける最高難易度の問題」を動的に生成する。 

段階的ヒント・ゲート: 最初に「解き方の指針（Gate 1）」、次に「使う公式（Gate 2）」、最後に「計算のステップ（Gate 3）」と、ユーザーの試行錯誤に合わせてヒントを小出しに制御。答えを「教えすぎる」AIから、自走を助ける「伴走型AI」へ進化させる。 

 

5.2. 没入型エンターテインメント (Entertainment & Gaming) 

課題: 長編作品やミステリーゲームにおける、AIとの対話による決定的なネタバレ。 

Progress Gateによる解決: 

感情同期型ファンチャット: ユーザーが読了した「その時点の熱量」をAIが共有。未来の展開を知らないAIと、当時のハラハラ感を分かち合える体験を提供。 

TRPG/シナリオ制御: プレイヤーがまだ見つけていない証拠品や秘密（進捗）に基づいて、ゲームマスターAIが提供する情報を物理的に遮断。情報の非対称性を維持し、ゲームの緊張感を守る。 

5.3. 専門職教育・ビジネス（Enterprise & Training） 

課題: 新人研修における情報過多や、社内マニュアルの機密保持。 

Progress Gateによる解決: 

ステップアップ型マニュアル: 新入社員の研修進捗に合わせ、高度な操作手順や機密性の高い情報を段階的に開放。習熟度試験をクリアするごとに「門（Gate）」が開き、参照可能な社内ナレッジが拡張される。 

失敗防止ガイド: 料理や工作などのDIY分野で、初心者のレベルでは「危険な工程」を伏せ、安全な代替案のみを提示。レベルアップに合わせてプロの技法を開放する。 

 

6. ライセンスと社会実装 (Vision) 

Our Mission 

「全人類の『ワトソン』を創る」 人が一生、挫折せず、事故に遭わず、未知なる驚きを楽しみ続けられる世界へ。 

Open Standard Proposal 

私は、Progress Gateを**「デジタルコンテンツにおける感動と安全の同期レイヤー」**と位置付けている。 

本アーキテクチャは、既存のRAGパイプラインに「フィルタリング・ミドルウェア」として差し込むだけで実装可能。電子書籍アプリの既読フラグ、LMS（学習管理システム）の修了ログ、SaaSの権限管理データベースなど、既存のあらゆる「進捗ソース」をAPI経由でゲートの鍵にできる。 

 本仕様は、あらゆるサービスプロバイダーによる実装・採用を歓迎する。    

 

7. リポジトリ情報 (Repository Info) 

Concept Verification:[figmaプロトタイプを体験する](https://www.figma.com/proto/erGBjPVvLJsGY8ObVqCDWU/...)


　　　　 ※ログイン不要。スマホのブラウザ（Safari/Chrome）でそのまま動かせます。 

Demo Video:  [Coming soon on X (Twitter)] [@eoqqvhkmur42233](https://x.com/@eoqqvhkmur42233)

Author:森田 悠斗 （([@eoqqvhkmur42233](https://x.com/@eoqqvhkmur42233))）] 
[Speaker Deck](https://speakerdeck.com/moritayuto/progress-gate-aino-jiao-esugi-wofang-guci-shi-dai-ragzhi-yu-purotokoru)

Feel free to contact me for collaborations or feedback! （コラボレーションやフィードバックはお気軽に！） 

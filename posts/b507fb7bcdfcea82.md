---
title: 'AWS re:Invent 2025 - Quick catch up to GenAI on AWS (GBL103)'
published: true
description: 'In this video, Shimizu, a Senior Manager of Architects at AWS, delivers a session titled "Quick Catch up to GenAI on AWS." He explains the framework of generative AI, focusing on foundation models (基盤モデル) that are trained on terabyte-scale data and can perform various tasks flexibly, unlike traditional models limited to specific tasks. The session introduces Amazon Bedrock, a service that allows users to easily access multiple vendor models and select the most suitable one based on characteristics like size, customization capabilities, security, and pricing. Shimizu highlights Amazon Nova, Amazon''s own model family offering various specifications from low to high performance, supporting text, images, and videos. He distinguishes between assistants and agents, explaining that agents can autonomously break down complex tasks, plan execution steps, and handle errors without human intervention, unlike traditional workflows. The presentation covers Amazon Q, which includes agent functionality for software development, enabling developers to move from coding to production-level development while automating documentation creation. He also mentions Transform capabilities for legacy system modernization and security services, encouraging attendees to explore these services and join the Japan session scheduled for Thursday afternoon.'
tags: ''
cover_image: 'https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/0.jpg'
series: ''
canonical_url: null
id: 3092788
date: '2025-12-08T17:51:39Z'
---

**🦄 Making great presentations more accessible.**
This project aims to enhances multilingual accessibility and discoverability while maintaining the integrity of original content. Detailed transcriptions and keyframes preserve the nuances and technical insights that make each session compelling.

# Overview


📖 **AWS re:Invent 2025 - Quick catch up to GenAI on AWS (GBL103)**

> In this video, Shimizu, a Senior Manager of Architects at AWS, delivers a session titled "Quick Catch up to GenAI on AWS." He explains the framework of generative AI, focusing on foundation models (基盤モデル) that are trained on terabyte-scale data and can perform various tasks flexibly, unlike traditional models limited to specific tasks. The session introduces Amazon Bedrock, a service that allows users to easily access multiple vendor models and select the most suitable one based on characteristics like size, customization capabilities, security, and pricing. Shimizu highlights Amazon Nova, Amazon's own model family offering various specifications from low to high performance, supporting text, images, and videos. He distinguishes between assistants and agents, explaining that agents can autonomously break down complex tasks, plan execution steps, and handle errors without human intervention, unlike traditional workflows. The presentation covers Amazon Q, which includes agent functionality for software development, enabling developers to move from coding to production-level development while automating documentation creation. He also mentions Transform capabilities for legacy system modernization and security services, encouraging attendees to explore these services and join the Japan session scheduled for Thursday afternoon.

{% youtube https://www.youtube.com/watch?v=FizAdSRkClY %}
; This article is entirely auto-generated while preserving the original presentation content as much as possible. Please note that there may be typos or inaccuracies.

# Main Part
[![Thumbnail 0](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/0.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=0)

### Introduction and Foundation Models: Understanding the Framework of Generative AI

皆さん、こんにちは。本日はこちらのセッション、Quick Catch up to GenAI on AWSということで、最後までよろしくお願いいたします。

[![Thumbnail 20](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/20.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=20)

まず自己紹介ですが、私清水と申します。アーキテクトのシニアマネージャーをしておりまして、社会インフラへのお客様を担当しております。趣味はモノづくりとなっておりまして、AWS Summitや東京でもありましたが、カメラで撮影したものをロボットアームが自動でピアノを弾くというようなものなど、ちなみにありがたいことにこちらのAWS Summitや東京の現地でも大変ありがとうございました。

[![Thumbnail 90](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/90.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=90)

早速こちらのセッションに入っていきたいと思うところなんですが、まず生成AIの大きな枠組みについて少し触れておきたいと思います。生成AIの大きな枠組みということで、まず深層学習というものがあって、その中にGenerative AIというものがあるわけですけれども、このテラバイト級のデータ、マクロなデータを使って学習し続けて、希望のパラメータを持つ基盤モデルというものがまさにこの基盤モデルというものになります。

[![Thumbnail 120](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/120.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=120)

この基盤モデルというものがどういうものかというと、従来ですとラベル付けされたデータを使って学習させることでモデルを作っていくという形でしたが、特定のタスクに限定されたものでした。テキストの要約とか何か文章を書くとか、あるいは特定の内容があるとか限られていました。

[![Thumbnail 150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/150.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=150)

一方で、この基盤モデルというものは特定のタスクに限定されたものではなくて、汎用的に使えるようになりますので、より人間に近い柔軟性ができることになります。この基盤モデルなんですけれども、ニューラルネットワークに様々なデータを学習させることができます。

[![Thumbnail 170](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/170.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=170)

例えばテキストをニューラルネットワークに学習させれば、オンゴーイングでデジタルなものであったり、あるいはニューラルネットワークにテキストを学習させたものであれば、そのテキストの要約や議事録の作成といったアプリケーションができるわけです。テキストやサンプルのプログラムコード、画像、動画といった様々なデータを学習させることができます。

[![Thumbnail 210](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/210.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=210)

例えばこちらはテキストから動画を生成するというものなんですが、ジューシーなチーズバーガー、溶けたチーズ、フライドポテト、そしてコーラといったダイナーのテーブルの上で撮影した映画のようなドリーショットという形で情報を与えることによって、ハンバーガーのような動画が生成できます。

[![Thumbnail 230](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/230.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=230)

### Amazon Bedrock and Nova: Model Selection, Agents, and Multi-Agent Collaboration

こちらはAmazon SageMakerというサービスがございますが、例えばInferenceやAPIといったものを使ってエリアをデコレーションすることができます。皆様のスキルに応じてモデルを使うというようなことで、こちらのAmazon Bedrockというサービスがございます。こちらの中で既に皆様がよく使うものになります。

[![Thumbnail 310](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/310.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=310)

[![Thumbnail 320](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/320.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=320)

AmazonのBedrockを使うことで、様々なモデルを簡単に使うことができます。Amazonだけではなくて、色々なベンダーのモデルの中から最適なものを選択することができます。それぞれのモデルは、それぞれ特徴があります。例えば、大きいものもあれば小さいものもありますし、カスタマイズできるものもあります。セキュリティやプライバシー、そして既存システムに対してどのように使うかといった観点から選ぶことができます。

そうは言いますが、タスクがそれほど単純ではなくて、複雑なタスクを解決するためのエージェントというものもあります。これは複雑な要件に対応できるものになります。マルチエージェントというものもありますので、それについても後ほど説明していきたいと思います。

[![Thumbnail 390](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/390.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=390)

では、どのようにモデルを選択するかということですが、様々なモデルの中から適切なものを選ぶ必要があります。やはり、まずはプライシングがございますので、例えば安いものから高いものまで、より性能の高い分野に対応できるモデルも含めて選ぶことができます。

[![Thumbnail 430](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/430.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=430)

また、適切に選ぶモデルの中には、Amazonと同じモデルも含まれております。それがこちらになります。AmazonのNovaと呼ばれるモデルになります。様々なスペックのものから、低スペックから高スペックまで、様々なタスクに対応できるモデルを選択することができます。また、画像や動画にも対応しておりまして、最もAmazonのNovaで最新のモデルがこちらになります。

[![Thumbnail 490](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/490.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=490)

まず、Amazonの様々なものの中から、いくつか機能があるものをご紹介します。例えば、ここではアシスタントのようなもの、例えばテキストで要約するとか、画像を生成するといったことができます。生成されたそのハイライトをまとめたりすることができると思いますが、ここでは画像の左側にアシスタントというものがあります。

また、まだ人間の介入が必要になります。その際に、何かしらのエラーがあった場合、アシスタントが判断できない場合は、人間がサポートしなければ、すぐにそれに進めることができません。ここで今お見せしますのが、右側にありますエージェントになります。

[![Thumbnail 560](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/560.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=560)

このエージェントというのは、高度なシステム自体を組むことができます。エージェントというものは、より高度なことができます。最初に各所のタスクを洗い出して、やるべきことを整理します。このように組むことで、そのタスクを連続して処理することができます。

従来のワークフローですと、どうしても自動的に人間が作り込んだロジックやルールに沿って処理を進めていかなければなりませんでした。そのため、何かエラーが起こると、まさにそこでそのエラーが止まってしまうということになります。そのサポートが必要になってしまいます。

そうですね、誰かが来るとですね、まあそこでリガトマトシマトネノサポートヒトツオストーリーまた。The iPodエージェントバヤネオエヤナジベソイタマカンキョウトソサオコヨネオモコタタタスカトヨコゾトヨガス。例えば、よくあるお任せたばにはれてればテラセキガイトモンデガマハメダエハリsit and gayoザイマムfroデスコタトデストヨコズッコスアmy agentデタバオヨホノアピケスルコトデヨロイアレストランヨロイスセレナアトヨコロハナンヨエコトルトヨコトモス。

[![Thumbnail 660](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/660.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=660)

[![Thumbnail 710](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/710.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=710)

 エココネオエレポオソカオモイマスエコトラガウナノエレポトトアットリマステエロックノキヨガイマエゲントノルリオカソクエルテオイマスand you ニジョヨネニワイツィパセノミアムタタモノガデナトンニニジョハチネニワエルサンジョサンパセントノソキヨムケソクフトヤニコイタゲンティクウェヤand オッキノガオササレトヤレトリアス。ザマタデスネニニジュハチネマデニニチジョギョムニョケイシケテエコノティジュゴパストガコイタゲンティクウェヤヨテナサレトジルit's taking your hand downサレルトyou are aトリアスマコイタデスネサンガアニデポトトセデテキトリアス。エテワイマit イッチェハヤクデスネエルミナサマノソスキコイタエアゲノカツソニワ  ドスレバイノッカトヨコトデマソノヒトスオマサイキアサビトステAmazon uクッスイトマズコショカイサイトモイマス。

[![Thumbnail 760](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/760.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=760)

ニネソソシキアキデスネオクノデタジョヨサレテロオモイムマスガジョレラオデスネカツヨステエヨリケメンナケテマツマリブサホンダoh canonニタメオサブビトナトリアスエコノクイコイトデスネI agentトオッキノガボザエマステエコレッガマチウノmemberトステナカマトステミナサマトホイエルカツオsecureルトユコトニナリマス。マトセナアラデスネエルpraibeto detaワデスネandゼニホゴセクルマソデコラタラデスネベスanswers to the zikas。エコノスネクツクノa agents customizedキオ  ミオスキニーココトヨコモデキスエタクリシアカイトキノスカイマストタヒブヤヨスリアレジガイブヤニョモエキストレコトガキシブスネカイトザスネサトオヨガデマス。

[![Thumbnail 810](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/810.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=810)

[![Thumbnail 860](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/860.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=860)

エッサラニエルquick kuroクイックアウトトツカキマストuhヨウケヨザテレスネククザツナギョウprocessトコロデスネuhジドカエルトユコトモデクイオナリマスエソセサグアノペストユキノスカテタダキマトコイタエヤオカツオサデスネエルチムクラボレザスネサラニココサオッコトガデオニナイマス。エコイタデスエルagentトロカツオテyou know thisネマフクザツナノエテキョウト  ユカテthere was some of them andオヨガキティマエルソオストウニソウテアカイハオセンガグザイマス。エッセンガコヨガjones。エコノヒダリオズデネエジュダノエアinソウテノカイハウシカ  エコドオカルキカタオシエテトエルsit monsurutoザsamurukoドアcodes soトヨコトマtake take itワッセクレルデスコレアニンゲンガエルハンダンthereエルinザソアprogramザtimeプロキアカニトリクムトヨコトヨリマタ。

エエオテオスタバニャサライコスエトヨブンセラタニエナニカシカユヨシマトsteakコドブンセキコドセセtestオサラdocumentオセネエルギルリスtechniqueカツセオヨガデマス。

[![Thumbnail 910](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/910.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=910)

### Amazon Q and Quick Code: Transforming Software Development with Generative AI

ジェネレーティブAIの技術活用についてご紹介します。それでは早速、 AI開発に関するソフトウェア開発の話をいくつかご紹介したいと思います。まずAmazon Developersが提供しているサービスについてですが、何かサポートしませんかということで、お聞きしたいことがある時にお勧めします。こちらのコードベースでテストを実施できるということで、いろいろな機能を使うことができるということです。

それからQuick Codeについてですが、いろいろなエージェントという機能もあります。これらを使って開発環境をIDEとして使っているものもあります。今まではソフトウェア開発というのは、まあ場合によってはコーディングをしないといけないので、まあよくタイプするマリットがよくあると思います。Quick Codeについてはプロトタイプだけでなくプロダクション開発にもこのレベルを引き上げていくことができるということが可能になっています。

エージェントについてですが、これを使ってタスクを決めてから開発を始めようということで、開発工数を削減することができます。まさにコードを書くオントタイプからプロダクションレベルに持っていくことができます。また、このような機能のストーリーもありますので、ドキュメントを自動で作成するということができます。ユーザードキュメントを作成するのが大変だということもありますが、それを解決することができます。

[![Thumbnail 1060](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/1060.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=1060)

そして3つ目がTransformです。カルチャーとしてジェネレーティブAIを使うということで、レガシーなワールドのモダナイゼーションやセキュリティサービスなどについてです。このセッションでご紹介しましたが、また改めてポートフォリオを見ていただければと思います。Amazon Bedrockなど、Amazonのサービスを使うことができます。

[![Thumbnail 1100](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/1100.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=1100)

エージェントが今もAmazon Quickを含めて様々なサービスがあります。Amazon Qを使ってTransformしたものをまとめて見ていただければと思います。多くの情報がありますので、まだ見ていない方もいらっしゃると思いますが、ぜひ検索していただければと思います。スライドを使ってストーリーをまとめていますので、ぜひご覧ください。

[![Thumbnail 1150](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/1150.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=1150)

[![Thumbnail 1180](https://raw.githubusercontent.com/kazuya-iwami/devto-repo/main/posts/images/b507fb7bcdfcea82/1180.jpg)](https://www.youtube.com/watch?v=FizAdSRkClY&t=1180)

それから最後になりますが、木曜日の午後2時から4時まで、ジャパンセッションを実施しますので、ぜひご参加ください。多くの情報を日本語でまとめていますので、セッションにぜひご参加いただければと思います。日本語でのセッションになります。ということでご紹介させていただきました。本日はありがとうございました。


----

; This article is entirely auto-generated using Amazon Bedrock.

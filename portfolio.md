---
title: "HUGOでブログ作成  →  GitHub Pagesで公開する手順"
date: 2017-12-25T14:36:54+09:00　# 作成日時が入る
draft: false　# trueの場合は下書き状態。このままビルドしてもサイトには表示されな
categories: ["HUGO"]
tags: ["GitHub","HUGO"]
---

# Windows環境で開発環境を整えて、VSCode + Hugo + GitHubでポートフォリオを作成する話

## 1. VSCodのインストール
インストール後、VSCodeでtextを開く

## 2. gitインストール

## インストール

ここを参考にインストール
https://qiita.com/shinsumicco/items/a1c799640131ae33c792

Ctrl+Shift+@でターミナルを開き、Gitのパスが通り、インストールされているか確認

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git version
    git version 2.19.1.windows.1

### ブランチ作成
https://guides.github.com/activities/hello-world/
とりまmasterへPullまで確認
portfolioのリポジトリと、master以外のブランチを作成しておく

作成したリポジトリはこちら
https://github.com/tsako2235/portfolio

ブランチはワンクッション置くという意味で、
仮にmarktime（調子を取る）とした、英語は壊滅的な評価しかもっていないので真似しないように。

プロジェクトディレクトリを作成

    mkdir /c/project/portfolio

## 3. VSCodeとGitHubの連携
Ctrl+Shift+Gにて、ソース管理を開いておく
Ctrl+Shift+@にて、ターミナルを開き、Pullしたいディレクトリに移動しておく
 
git pull [リポジトリ] [ブランチ]

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git pull https://github.com/tsako2235/portfolio marktime
    remote: Enumerating objects: 3, done.
    remote: Counting objects: 100% (3/3), done.
    remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From https://github.com/tsako2235/portfolio
    * branch            marktime   -> FETCH_HEAD

あかん、何いってのかわからんｗ
だが、lsの結果きちんとREADMEのマークダウンファイルが落とせてるので
大丈夫なんだろう…

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ ls -l
    total 3
    -rw-r--r-- 1 TAISHI-SAKO 197121 1899 Dec  5 18:38 portfolio.txt
    -rw-r--r-- 1 TAISHI-SAKO 197121   22 Dec  5 18:37 README.md

しかし、ユーザとパスワードが要求されない、gitconfigをみてみよう
うん、こういうのはたいていインストールディレクトリにconfigがないときは
etcにあるものだと思ったけど、それっぽいファイルがない

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/Program Files/Git/etc
    $ ls
    bash.bash_logout  docx2txt.config  inputrc              mtab      nsswitch.conf         pki        protocols  tigrc
    bash.bashrc       fstab            install-options.txt  nanorc    package-versions.txt  profile    services   vimrc
    DIR_COLORS        hosts            msystem              networks  pkcs11                profile.d  ssh

ここでCtrl+@でターミナルに移動できると知る。うむ

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/Program Files/Git/dev
    $ ls
    fd  mqueue  shm  stderr  stdin  stdout

devにもない、ふぅん、ユーザプロファイル系かな、そういうの、嫌いだな（凛ちゃん感）
結局コマンドで調べるいいんだよ、コマンドで、うん？ない
なるほどな、公開されているpublicなGitHubでは参照系については、ユーザもパスも要らないんだな
多分、ここではこう仮定しておこう、pushする時わかるはずだ、いずれわかる、いずれな

    $ git config -l --global
    filter.lfs.smudge=git-lfs smudge -- %f
    filter.lfs.process=git-lfs filter-process
    filter.lfs.required=true
    filter.lfs.clean=git-lfs clean -- %f
    user.name=tsako
    user.email=c.takosa.40@gmail.com
    core.editor='C:\Users\TAISHI-SAKO\AppData\Local\Programs\Microsoft VS Code\Code.exe' --wait

やばいぞ、VSCodeめちゃくちゃ便利やん、Microsoftやるやん、
VSSとか仕事で使ってた時は何死んでんねん、復旧くっそめんどい死ねやこら、とか
C++からExcel弄るツール作ってた時、Excel Objectにバグがあって問い合わせたら
Microsoft日本担当「本国に問い合わせてみます^^」
本国「お前のような小規模なQAに対応している暇はない」とか塩対応されたとか
あったからMicorosoftは糞って思ってたけど…

ついでに、portfolioを別名として追加しておく

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git remote add portfolio https://github.com/tsako2235/portfolio

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git remote -v
    portfolio       https://github.com/tsako2235/portfolio (fetch)
    portfolio       https://github.com/tsako2235/portfolio (push)

試しにこの、portfolio.txtをpushしてみる。
ちょっと待て、masterに登録されたぞ
Ctrl+Shift+Gのその他の操作でステージング後、
コミット後、その他の操作にて、「プッシュ先...」押下後
登録したremoteリポジトリを選択したところ…masterに飛んでいってしまった。

fetchを確認し

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git fetch portfolio -v
    From https://github.com/tsako2235/portfolio
    = [up to date]      master     -> portfolio/master
    = [up to date]      marktime   -> portfolio/marktime

作業ブランチを変更することに、さぁこれでリモート先も変わったかな

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git checkout marktime
    Switched to a new branch 'marktime'
    Branch 'marktime' set up to track remote branch 'marktime' from 'portfolio'.

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)

よしよし、うまくいったぞ、次はコマンドからpushしてみる

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git push portfolio
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 340 bytes | 340.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To https://github.com/tsako2235/portfolio
      62ee769..6bc5685  marktime -> marktime

完璧だ、あとはmasterへのマージpushか…
GitHubのWebでなくともクライアントから勿論できるんだよな？
うむ、どちらかというとmaster側でmarktimeブランチからマージというイメージだな
Web上ではできた

are?

test

commitして

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git commit portfolio.md portfolio.md
    hint: Waiting for your editor to close the file...
    [marktime 6a547ca] committed
    1 file changed, 10 insertions(+)

pushして

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git push portfolio marktime
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 401 bytes | 133.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To https://github.com/tsako2235/portfolio
      6a4e280..6a547ca  marktime -> marktime

ブランチ確認、うんmarktimeだな

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -a
    * marktime
      master
      remotes/portfolio/marktime
      remotes/portfolio/master

ブランチ変更

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git checkout master
    Switched to branch 'master'
    Your branch is behind 'portfolio/master' by 5 commits, and can be fast-forwarded.
      (use "git pull" to update your local branch)

マージ…何か違和感が

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git merge
    Updating 7091341..4314901
    Fast-forward
    portfolio.md  | 143 ++++++++++++++++++++++++++++++++
    portfolio.txt | 257 +++++++++++++++++++++++++++++++++-------------------------
    2 files changed, 288 insertions(+), 112 deletions(-)
    create mode 100644 portfolio.md

んんんん？ちょっとまて、君どことマージしたの！？
もちろんremoteのGitHubに変更はない
あ、あれれ？

もっかい・・marktimeにブランチ変更して

    AISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git checkout marktime
    Switched to branch 'marktime'
    Your branch is up to date with 'portfolio/marktime'.

ファイルをコミットして

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git commit portfolio.md
    hint: Waiting for your editor to close the file...
    [marktime c1a2ad9] a
    1 file changed, 28 insertions(+), 1 deletion(-)

プッシュして

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git push portfolio marktime
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 808 bytes | 269.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To https://github.com/tsako2235/portfolio
      04ab5b3..c1a2ad9  marktime -> marktime

ブランチ変更して

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git checkout master
    Switched to branch 'master'
    Your branch is up to date with 'portfolio/master'.

マージ・・ああ、きちんとブランチ指定ができてなかったからか
きちんとマージできた

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git merge marktime
    Updating 4314901..c1a2ad9
    Fast-forward
    portfolio.md | 67 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++--
    1 file changed, 65 insertions(+), 2 deletions(-)

何か違和感あるな、リモートのmarktimeブランチからリモートのmasterブランチへ
マージしたいんだから、ローカルブランチを変更する点に違和感がある。
直接リモートのmasterでマージできるべきなんじゃないか？
少なくともGitHubのWeb上ではそういう操作だ

とりあえず、ローカルブランチをmaster、marktimeから
portfolio.master portfolio.marktimeに

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -m master portfolio.master

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -m marktime portfolio.marktime

    $ git branch -a
      marktime
    * master
      remotes/portfolio/marktime
      remotes/portfolio/master
      remotes/portfolio/portfolio.marktime

んんんんん？
なるほど、安易にリネームしてはいけないのか…しかしローカルでmasterは…

と、とりあえず、marktimeを戻す。そして紐づけ情報を確認

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -vv
    * marktime         cbda04a [portfolio/marktime] ok
      portfolio.master c5197e5 [portfolio/master: ahead 1] a-

ま、masterも戻そう。

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -vv
    * marktime cbda04a [portfolio/marktime] ok
      master   c5197e5 [portfolio/master: ahead 1] a-

んんんん、あそっかこのテキストを編集しているんだから名称どうとか以前に
先にマージしなさいということね。

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git branch -vv
      marktime 07c97c7 [portfolio/marktime] add message
    * master   b8b0991 [portfolio/master] add message master

よしよし、で、再度名称を変更、する前にこのテキストをきちんとコミットして
それから名称を変更してみよう

結論から言うと、まず、ローカルのマスターブランチは不要
んで、誤ってくっそださい、portfolio.masterやportfolio.marktimeとかいう
GitHub上のブランチは削除

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
    $ git branch -a
      marktime
    * master
      remotes/portfolio/marktime
      remotes/portfolio/master
      remotes/portfolio/portfolio.marktime
      remotes/portfolio/portfolio.master

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -D master
    Deleted branch master (was aef9bbe).

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -a
    * marktime
      remotes/portfolio/marktime
      remotes/portfolio/master
      remotes/portfolio/portfolio.marktime
      remotes/portfolio/portfolio.master

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -vv
    * marktime c8f9fa4 [portfolio/marktime] add message

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git push portfolio :portfolio.marktime
    To https://github.com/tsako2235/portfolio
    - [deleted]         portfolio.marktime

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git branch -a
    * marktime
      remotes/portfolio/marktime
      remotes/portfolio/master
      remotes/portfolio/portfolio.master

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git push portfolio :portfolio.master
    To https://github.com/tsako2235/portfolio
    - [deleted]         portfolio.master

あと、個人的に慣れないうちはコマンド省略しない、これが要点だとわかった。
プッシュ自にリポジトリ名とローカルブランチ名を省略しない、これが重要だ
「git push プッシュ先リポジトリ ローカルのブランチ名:リモートのブランチ名」

## 4. VSCodeとHugo

### とりあえず動かしてみる

さて、まずはHugoのWin版クライアントのダウンロードだ  
https://github.com/gohugoio/hugo/releases  
hugo_0.52_Windows-64bit.zip 7.4 MB

解凍して、パス通して…

ぐぐぐ、そんなことよりターミナルを大きくしたい。Ctrl+@押下した時に  
ターミナルをフォーカスした時ちょびっとパネルが上に大きくなるようにしたい  
Toggle Maximized Panelで解決したわ

ターミナル開いたままエディタに戻るのどうやんねんCtrl+@じゃ消えるやん  

気を取り直してっと以下サイトを参考に進めてみる  
https://qiita.com/satzz/items/e24bd703fc04fb45f7ef  
まずサイトを作るそうだ  

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ hugo new site test
    Congratulations! Your new Hugo site is created in C:\project\portfolio\test.

    Just a few more steps and you're ready to go:

    1. Download a theme into the same-named folder.
      Choose a theme from https://themes.gohugo.io/, or
      create your own with the "hugo new theme <THEMENAME>" command.
    2. Perhaps you want to add some content. You can add single files
      with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
    3. Start the built-in live server via "hugo server".

    Visit https://gohugo.io/ for quickstart guide and full documentation.

で、公開

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ hugo server
    Error: Unable to locate Config file. Perhaps you need to create a new site.
          Run `hugo help new` for details.

だめやん、そうかnewしたあとローカルサーバのディレクトリに移動してないからか？

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ cd test

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ hugo server
    Building sites … WARN 2018/12/16 17:30:09 Found no layout for "taxonomyTerm", language "en", output format "HTML": create a templa
    te below /layouts with one of these filenames: categories/terms.en.html.html, categories/list.en.html.html, categories/terms.html.html, categories/list.html.html, categories/terms.en.html, categories/list.en.html, categories/terms.html, categories/list.html, taxonomy/terms.en.html.html, taxonomy/list.en.html.html, taxonomy/terms.html.html, taxonomy/list.html.html, taxonomy/terms.en.html, taxonomy/list.en.html, taxonomy/terms.html, taxonomy/list.html, _default/terms.en.html.html, _default/list.en.html.html, _default/terms.html.html, _default/list.html.html, _default/terms.en.html, _default/list.en.html, _default/terms.html, _default/list.html
    WARN 2018/12/16 17:30:09 Found no layout for "home", language "en", output format "HTML": create a template below /layouts with one of these filenames: index.en.html.html, home.en.html.html, list.en.html.html, index.html.html, home.html.html, list.html.html, index.en.html, home.en.html, list.en.html, index.html, home.html, list.html, _default/index.en.html.html, _default/home.en.html.html,
    _default/list.en.html.html, _default/index.html.html, _default/home.html.html, _default/list.html.html, _default/index.en.html, _default/home.en.html, _default/list.en.html, _default/index.html, _default/home.html, _default/list.html
    WARN 2018/12/16 17:30:09 Found no layout for "taxonomyTerm", language "en", output format "HTML": create a template below /layouts
    with one of these filenames: tags/terms.en.html.html, tags/list.en.html.html, tags/terms.html.html, tags/list.html.html, tags/terms.en.html, tags/list.en.html, tags/terms.html, tags/list.html, taxonomy/terms.en.html.html, taxonomy/list.en.html.html, taxonomy/terms.html.html, taxonomy/list.html.html, taxonomy/terms.en.html, taxonomy/list.en.html, taxonomy/terms.html, taxonomy/list.html, _default/terms.en.html.html, _default/list.en.html.html, _default/terms.html.html, _default/list.html.html, _default/terms.en.html, _default/list.en.html, _default/terms.html, _default/list.html

                      | EN
    +------------------+----+
      Pages            |  3
      Paginator pages  |  0
      Non-page files   |  0
      Static files     |  0
      Processed images |  0
      Aliases          |  0
      Sitemaps         |  1
      Cleaned          |  0

    Total in 29 ms
    Watching for changes in C:\project\portfolio\test\{content,data,layouts,static}
    Watching for config changes in C:\project\portfolio\test\config.toml
    Serving pages from memory
    Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
    Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
    Press Ctrl+C to stop
    WARN 2018/12/16 17:30:18 Found no layout for "home", language "en", output format "HTML": create a template below /layouts with one of these filenames: index.en.html.html, home.en.html.html, list.en.html.html, index.html.html, home.html.html, list.html.html, index.en.html, home.en.html, list.en.html, index.html, home.html, list.html, _default/index.en.html.html, _default/home.en.html.html,
    _default/list.en.html.html, _default/index.html.html, _default/home.html.html, _default/list.html.html, _default/index.en.html, _default/home.en.html, _default/list.en.html, _default/index.html, _default/home.html, _default/list.html

よしよし、起動と接続もOKだ
次は、何々、テーマを入力とな？テンプレートみたいな？

このチャラカというかいうのがよさそうだ、最終的には双葉の掲示板みたいにしたい
過去に公開していたサイトもそんな感じだったのだ、背景乳白色（POI）、あの茶色文字、
返信で緑文字、あれがいいんだ、目に優しいし  
https://themes.gohugo.io/charaka-hugo-theme/

はぁ、なるほどね、gitでcloneしてきたテーマを反映するわけね

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    git clone https://github.com/natarajmb/charaka-hugo-theme.git ./themes/charaka

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ code ./config.toml

テーマサイトを参考に設定して、サーバ再起動
toml…toml…？そういうフォーマットもあるのか  

    baseurl = "https://example.com"
    languageCode = "en"
    ～
    [[params.social]]
      url = "https://twitter.com/natarajmb"
      fa_icon = "fab fa-twitter"

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ hugo server
    Error: "C:\project\portfolio\test\config.toml:19:6": While parsing config: (19, 6): was expecting token =, but got "_icon" instead

んん？…何かテーマがきちんと参照できてないぽい？
しゃーない、テーマサイトの説明通りやってみるか

https://themes.gohugo.io/charaka-hugo-theme/

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ rm -fr ./charaka

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ git clone https://github.com/natarajmb/charaka-hugo-theme.git
    Cloning into 'charaka-hugo-theme'...
    remote: Enumerating objects: 211, done.
    Receiving objects:  74% (157/211), 1.56 MiB | 1.55 MiB/s   s   ed 0 (delta 0), pack-reused 211
    Receiving objects: 100% (211/211), 1.57 MiB | 1.54 MiB/s, done.
    Resolving deltas: 100% (95/95), done.

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ hugo server
    Error: "C:\project\portfolio\test\config.toml:19:6": While parsing config: (19, 6): was expecting token =, but got "_icon" instead

だめみたいですね
ああ、「Take a look in the exampleSite folder.」て書いてあるやん

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ cd themes/

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ ls
    charaka-hugo-theme

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ cd charaka-hugo-theme/

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme (master)
    $ ls
    archetypes  exampleSite  images  layouts  LICENSE.md  package.json  README.md  scss  static  theme.toml

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme (master)
    $ cd exampleSite/

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme/exampleSite (master)
    $ cp -r ./* ./../../../

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme/exampleSite (master)
    $ cd ..

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme (master)
    $ ls
    archetypes  exampleSite  images  layouts  LICENSE.md  package.json  README.md  scss  static  theme.toml

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes/charaka-hugo-theme (master)
    $ cd ..

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ ls
    charaka-hugo-theme

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test/themes (marktime)
    $ cd ..

よし、上書きしたぞ

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio/test (marktime)
    $ hugo server
    Building sites …
                      | EN
    +------------------+----+
      Pages            | 31
      Paginator pages  |  0
      Non-page files   |  0
      Static files     |  4
      Processed images |  0
      Aliases          | 12
      Sitemaps         |  1
      Cleaned          |  0

    Total in 99 ms
    Watching for changes in C:\project\portfolio\test\{content,data,layouts,static,themes}
    Watching for config changes in C:\project\portfolio\test\config.toml
    Serving pages from memory
    Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
    Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
    Press Ctrl+C to stop


おうけい、見れた  
カスタムとか何かは次に回そう、とりあえず使えることを目的に  
ていうか手探りでやるにしてもクイックスタートくらいは守ろう  

https://gohugo.io/categories/getting-started

ということで作りなおし

新規で作成して

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project
    $ hugo new site hugo
    Congratulations! Your new Hugo site is created in C:\project\hugo.

    Just a few more steps and you're ready to go:

    1. Download a theme into the same-named folder.
    Choose a theme from https://themes.gohugo.io/, or
    create your own with the "hugo new theme <THEMENAME>" command.
    2. Perhaps you want to add some content. You can add single files
    with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
    3. Start the built-in live server via "hugo server".

    Visit https://gohugo.io/ for quickstart guide and full documentation.

テーマを落として

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo/themes (master)
    $ git clone https://github.com/natarajmb/charaka-hugo-theme.git ./charaka
    Cloning into './charaka-hugo-theme'...
    remote: Enumerating objects: 211, done.
    remote: Total 211 (delta 0), reused 0 (delta 0), pack-reused 211 eceiving objects:  80% (169/211), 1.09 MiB | 724.00 KiB/s
    Receiving objects: 100% (211/211), 1.57 MiB | 832.00 KiB/s, done.
    Resolving deltas: 100% (95/95), done.

設定に参照するテーマを指定して

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo
    $ echo 'theme = "charaka"' >> config.toml

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo
    $ cat config.toml
    baseURL = "http://example.org/"
    languageCode = "en-us"
    title = "My New Hugo Site"
    theme = "charaka"

クイックスタート通りに新しい記事を作成して

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo
    $ hugo new posts/my-first-post.md
    C:\project\hugo\content\posts\my-first-post.md created

サーバースタート

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo
    $ hugo server -D
    Building sites …
                    | EN
    +------------------+----+
    Pages            | 10
    Paginator pages  |  0
    Non-page files   |  0
    Static files     |  3
    Processed images |  0
    Aliases          |  4
    Sitemaps         |  1
    Cleaned          |  0

    Total in 96 ms
    Watching for changes in C:\project\hugo\{content,data,layouts,static,themes}
    Watching for config changes in C:\project\hugo\config.toml
    Serving pages from memory
    Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
    Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
    Press Ctrl+C to stop

うむ、これだけだな、とりあえず次に進むか

7. HugoとGitHub

さて、HugoとGitHubについてだが…  
まずは公開用のリポジトリを作成せねばいけないらしい、  
作るか

ブランチを作成して

https://github.com/tsako2235/blog

設定で、GitHub Pagesのmasterブランチになるように変更

https://ユーザまたは組織名.github.io/リポジトリ名

https://tsako2235.github.io/blog
よし、何か表示された、で  
さっきつくったHugoのビルド済みファイルをPushしてやればいいのかな  

えーと、今現在のremoteは、以下2件のみだから

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (marktime)
    $ git remote -v
    portfolio       https://github.com/tsako2235/portfolio (fetch)
    portfolio       https://github.com/tsako2235/portfolio (push)

あー、なるほど、今masterブランチしかないのか
直接触りたくないので、えーと、これmarktimeと同じ名称でいいのかな

    git pull origin pullしたいリモートブランチ名:ローカルブランチ名

さすがに同名はやばいと思ったのかリポジトリ名_ブランチ名とすることに

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog
    $ git init
    Initialized empty Git repository in C:/project/blog/.git/

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (master)
    $ git pull https://github.com/tsako2235/blog.git marktime:blog_marktime
    remote: Enumerating objects: 3, done.
    remote: Counting objects: 100% (3/3), done.
    remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From https://github.com/tsako2235/blog
    * [new branch]      marktime   -> blog_marktime

えーと、リポジトリ別名はblogでいいな

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (master)
    $ git remote add blog https://github.com/tsako2235/blog

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (master)
    $ git remote -v
    blog    https://github.com/tsako2235/blog (fetch)
    blog    https://github.com/tsako2235/blog (push)

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (master)
    $ git branch -a
    blog_marktime
    * master

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (master)
    $ git fetch blog -v
    From https://github.com/tsako2235/blog
    * [new branch]      marktime   -> blog/marktime
    * [new branch]      master     -> blog/master

はぁ、master先生また出来てる…  
portfolioは、あとから消そう  
で、ローカルルールとして、pullするときのブランチ名はリポジトリ名_ブランチ名としよう  
絶対いつか混乱するしな
あ、あれ？gitて他リポジトリに切り替えたい時どうすりゃいいんだ？

ああ、ディレクトリ移動だけで大丈夫なんか
とりま、ローカルのmasterを削除

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $     git branch -d master
    Deleted branch master (was 686be34).

うむ

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git branch -a
    * blog_marktime
    remotes/blog/marktime
    remotes/blog/master

さて、さきほどビルドしたHugoのリソースをプッシュだぁ！  
と、ちょっとまて、svnと一緒でテーマ落とした時とか.gitファイルあるぞ  
そのままプッシュして本当に、大丈夫なのかにゃ？  

何か嫌な予感がするのでとりあえず削除しておこう

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugo
    $ find . -name .git | xargs rm -rf
    
おけ、あとpublicディレクトリだけでいいのか、公開するのは…  
んんー全体をプッシュしてpublicディレクトリ配下のみを  
公開したい…ルート変更できるかな？  

うそやろ？いったんmaster殺してmasterディレクトリにするの  
そんなばかな、ぐ、ぐぬぬsettingsでルートをdocにできるが…
Hugoから吐かれるフォルダは、publicなんだよなぁ…  
windowsでシンボリックリンクみたいのできるんだっけか？
ぐ、ぐぬうコマンドがわからん、VSCodeのターミナルbashだし
普通のUnix系シンボリックコマンドでもうまく読み替えてくれるだろたぶん

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/hugopwd
    $ ln -s public docs

できた、さすがやな！！  
よっしゃコピーしてプッシュや！

あ、リンクが怒られた…コピーしてからシンボリックリンクにしようね

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git add .

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git commit
    hint: Waiting for your editor to close the file...
    [blog_marktime c5a18ff] 2018-12-25 16:35:00 tsako2235 add hugo files
    32 files changed, 1811 insertions(+)
    create mode 100644 archetypes/default.md
    create mode 100644 config.toml
    create mode 100644 content/posts/my-first-post.md
    create mode 100644 docs/404.html
    create mode 100644 docs/categories/index.html
    create mode 100644 docs/categories/index.xml
    create mode 100644 docs/categories/page/1/index.html
    create mode 100644 docs/css/styles.css
    create mode 100644 docs/index.html
    create mode 100644 docs/index.xml
    create mode 100644 docs/js/highlight.js
    create mode 100644 docs/js/jssocials.js
    create mode 100644 docs/page/1/index.html
    create mode 100644 docs/sitemap.xml
    create mode 100644 docs/tags/index.html
    create mode 100644 docs/tags/index.xml
    create mode 100644 docs/tags/page/1/index.html
    create mode 100644 public/404.html
    create mode 100644 public/categories/index.html
    create mode 100644 public/categories/index.xml
    create mode 100644 public/categories/page/1/index.html
    create mode 100644 public/css/styles.css
    create mode 100644 public/index.html
    create mode 100644 public/index.xml
    create mode 100644 public/js/highlight.js
    create mode 100644 public/js/jssocials.js
    create mode 100644 public/page/1/index.html
    create mode 100644 public/sitemap.xml
    create mode 100644 public/tags/index.html
    create mode 100644 public/tags/index.xml
    create mode 100644 public/tags/page/1/index.html
    create mode 160000 themes

よっしゃ、プッシュや

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git push blog blog_marktime:marktime
    Enumerating objects: 34, done.
    Counting objects: 100% (34/34), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (23/23), done.
    Writing objects: 100% (33/33), 27.99 KiB | 1.40 MiB/s, done.
    Total 33 (delta 7), reused 0 (delta 0)
    remote: Resolving deltas: 100% (7/7), done.
    To https://github.com/tsako2235/blog
    686be34..c5a18ff  blog_marktime -> marktime

よすよす

masterへのpullリクエストを作成して、承認と
で、改めてmasterブランチのセッティングからGitHub Pagesの設定で
公開する対象をdocsに変更、と

さぁ、みてみるぜよ…は？  
何か、テーマ反映されていないんですが
あ、ああ、そうだよね

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ cat config.toml
    baseURL = "http://example.org/"
    languageCode = "en-us"
    title = "My New Hugo Site"
    theme = "charaka"

よしよし

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ cat config.toml
    baseURL = "https://tsako2235.github.io/blog/"
    languageCode = "en-us"
    title = "My New Hugo Site"
    theme = "charaka"

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git add .

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git commit .
    hint: Waiting for your editor to close the file...
    [blog_marktime ec6c056] 2018-12-25 16:47:26 tsako2235 changes base url
    1 file changed, 1 insertion(+), 1 deletion(-)

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git push blog blog_marktime:marktime
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 379 bytes | 379.00 KiB/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Resolving deltas: 100% (1/1), completed with 1 local object.
    To https://github.com/tsako2235/blog
    c5a18ff..ec6c056  blog_marktime -> marktime

今度はhugoコマンドを忘れるくそ

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ hugo
    Building sites …
                    | EN
    +------------------+----+
    Pages            |  7
    Paginator pages  |  0
    Non-page files   |  0
    Static files     |  3
    Processed images |  0
    Aliases          |  3
    Sitemaps         |  1
    Cleaned          |  0

    Total in 47 ms

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git add .

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git commit .
    hint: Waiting for your editor to close the file...
    [blog_marktime 56bcaff] 2018-12-25 16:50:01 tsako2235 issue hugo commands
    11 files changed, 20 insertions(+), 20 deletions(-)

    TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/blog (blog_marktime)
    $ git push blog blog_marktime:marktime
    Enumerating objects: 41, done.
    Counting objects: 100% (41/41), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (16/16), done.
    Writing objects: 100% (22/22), 1.61 KiB | 411.00 KiB/s, done.
    Total 22 (delta 12), reused 0 (delta 0)
    remote: Resolving deltas: 100% (12/12), completed with 6 local objects.
    To https://github.com/tsako2235/blog
    ec6c056..56bcaff  blog_marktime -> marktime

あれ、だめだな…
publicは大丈夫だ

    <link rel="stylesheet" href="https://tsako2235.github.io/blog//css/styles.css">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css"

は？docsは…シンボリックリンク死んでるやん、なんで？

    <link rel="stylesheet" href="http://example.org//css/styles.css">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css"

つかシンボリックリンクコマンド、これコピーしてるだけじゃねｋ？
ローカルもdocs配下更新されてない、というかリンクじゃなさそう。

しゃーない、あとでhugoコマンド発行時にコピペするようにするか  
とりあえずmasterまでの反映完了っと  
よし、表示され…あれ？最初につくった記事、どこいっただ
うーん、ない、contentsにはあるけど、なかったことになっているぽいー  
baseurl変更したからか？試しにnew postして試してみよう

ああ、draft: trueになってるからか

    draft: true
    ---

だめだぁ…んん？どうもテーマにより、参照するcontent配下のパスが違うような
今回のcharakaをみてみると、スクリプトの内容的に  
記事を配置するのは ./content/posts/xx.md  
となるようだ、そうか

よし、できた！


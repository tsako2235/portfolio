Windows環境で開発環境を整えて、
VSCode + Hugo + GitHubでポートフォリオを作成する話

1. VSCodのインストール
インストール後、VSCodeでtextを開く

2. gitインストール
ここを参考にインストール
https://qiita.com/shinsumicco/items/a1c799640131ae33c792

Ctrl+Shift+@でターミナルを開き、Gitのパスが通り、インストールされているか確認
 TAISHI-SAKO@DESKTOP-LV0F0HG MINGW64 /c/project/portfolio (master)
 $ git version
 git version 2.19.1.windows.1

4. いつものを見て実施
https://guides.github.com/activities/hello-world/
とりまmasterへPullまで確認
portfolioのリポジトリと、master以外のブランチを作成しておく

作成したリポジトリはこちら
https://github.com/tsako2235/portfolio

ブランチはワンクッション置くという意味で、
仮にmarktime（調子を取る）とした、英語は壊滅的な評価しかもっていないので真似しないように。

プロジェクトディレクトリを作成
C:\project\portfolio

5. VSCodeとGitHubの連携
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

ついでに、portfolioをリモートリポジトリに追加しておく
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
portfolio.master portfolio.marktime

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

6. VSCodeとHugo


7. HugoとGitHub


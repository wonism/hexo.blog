---
title: Git 되돌리기
date: 2016-08-14 17:13:04
tags: Github, git, git command, reset
---

<p>깃을 사용하다보면, 커밋 또는 푸시를 잘못 할 수도 있다. (추가하길 원치 않았던 파일들을 추가해서 커밋을 했거나, 다른 브랜치에서 커밋을 했거나.. 기타 등등)<br />이 때, 몇 가지 작업을 통해 커밋 / 푸시를 취소할 수 있다.</p>

<p>먼저, git reset 을 통해 리셋을 한다.</p>
```sh
# 커밋 하나 되돌리기
$ git reset HEAD^

# 커밋 3 개 되돌리기
$ git reset HEAD~3
```

<p>그 다음, 다시 커밋을 한 뒤, 푸시를 한다.</p>
```sh
# 커밋 (이때 커밋한 내용은 저장소에 저장되지 않는다. 고로, 커밋 메시지를 아무렇게나 해도 무방함..)
$ git commit -am '커밋 취소'

# 푸시 (이 때, 다음과 같은 메시지가 뜬다.)
$ git push
To git@github.com:USERNAME/REPOSITORY.git
 ! [rejected]        master -> master (non-fast-forward)
 error: failed to push some refs to 'git@github.com:USERNAME/REPOSITORY.git'
 hint: Updates were rejected because the tip of your current branch is behind
 hint: its remote counterpart. Integrate the remote changes (e.g.
 hint: 'git pull ...') before pushing again.
 hint: See the 'Note about fast-forwards' in 'git push --help' for details.

# 강제 푸시
$ git push -f
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:USERNAME/REPOSITORY.git
 + COMMIT_HASH...COMMIT_HASH master -> master (forced update)
```

<p>이러면, 푸시한 내용이 사라지게 된다.</p>


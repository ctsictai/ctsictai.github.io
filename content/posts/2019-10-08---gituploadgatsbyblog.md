---
title: Git을 이용한 gatsby blog 작성
date: "2019-10-08T22:30:32.169Z"
template: "post"
draft: false
slug: "/posts/the-origins-of-social-stationery-lettering"
category: "git"
description: ""
socialImage: "/media/image-3.jpg"
---

# git의 구조

![Git-Structure](https://jistol.github.io/assets/img/git/git-basic/git-basic-3.png)

## Git add .

- 변경된 파일을 모두 tracking해서 working space에서 stage(index)로 upload

## git status

- 현재의 git과의 상태를 체크하는 명령어 - step by step 마다 쳐서 확인한다. (add/commit 상태 확인)

- no add status
  - modified: movie/views.py
- add status
  - new file: requirements.txt

## git commit -m "message"

- 간결하고 알아볼 수 있는 message를 남긴다. commit을 하면 index에서 local repo로 간다.

## git remote add origin github repo url

- local repo와 remote repo를 연결하기 위해 github에서 만든 new repository 주소를 붙여준다.

## git remote -v

- local repo와 연결된 remote repo 상태를 보여주는 명령어

## git branch "branch name"

- create branch

## git branch

- 해당 remote repo에 생성된 branch 현황을 보여준다.

## git push origin master 혹은 branch name

- remote repo(github에 있는 repo)에 최종적으로 file upload
- 이 때 master는 master branch에 push 하는 것이고 branch name은 만든 branchdp push 한다.

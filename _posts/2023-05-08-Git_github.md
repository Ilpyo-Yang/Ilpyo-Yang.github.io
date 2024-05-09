---
title: Git & GitHub
author: Rosie Yang
date: 2023-05-08
category: tool
layout: post
---

+ [Git Command 정리]({{site.baseurl}}/tool/2023/05/08/Git_github.html#jekyll-github-블로그-로컬에서-관리하기)
+ [잘못된 Github 실수 만회하기]({{site.baseurl}}/tool/2023/05/08/Git_github.html#jekyll-github-블로그-로컬에서-관리하기)
+ [Jekyll GitHub 블로그 로컬에서 관리하기]({{site.baseurl}}/tool/2023/05/08/Git_github.html#jekyll-github-블로그-로컬에서-관리하기)

## Git Command 정리  
##### 기본 명령어
```git clone```, ```git commit``` 이런거 말고 자주 쓸지도 모르는 명령어들을 정리해봤습니다.
+ ```git reset [파일명]``` 파일 언스테이징
+ ```git stash``` 현재 변경사항 stash에 임시 저장하고 원래 커밋 상태로 되돌림
  + 여기서 stash에 저장된 내역은 ```git stash apply```로 최신 저장된 것을 적용할 수 있습니다. 특정 인덱스 적용시에는 ```git stash apply stash@{N}```을 사용하면 됩니다.
  + ```git stash pop```으로 최근 것 적용후 그 스태시를 목록에서 제거할 수도 있습니다.
  + 그 외에도 ```drop```, ```list``` 기능들이 있습니다.
+ ```git remote show [원격 저장소]``` 특정 원격 저장소에 대한 자세한 정보를 확인
+ ```git bisect``` 이진 검색으로 버그가 발생한 특정 커밋 찾기
+ ```git reset --hard [커밋]``` 특정 커밋으로 (강제) 돌아감
+ ```git cherry-pick [커밋1] [커밋2]``` 다른 브랜치 커밋을 현재 브랜치로 가지고 오기
  + 브랜치명을 이용해서 가지고 올 수도 있습니다.
+ ```git clean -n``` 추적되지 않은 파일 확인
+ ```git switch -c``` 브랜치 생성과 이동

#### Git log 깔끔하게 관리하기

##### [ 헷갈리는 Reset, Revert, Rebase, Undo 정리하기 ]  
git log 관리할 때마다 쓰는데 헷갈린 적 있었던 관련 명령어를 이번 기회에 ~~명절이니까 머리가 여유롭기 때문에~~ 정리했습니다.  
한 줄로 정리하면, <span style="background-color:#fff5b1">이전꺼 지울려면 Reset, 이것만 정리할라면 Revert, 브랜치 기준으로 정리하려면 Rebase, 히스토리 변경없이 상태만 바로 이전으로 변경하고 싶으면 Undo를 사용하면 됩니다.</span>  

**Reset**  
이전 커밋들을 취소하거나 삭제하는 등 특정 커밋으로 돌아가기 위한 명령어로 옵션에 따라 작업내용을 유지할지 여부를 정할 수 있습니다.  
+ 옵션은 soft, mixed(defalut), hard가 있는데 모든 변경사항을 특정 시점 기준으로 확인하고 스테이징을 유지하기 위해 주로 저는 soft를 이용합니다. 

<p style="text-align: center">
    <img src="/assets/gitbook/post_images/git/git-reset.png" style="width:80%">
</p>

**Revert**  
특정 커밋 사항을 취소하고 새로운 커밋으로 기록하는 것으로 커밋 히스토리에 이전 커밋을 취소한 내용이 추가됩니다. 그 사이 커밋들은 유지되는 것을 볼 수 있습니다.
협업 중인 경우, 공유된 커밋으로 인한 충돌을 막기 위해서는 Revert를 사용하는 것이 좋습니다.

<p style="text-align: center">
    <img src="/assets/gitbook/post_images/git/git-revert.png" style="width:80%">
</p>

**Rebase**  
다른 브랜치를 기준으로 변경하거나 정리할 수 있도록 하는 명령어입니다. Rebase 역시 중간 커밋을 히스토리 없이 삭제가 ```pick```, ```drop```으로 가능합니다.
+ Interactive Rebase 종류는 ```git rebase -i```로 확인이 가능합니다.
+ ```Squash```: 여러 개의 연속적인 커밋을 하나의 커밋으로 합치는 경우에 사용됩니다. 반대로 되돌리려는 경우에는 ```git reset HEAD^ --hard```로 취소 후 다시 커밋처리 해주어야 합니다.
+ ```Fixup```: ```Squash```와 유사하지만 새로운 커밋 메시지를 지정할 수 없고 이전 커밋메시지를 사용합니다.
+ ```Reorder```: 커밋순서 변경
+ ```Exec```: 커밋 히스토리를 변경하는 것이 아니라 특정 커밋 사이에 특정 작업을 할 수 이게 합니다. 예를 들어 다음 명령과 같이 커밋 사이에 특정 스크립트를 실행하는 것을 일시적 목적으로 사용할 수 있게 합니다.
  ```
  pick c0ffee1 First commit
  exec ./myscript.sh
  pick 1a2b3c4 Second commit
  ```

<p style="text-align: center">
    <img src="/assets/gitbook/post_images/git/git-rebase.png" style="width:80%">
</p>

**Undo**  
작업 디렉토리와 스테이징 영역의 변경 사항을 이전 커밋 상태로 되돌리는 것을 말합니다. 커밋 히스토리에는 영향을 주지 않습니다.  

```
git reset [커밋 해시] --hard
git revert [커밋 해시]
git rebase [다른 브랜치]
git rebase -i HEAD~[커밋 개수]
git restore --source=[커밋 해시] --worktree --staged --source=HEAD [파일명]
```

##### [ 원본 저장소 최신 상태 로컬에 반영하기 ]  
오픈소스의 최신 커밋상태를 가지고 와서 기존 로컬 브런치 상태를 업데이트 하기 위해 사용합니다. 여기서 upstream은 내가 fork한 원격 브런치를 의미합니다. 반대로 fork를 해서 가지고 온 내 repo는 origin이 됩니다.
아래 코드에서 보면, git clone으로 가지고 온 repo이기 때문에 여기서 upstream을 추가하고 fetch로 원격 저장소 최신 상태를 가지고 옵니다. 원격 저장소가 2개 이상인 경우의 upstream을 별도로 명시하면 되지만, 모든 원격 저장소 최신 사항을 반영하려는 경우에는 ```git fetch --all```과 같이 적용할 수 있습니다.
그 다음 ```upstream/main``` 위로 현재 작업 브런치 커밋을 올려주는데 이 때 충돌이 발생하는 경우에는 해결 후 ```git rebase --continue```로 진행할 수 있습니다.
그리고 로컬 브랜치 ```main```에 적용합니다.
```
git remote add upstream [원본저장소 url]
git fetch upstream
git checkout main
git rebase upstream/main
git push origin main --force
```

##### [ remote 추가하기 ]  
원격 브런치가 두 개 이상 필요한 경우 remote를 하나 더 추가해서 커밋할 경우 해당하는 remote에 적용 또는 풀 받아올 수 있도록 할 수 있습니다. 앞서 설명한 것처럼 origin과 upstream은 차이가 있으므로 구분해서 사용할 필요가 있습니다.
```
git remote add upstream [원본저장소 url]
git remote add origin [원본저장소 url]
```
등록된 원격저장소는 ```git remote -v```로 확인이 가능합니다.

<hr/>

+ [When to Use Git Reset, Git Revert & Git Checkout](https://dev.to/neshaz/when-to-use-git-reset-git-revert--git-checkout-18je)
+ [Git Rebase](https://www.geeksforgeeks.org/rebasing-of-branches-in-git/)

<br><br>

## 잘못된 Github 실수 만회하기
> 아찔한 잘못된 깃헙 실수를 발견할 때가 있습니다. 이를 다시 수정하기 위한 각각 상황별 방법을 기록해보고자 합니다.

<p style="text-align: center; margin: 10px 0">
    <img src="/assets/gitbook/post_images/git/git_meme.jpg" style="width:40%">
</p>

### 푸쉬한 내 커밋 메시지 수정하기
IntelliJ에서 취소를 했는데도 잘못된 깃헙 메시지가 푸쉬서 올라가는 경우가 있습니다. 이 경우 아래 방법들로 해결할 수 있습니다. 하지만 협업하는 팀 프로젝트에서는 각 커밋들이 강제로 push된다고 했을 때
다른 브런치와의 충돌이 있을 수 있기 때문에 한 브런치에서 작업한 작업물에 대한 commit 내역을 수정하는 경우에 권장됩니다. 그래서 ```feature```를 따로 작게 분류하거나 ```fork```하는 것이 좋습니다. 
~~엮이지 말자..~~

**IntelliJ에서 수정하기**
+ 간단하게 Git Log (```Alt+F9```)를 확인하고 해당 commit 내역을 ```Shift+F6``` 로 메시지 수정이 가능합니다.
+ 이 경우 그 commit을 기준으로 이후의 커밋들이 다시 쓰여지기 때문에 모두 ```Force Push``` 처리로 적용이 가능합니다.

**Git Bash를 이용한 수정방법**  
프로젝트 git 위치에서 수정할 커밋이 최근 커밋으로부터 몇 번째인지 확인하고 ```rebase```합니다. 
여기서 ```rebase```란, 기존 커밋 히스토리를 새로운 기준으로 재배치하는 기능입니다. 작업 중인 feature 브랜치에 다른 브랜치의 변경사항들을 반영시키기 위해서도 사용합니다.
이 이슈에서는 커밋 히스토리를 변경하기 위해 적용해보겠습니다.
```
git rebase HEAD~10 -i
```
이후에 편집모드를 사용해서 ```pick```을 ```reword``` 상태로 수정 후 커밋메시지를 수정합니다. 그리고 저장 후 강제 푸쉬를 하면 적용된 것을 확인할 수 있습니다.
```
git push --force
```

##### 실제 적용화면

![rebase_commit.png](/assets/gitbook/post_images/git/rebase_commit.png)


<br><br>

## Jekyll GitHub 블로그 로컬에서 관리하기
```깃헙계정 아이디```.github.io 로 된 repo를 생성합니다.   
jekyll에서 맘에 드는 테마를 가지고 오면 되는데 이 블로그에 사용된 테마는 [gitbook 테마](http://jekyllthemes.org/themes/gitbook/)입니다. 깃북으로 만들어도 되지만 더 마음대로 꾸밀 수 있고, 깃헙 파일구조도 깔끔하게 가지고 갈 수 있어서 jekyll 테마를 사용하게 되었습니다.  
설치될 OS에 적합한 <span style="background-color:#fff5b1">Ruby</span>를 설치하고 <span style="background-color:#fff5b1">Jekyll+Devkit</span> 설치를 진행합니다.    
Ruby가 설치되면 아래 명령어로 Ruby cmd에서 Jekyll과 Bundler를 설치합니다.
```
gem install bundler jekyll 
```
이후 블로그 디렉토리로 이동해서 <span style="background-color:#fff5b1">Gemfile을 수정</span>해 필요한 플러그인이 설치될 수 있도록 합니다. 실제로 작성되어 있던 파일들 중에서 주석을 제거하고 버전을 맞춰주었는데 현재 이 블로그에 적용된 설정은 다음과 같습니다.
```
source "https://rubygems.org"
gem "minima", "~> 2.5"
gem 'jekyll-include-cache'
gem "webrick", "~> 1.7"
gem "github-pages", group: :jekyll_plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
```
로컬 확인을 위해 <span style="background-color:#fff5b1">_config.yml 파일</span>의 url 부분을 ```https://localhost:4000```을 수정하고 Ruby cmd에서 실행하면, 로컬에서 확인이 가능합니다.
```
bundle exec jekyll serve
```

##### 🚴🏽 FilePemissionError 발생시
Mac에서 다시 jekyll 세팅하는 과정에서 권한 오류가 발생했는데 root 권한으로 하지 않는 경우에 시스템 ruby에 대한 권한 문제로 발생한 에러압니다. 
[Mac에서 Gem::FilePermissionError 에러 발생시 해결 방법](https://jojoldu.tistory.com/288)의 향로님 글을 보고 해결했는데, 그 중에서 rbenv path를 설정하는 부분만 여기서 살펴보려고 합니다.
```
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```
이 스크립트를 추가하게 되면, Ruby의 환경관리도구인 rbenv를 초기화하고 환경변수를 설정하게 됩니다. 현재 홈 디렉토리 내에 `.rbenv` 디렉토리가 있는지 확인하고 있다면 환경변수에 추가해 rbenv 실행 파일들이 환경변수에 포함되어 명령어를 실행할 때 찾게 합니다.


<div style="padding:3px; margin:200px 0;"></div>   







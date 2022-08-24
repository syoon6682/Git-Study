# Git Merge

## 0. 시작하기에 앞서..

생각보다 conflict를 일으키는 것이 어렵다...! 그러므로 conflict가 일어나는 상황부터 이해하고 넘어가자!

![image-20220628223657956](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628223657956.png)

아래의 경우 master에서는 전혀 개발이 진행하지 않고 branch에서만 진행을 하다가 바로 master에 merge를 시키는 그림이다. 이때 저 branch에서 추가를 하던, 수정을 하던 그 어떤 변화도 충돌이 일어나지 않고 바로 merge가 가능하다..나는 conflict 상황을 봐야하는데 conflict 상황이 안 일어나...



![image-20220628224105908](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628224105908.png)

하지만 다음과 같은 경우, master에서도 추가적으로 commit, push가 이루어지고 branch에서 commit, push가 merge 될 때에는 충돌이 일어날 수 있다. 만약 처음 commit으로부터 동일 부분이 master에도 변경이 일어나고 branch에서도 변경이 일어나면 프로그램은 판단할 수 없기 때문에 개발자가 직접 판단해서 고쳐주는 것이다. (이제서야 깨닫는 구체적인 conflict가 일어나는 이유..) 이때 발생하는 merge request를 같이 해결해보도록 하자.



## 1. 시작 환경

: master branch에서 branch1 branch를 생성 후 변경을 준 후 commit, push 진행

![image-20220628221333251](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628221333251.png)

![image-20220628221402868](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628221402868.png)



## 2.  MR 작성하기

### 2-1 MR 요청

![image-20220628224510080](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628224510080.png)

이때 Target branch는 Master branch, Source branch는 병합하고자 하는 branch를 선택해준다. 

 



### 2-2 option 설정

![image-20220628224628209](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628224628209.png)

개인별로 알맞은 옵션을 선택해서 진행



## 3. 충돌 발생

![image-20220628224856420](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628224856420.png)

이때 conflict가 발생했으므로 해결을 해주어야 하는데 2가지 방법이 제시가 된다. 

1. Resolve conflicts: gitlab 자체에서 해결하는 방법
2. Resolve locally: code에서 해결하는 방법

그러면 본격적으로 Gitlab 내부에서 MR을 처리하는 방법과 vscode와 bash를 활용하여 MR을 처리하는 것을 설명하도록 하겠다. 



## 4. Resolve conflicts



### 4-1 conflict 처리 상황

![image-20220628225931371](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628225931371.png)

origin // their changes: master로부터의 변경

HEAD // our changes:  branch로부터의 변경

이렇게 각각의 충돌사항을 이해하고 우측 버튼을 통해 원하는 라인을 선택을 해준다. 이렇게 모든 conflicts를 상황을 선택해주면 아래 "Commit to source branch"가 활성화가 되는데 활성화된 버튼을 누른다. 



### 4-2 Merge 활성화 및 Merge 진행

![image-20220628230245359](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628230245359.png)

 Merge를 할 수 있게 활성화가 된다! 그래서 기분 좋게 merge를 진행해주도록 하자.



### 4-3. 결과 화면

![image-20220628230354224](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628230354224.png)

"Commit to source branch"에서 미리 작성이 되어있던 commit message로 commit, push가 진행되어 있는 것을 볼 수 있다..! 그 후 git pull 받아서 이어서 개발을 진행하면 merge를 완료한다. 



## 5. Resolve locally



### 5-1. 사전 상황 설명 및 flow

branch를 파고, commit, push 진행, 그 후 master에서도 commit push를 진행한 후 MR을 보내서 conflicts 발생

그 후 Resolve locally를 누르면 다음과 같이 화면이 나온다. 

![image-20220628231313071](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628231313071.png)

매우 친절한 것을 알 수 있다.

다음 step에 따라 차근차근 진행해보았다. 



### 5-2. 각 Step별 세부 사항

**Step 1. git fetch 진행하고 branch로 checkout으로 이동하기** 

![image-20220628232216131](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628232216131.png)

근데 난 이런 error가 뜨더라..이때 두려워하지 말고 `git branch -d branch2`를 활용하여 local branch를 지워주면 해결이 된다.



**Step 2. 네, 그냥 해당 branch로 가서 실제로 눈으로 확인하라는 소립니다. 별 거 없어요.**



**Step 3. 다시 master branch로 돌아와서 병합하세요.**

`git merge --no-ff 'branch2'`에 대해서 조금만 분석을 해보면 `ff`옵션은 fast-forward 옵션이고 `--no-ff`는 병합 커밋을 만드는 (즉, merge가 되어서 만들어진 통합된 commit)을 만드는 옵션이라고 한다. 

아무튼 해당 코드까지 입력하면 

![image-20220628233049510](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628233049510.png)

자주 보았던 장면이 보인다. 이때 알맞은 옵션을 선택해주고 보면

![image-20220628233324960](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628233324960.png)

여전히 느낌표가 떠있는 상태로 유지가 되어 있다. (그리고 경험상 항상 여기서 그 다음은 뭘 해야하지 하면서 벌벌 떨었던 거 같다..)



**Step 4. Master로 push하면 끝**

하지만 현재 해결해야할 conflict는 다 해결했으므로 그냥 commit, push를 진행해주면 

![image-20220628233715126](Git-Merge_%EC%8A%B9%EC%9C%A4.assets/image-20220628233715126.png)

이렇게 MR이 없어지면서 Merged가 된 것을 알 수 있다.



## 6. 마무리

사실 실제로 해보기 전에는 과거 merge를 bash와 vscode를 활용하는 것만 많이 봐서 당연히 locally한 방법이 더 쉬울 줄 알았으나 정말 숙달과 이해만 된다면 gitlab 내부에서 진행하는 게 훨씬 더 쉬울 거 같다고 생각하게 됨. 오히려 오류가 나는 상황이나 절차 등이 gitlab 내부에서 진행하는 것이 덜 위험하고 간소화되어 있다. 그러므로 여러분들이 github을 쓰게 될 수도 있고, gitlab을 쓰게 될 수도 있지만 그런 저장소 내부 MR 처리 기능을 한번 활용해보자

그리고 locally한 방법에서 항상 저 conflicts들을 다 해결해도 bash는 Merging 상태에 빨간 느낌표가 사라지지 않아 항상 불안하고 그 뒤를 깔끔하게 넘어간 적이 없었는데 그냥 commit, push 찍는 거였다...허무해라...생각보다 fast-forward merge 안에서 발생하는 confilct는 그렇게 어렵지 않게 해결할 수 있는 문제였다는 것을 발표 준비하면서 느끼게 되었다. 





## 참고 블로그

https://chaeyoung2.tistory.com/61

https://hoi5088.medium.com/gitlab-merge-request-8e7965f7f4be

https://koreabigname.tistory.com/65

https://docs.gitlab.com/ee/user/project/repository/web_editor.html#create-a-new-branch

https://jw910911.tistory.com/16
---
layout: post
title: Github Personal Access Token 적용하기
tags: 
- github
- git
- personal access token
---

Local에서 git push를 하려고 보니, 아래와 같이 Authentication에 실패하였다는 에러가 발생하였다.  

![git_authentication_fail](/assets/images/git_authentication_fail.png)

자세히 읽어보니, 비밀번호 대신 Personal Access Token을 사용하라는 내용이었다.

> remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.

Github에서 자꾸 비밀번호가 deprecated될 것이니 무슨 조치를 취하라고 아래와 같은 메일을 계속 보냈었던거 같은데, 이게 그건가 싶었다.

![github_deprecation_notice](/assets/images/github_deprecation_notice.png)

귀찮아서 피하고 있었지만... 이제 피할 수 없었다.

아무튼, 다음 세 과정을 통해 Password 대신 Personal Access Token을 적용할 수 있다.

### 1. Github에서 Personal Access Token 발급하기.

   - Github에 로그인 한 후, 우측 메뉴의 Settings로 들어간다.

   ![Github_Menu](/assets/images/Github_Menu.png){: width="35%"}

   - 그리고 좌측 세부 메뉴에서 Developer settings로 들어간다.

   ![Github_Settings](/assets/images/Github_Settings.png)

   - 아래와 같이, Note란에 토큰에 대한 간단한 설명을 적고, Expiration란에서 토큰 지속 기간을 선택한다. (지속기간을 무한으로 할 수 도 있고 원하는 만료날짜를 선택할 수 있지만, 혹시 몰라서 3개월로 일단 해보았다.) 마지막으로 토큰의 scope를 선택하면된다. 나는 repo 접근권한만 선택하였다.
   
![Github_New_Personal_Access_Token](/assets/images/Github_New_Personal_Access_Token.png)

   - 입력 후 Generate Token 버튼을 누르면 다음과 같이 토큰이 발급된다. 이 토큰을 클립보드에 복사한다.

![Github_Personal_Aceess_Token_Complete](/assets/images/Github_Personal_Aceess_Token_Complete.png)

### 2. Local의 Git config에 등록되어있는 기존 Password를 삭제하기.

   -  terminal에 `git config --global --unset user.password` 를 입력하면 기존에 등록되었던 Password가 초기화된다.
   
   ![Git_Reset_Password](/assets/images/Git_Reset_Password.png)

### 3. Password에 Personal Access Token을 입력하기.

- Password를 삭제 한 후에, git push를 하면 다음과 같이 Username과 Password를 입력하라고 나온다. (패스워드만 초기화시키려고 했는데 ID도 다시 입력하라니...) 
- Username에 github ID를 입력하고 Password에 복사해두었던 Personal Access Token을 붙여넣으면 된다.

![Git_Set_Password](/assets/images/Git_Set_Password.png)

- 그럼 다음과 같이 토큰이 저장되고, push가 잘 되는 것을 확인할 수 있다.

![Git_Push_Complete](/assets/images/Git_Push_Complete.png)

## 참고

- git password 초기화: [https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git](https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git)

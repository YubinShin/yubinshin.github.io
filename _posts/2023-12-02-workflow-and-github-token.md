---
title: Github Actions 와 Github Token 으로 리드미에 동적인 차트 만들기
date: 2023-12-02
categories: [troubleshooting, rpa]
tags: [github, github-actions, 업무자동화]
---

## 🤔 Problem

최근 프로그래머스와 백준에서 알고리즘 풀이를 시작했다.

java와 sql 을 주로 풀고 있는데, 채용담당자 분들이 내 저장소를 봤을 때 어느 언어를 많이 연습했는지 한 눈에 잘 보이면 좋겠다는 생각이 들었다.

그래서 파일의 확장자를 정규표현식으로 검색하고, 저장소 대문인 `README.md` 에 퍼센티지로 나타내기로 했다.

[https://github.com/YubinShin/algorithm](https://github.com/YubinShin/algorithm)

## 🌱 Solution

### 결과물

   <div style="width: 80%;">
      <img src="https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-02-workflow-and-github-token/github-token-01.png" alt="README.md 결과물">
   </div>

### 주요 용어

`GITHUB_TOKEN`

깃허브 워크플로우 중에 인증 토큰이 필요하다면PAT(Personal Access Token) 대신에 사용할 수 있습니다.

 > GitHub Actions 워크플로우는 각 작업 시작 시 고유한 GITHUB_TOKEN 비밀 토큰을 **자동으로** 생성합니다. 
 >
 > 이 토큰은 워크플로우 작업에서 인증을 위해 사용할 수 있습니다. 
 >
 > GITHUB_TOKEN은 GitHub 앱 설치 액세스 토큰으로, 해당 리포지토리에 설치된 GitHub 앱을 대신해 인증하는 데 사용됩니다. 
 >
 > 이 토큰의 권한은 워크플로우가 포함된 리포지토리에 제한됩니다​​.
 > 
 > 출처 : [https://docs.github.com/en/actions/security-guides/automatic-token-authentication](https://docs.github.com/en/actions/security-guides/automatic-token-authentication)  

### 과정

나는 [백준허브](https://github.com/BaekjoonHub/BaekjoonHub)라는 크롬 익스텐션을 사용해서 자동으로 맞춘 문제가 push 되게끔 해두었다.  

1. 저장소의 Setting > Actions > General > Workflow permissions 의 설정이 필요하다.
   - Workflow 가 저장소의 내용을 변경하고 커밋하려면 별도의 권한을 부여해줘야한다.
   - Read and Write Permission 을 체크해주면 권한 부여 완료.
  
    <div style="width: 80%;">
      <img src="https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-02-workflow-and-github-token/github-token-03.png" alt="권한부여">
    </div>


2. 깃허브 저장소의 액션탭에 들어가서 새로운 워크플로우를 생성한다.

    <div style="width: 80%;">
      <img src="https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-02-workflow-and-github-token/github-token-02.png" alt="깃헙액션생성">
    </div>


3. workflow 에 단계를 추가해준다.
    - 저장소에 체크아웃
    - 파일 갯수를 세고 차트를 생성하기
    - 리드미에 업데이트할 내용 만들기
    - 깃헙 액션 권한으로 새로 만들어진 `README.md` 를 푸시하기
  
    <details markdown="block"><summary> 코드 전문 </summary>

    [https://github.com/YubinShin/algorithm/blob/main/.github/workflows/file_count.yml](https://github.com/YubinShin/algorithm/blob/main/.github/workflows/file_count.yml)

    ```yaml
    name: File Count and Update README

    on:
    push:
    workflow_dispatch:

    jobs:
    count_files_and_update_readme:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout Repository
            uses: actions/checkout@v2

        - name: Count Files and Generate Markdown Bar Chart
            run: |
            # 파일 수 계산
            java_count=$(find . -type f -name "*.java" | wc -l)
            sql_count=$(find . -type f -name "*.sql" | wc -l)
            undefined_count=$(find . -type f -not -name "*.*" | wc -l)
            total_files=$(find . -type f | wc -l)
            java_ratio=$(echo "scale=2; $java_count/$total_files*100" | bc)
            sql_ratio=$(echo "scale=2; $sql_count/$total_files*100" | bc)
            undefined_ratio=$(echo "scale=2; $undefined_count/$total_files*100" | bc)

            # 마크다운 바 차트 생성
            bar_chart="Java files ($java_ratio%): "
            for i in $(seq 1 $java_ratio); do bar_chart+="█"; done
            bar_chart+=" $java_ratio%<br/>SQL files ($sql_ratio%): "
            for i in $(seq 1 $sql_ratio); do bar_chart+="█"; done
            bar_chart+=" $sql_ratio%<br/>Undefined files ($undefined_ratio%): "
            for i in $(seq 1 $undefined_ratio); do bar_chart+="█"; done
            bar_chart+=" $undefined_ratio%"

            # README 업데이트
            sed -i "/<!-- file_counts_start -->/,/<!-- file_counts_end -->/c\<!-- file_counts_start -->\n$bar_chart\n<!-- file_counts_end -->" README.md

        - name: Commit and Push README Updates
            run: |
            git config --local user.email "action@github.com"
            git config --local user.name "GitHub Action"
            git add README.md
            git commit -m "Update file counts and bar chart in README"
            git push
            env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    ```
    </details>
    <br/>

4. `README.md`에 마커를 추가해준다.
    - `<!-- file_counts_start -->`와 `<!-- file_counts_end --> `
    - 위와 같이 마커를 추가해주면 워크플로우가 마커가 있는 부분만을 새로운 내용으로 교체할 수 있다.
  
    <details markdown="block"><summary> 코드 전문 </summary>
    [https://github.com/YubinShin/algorithm/blob/main/README.md](https://github.com/YubinShin/algorithm/blob/main/README.md)

    ```md
    # algorithm

    This is an auto-push repository for Baekjoon Online Judge created with [BaekjoonHub](https://github.com/BaekjoonHub/BaekjoonHub).

    <!-- file_counts_start -->
    <!-- file_counts_end -->
    ```
    </details>
    <br/>

5. 문제를 새로 풀 때마다 그래프가 새로운 비율로 그래프가 변경된다.

    <div style="width: 80%;">
      <img src="https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-02-workflow-and-github-token/github-token-01.png" alt="결과물">
    </div>

## 📎 Related articles

| 이슈명                         | 링크                                                                                                                                                                   |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Automatic token authentication | [https://docs.github.com/en/actions/security-guides/automatic-token-authentication](https://docs.github.com/en/actions/security-guides/automatic-token-authentication) |

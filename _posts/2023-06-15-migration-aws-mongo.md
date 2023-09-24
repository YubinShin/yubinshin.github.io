---
title: AWS S3, 몽고 디비 마이그레이션 대작전
date: 2023-06-15
categories: [troubleshooting]
layout: post
---

# [트러블슈팅]AWS S3, 몽고 디비 마이그레이션 대작전

날짜: 2023년 6월 15일
태그: 블로그, 작성중, 트러블슈팅

# 맛이슈 마이그레이션 대작전

## 🤔 Problem

배포 이틀 전, AWS S3 이미지 서버를 사용하던 중 프리티어가 터졌다!

S3 get 요청이 무료 기준인 20000번을 넘어가서 발생한 일이었다 🥹

## 🌱 Solution

누군가 동일한 문제를 만났을때 빠르게 해결할 수 있게 스텝바이스텝으로 작성한다.

1.  **[AWS S3 Bucket 마이그레이션 하기](https://interconnection.tistory.com/119)**
    새로운 **AWS 프리티어 계정**과 **AWS CLI**가 필요합니다.
    [최신 버전의 AWS CLI 설치 또는 업데이트 - AWS Command Line Interface](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)
2.  DB 내 이미지 url 변경하기
    마이그레이션 된 버킷에서 생성된 이미지 주소로 몽고디비에 기록된 링크를 교체합니다.
    ```bash
    from fastapi import FastAPI, HTTPException
    from pymongo import MongoClient

        app = FastAPI()

        # MongoDB 연결
        client = MongoClient(<<몽고디비url>>)
        db = client[<<DB명>>]
        collection = db[<<DB컬렉션>>]

        @app.put("/update-all")
        def update_all_documents(new_value: str):
            # 변경할 내용
            update_data = {"$set": {"특정_칼럼": new_value}}

            # 업데이트 수행
            result = collection.update_many({}, update_data)

            # 결과 확인
            if result.matched_count > 0:
                return {"message": "모든 문서 업데이트 성공!"}
            else:
                raise HTTPException(status_code=404, detail="조건과 일치하는 문서를 찾을 수 없습니다.")
        ```

## 📎 Related articles

| 이슈명                                  | 링크                                    |
| --------------------------------------- | --------------------------------------- |
| https://interconnection.tistory.com/119 | https://interconnection.tistory.com/119 |

---
date: 2024 년 11 월 17 일 23 시 11 분
tags:
  - Dev
  - s3
  - SimpleStorageService
author: Joung Dong Hee
share: true
---

# S3(Simple Storage Service)

Simple Storage Service 클라우드 기반 객체 스토리지 서비스로 이며 다양한 이미지 나 파일 등 을 저장하고 관리하는 데 사용한다.

현재 내가 사용하는 이미지 서버 또한 [[https://min.io/ | Minio]] 라는 무료 오픈 소스 S3 서버를 나스 Docker 에 설치하여 이미지 파일을 업로드 및 관리하고 있다.

## 버킷 (Bucket) 과 객체(Object)

S3 에서는 이미지 파일 및 관리하는 데이터 저장소 `Bucket` 이라는 용어로 정의 한다. 
버킷에는 사용자가 원하는 다양한 이미지 파일이나 미디어 파일 등 을 모아두는 역할 을 한다. 이때 버킷 안에 있는 데이터 하나 하나 를 `Object` 객체로 정의 한다.


# S3 를 사용하는 이유

1. S3 는 정말 쉽고 간편하게 데이터를 관리할수 있다.
2. S3 에서 제공하는 API 통신을 통해 파일 관리 및 삭제가 용이하며 또한 확장성 도 좋다.
3. S3 에서는 **IAM(Identity and Access Management)** 정책, 버킷 정책, ACL(Access Control List) 등을 사용해 세밀한 접근 권한을 설정할 수 있습니다. 이를 통해 공용 액세스, 읽기/쓰기 권한 등을 버킷별로 관리할 수 있습니다.
4. S3는 저장 용량, 데이터 요청 수, 데이터 전송량 등에 따라 요금이 부과됩니다. 효율적인 클래스 선택과 데이터 압축 등을 통해 비용을 절감할 수 있습니다.



---

### **S3를 제공하는 서비스 제공자**

아래는 S3 또는 유사 서비스를 제공하는 주요 클라우드 제공자입니다.

- **AWS S3 (Amazon Web Services)**  
    [AWS S3 공식 사이트](https://aws.amazon.com/s3)
- **Google Cloud Storage**  
    Google Cloud Storage 공식 사이트
- **Microsoft Azure Blob Storage**  
    [Azure Blob Storage 공식 사이트](https://azure.microsoft.com/en-us/products/storage/blobs)
- **MinIO (오픈 소스)**  
    [MinIO 공식 사이트](https://min.io/)
- **Wasabi**  
    [Wasabi 공식 사이트](https://wasabi.com/)  
    저비용 S3 호환 스토리지.
- **Backblaze B2**  
    Backblaze B2 공식 사이트  
    저렴한 가격의 S3 대체 스토리지.
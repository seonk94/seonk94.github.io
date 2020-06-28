---
title:  "Window Docker tutorial"
search: true
categories: 
  - Docker
tag:
  - Docker
  - Window
  - Tutorial
last_modified_at: 2020-06-28T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Docker
meta_keywords: Docker,Window,Tutorial
---

Window 에서 Docker 사용하기.

## 1. Download

![image](https://user-images.githubusercontent.com/34367843/85937822-00c34a00-b942-11ea-82a6-c181724633bc.png)


## 2. Clone

![image](https://user-images.githubusercontent.com/34367843/85937855-5d266980-b942-11ea-9d0d-b44cf1398e97.png)

## 3. Build

![image](https://user-images.githubusercontent.com/34367843/85937875-9363e900-b942-11ea-8711-527617f27cef.png)


| `docker build -t {github_id}/cheers2019` 말고 뒤에 ` .` 이 붙어야한다.  
| `docker build -t {github_id|/cheers2019 .`

## 4. Run

![image](https://user-images.githubusercontent.com/34367843/85937926-3c124880-b943-11ea-883d-77ee86395d5e.png)


### Build Result

![image](https://user-images.githubusercontent.com/34367843/85937933-5b10da80-b943-11ea-9c2a-2102a194b5ae.png)


## 5. Ship

![image](https://user-images.githubusercontent.com/34367843/85937942-6ebc4100-b943-11ea-9dfa-8e77bd22929c.png)


### Login

![image](https://user-images.githubusercontent.com/34367843/85937950-88f61f00-b943-11ea-8fff-a08f2bd4602e.png)


### Docker Push

![image](https://user-images.githubusercontent.com/34367843/85937957-990dfe80-b943-11ea-9738-f24045c8cda1.png)


## Finish

![image](https://user-images.githubusercontent.com/34367843/85937963-ad51fb80-b943-11ea-8a09-66b753faf2aa.png)

튜토리얼이 끝나고 Repository를 확인해보면

![image](https://user-images.githubusercontent.com/34367843/85937975-ce1a5100-b943-11ea-8786-caac41761e78.png)

push한 cheers2019를 확인할 수 있다.
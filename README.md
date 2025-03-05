# Melodify: AI 기반 음악 생성 웹 애플리케이션

## 프로젝트 요약
Melodify는 사용자가 이미지를 업로드하면 해당 이미지의 분위기와 내용을 기반으로 캡셔닝을 생성하고, 이를 활용하여 맞춤형 음악을 생성하는 AI 기반 웹 애플리케이션입니다. 이 프로젝트는 Django 백엔드와 Google Cloud Platform(GCP)의 Vertex AI를 활용하여 다수의 사용자가 동시에 접근할 수 있는 안정적이고 확장 가능한 서비스를 제공합니다.

---

## 프로젝트 배경
기존의 이미지 생성 서비스는 비주얼 중심의 콘텐츠 제공에 초점이 맞춰져 있었습니다. Melodify는 이미지의 감성과 분위기를 음악으로 표현하여 새로운 방식의 창작 경험을 제공합니다. 이 프로젝트는 창작자, 디자이너, 음악 애호가들이 영감을 얻고 창작 활동을 확장할 수 있도록 돕기 위해 설계되었습니다.

---

## 프로젝트 주요 화면

### 팀 멤버
<p align="center">
  <img src="./portfolio_images/member.png" alt="팀 멤버" width="30%">
</p>

### 구현 화면
<table style="width:100%; text-align:center;">
  <tr>
    <td>
      <br>
      <p align="center">
        <img src="./portfolio_images/upload_page.png" alt="업로드 페이지" width="40%">
      </p>
        <br><업로드 페이지><br>사용자가 이미지를 업로드하고, 음악 생성 작업을 시작할 수 있는 페이지입니다.
    </td>
    <td>
      <br>
      <p align="center">
        <img src="./portfolio_images/loading_page.png" alt="로딩 페이지" width="40%">
      </p>
        <br><로딩 페이지><br>음악 생성 작업 중 상태를 보여주는 페이지입니다.
    </td>
    <td>
      <br>
      <p align="center">
        <img src="./portfolio_images/result_page.png" alt="결과 페이지" width="40%">
      </p> 
        <br><결과 페이지><br>생성된 음악을 실시간으로 재생하고 다운로드할 수 있는 페이지입니다.
    </td>
  </tr>
</table>

---
    
## 주요 기능

1. **이미지 업로드 및 저장**
   - 사용자로부터 이미지를 업로드받아 Google Cloud Storage(GCS)에 안전하게 저장합니다.
   - 이미지 처리 속도와 안정성을 위해 비동기 작업 구조를 채택했습니다.

2. **이미지 캡셔닝 생성**
   - OpenAI API를 사용하여 업로드된 이미지의 분위기와 내용을 분석하고, 이를 기반으로 적절한 캡션을 생성합니다.
   - 생성된 캡션은 음악 생성 파이프라인의 입력 데이터로 활용됩니다.

3. **음악 생성**
   - GCP Vertex AI의 머신러닝 모델을 활용하여 자동으로 음악을 생성합니다.
   - 생성 과정은 GCP의 파이프라인을 통해 효율적으로 관리됩니다.

4. **결과 페이지 제공**
   - 생성된 음악을 웹 인터페이스에서 실시간으로 재생하고, 다운로드할 수 있는 옵션을 제공합니다.
   - 사용자 경험을 고려한 직관적인 UI를 설계했습니다.

---

## 🛠 사용 기술 및 라이브러리

- **프론트엔드**: HTML, CSS
- **백엔드**: Django, Django REST Framework
- **AI 및 머신러닝**:
  - OpenAI API: 이미지 캡셔닝 생성
  - GCP Vertex AI: 음악 생성 파이프라인
- **클라우드 및 배포**:
  - Google Cloud Storage: 이미지 및 음악 파일 저장
  - GCP CICD: 자동화된 모델 배포

---

## 🖥 담당한 기능 (ML, Cloud)

- 데이터셋 수집 및 생성
    - **이미지 캡션을 음악전 표현으로 가공**하기 위한 Vertex AI의 Gemini-1.0-pro-002 학습 데이터를 ChatGPT, Gemini로 생성
    - 20개의 다양한 장르로 구성(Pop, Hip-hop, Rock, Jazz 등)하고 Jsonl 파일로 **지도 조정 방식의 싱글턴 형식** 데이터 세트 활용
- 모델 학습 및 배포
    - Vertex Ai Studio - Tuning을 사용하여 데이터셋으로 특정 작업에 맞게 훈련을 진행
    - Vetex Ai Model Registry를 사용하여 모델을 Online Prediction 구현.
    - Docker와 Flask를 사용하여 Container Image를 생성하여 Artifact Registry에 등록, Model Registry를 사용하여 Online Prediction 구현.
- ML 프로세스 설계
    - Google Cloud Storage에 이미지 캡션 저장 및 전송, 생성된 노래 파일 저장
    - Vertex Ai를 사용하여 **이미지 캡션 가공+노래 생성** 작업을 담당하는 컴포넌트로 **Pipeline 구성하여 ML 추론 자동화**
- Google Cloud Pipeline을 통한 음악 생성 프로세스.
    - 이미지 설명을 **Vertex AI의 파이프라인 입력 프롬프트로 전달하여 생성된 음악을 받아오는 과정**을 관리.
    - 3개의 컴포넌트로 구성 - 텍스트 가공 **모델을 로드 및 입력 설정** 컴포넌트, 입력 텍스트를 **음악적 표현으로 가공**하는 컴포넌트, 입력된 음악적 표현을 MusicGen으로 **음악으로 생성**하고 **GCS에 저장**하는 컴포넌트

---

## 💡 성장한 부분

- **Vertex AI 기반 ML 파이프라인 구축 및 최적화**
    - Kubeflow Pipeline (KFP)을 활용하여 텍스트 프롬프트 → 음악 생성 → 저장까지 자동화된 ML 파이프라인을 설계하였습니다. 큰 오픈소스 모델을 사용하면서 리소스 절약을 위해 적합한 모델을 선택하고 반복 실행시 set_caching_options()를 사용하여 캐싱 최적화를 적용하였고Vertex AI Model Registry와 Online Prediction을 활용하여 엔드포인트 기반 AI 서비스에 대해 이해하였습니다.
- **Generative AI 모델과 음악 생성 모델 통합**
    - ChatGPT를 사용한 이미지 캡셔닝, Gemini를 사용한 이미지 캡션 가공, 이를 MusicGen 모델의 입력으로 변환하는 과정을 설계하여 사용자가 사진을 업로드 했을 때 이미지 캡셔닝과 프롬포트 엔지니어링을 통해 사용자의 요구를 반영하는 음악을 생성할 수 있었습니다. 이 과정을 통해 텍스트 기반 음악 변환과 데이터 처리 기술을 배울 수 있었습니다.
- **GPU 최적화 및 클라우드 기반 모델 서빙 경험**
    - CUDA를 활용한 PyTorch 모델 최적화를 진행하였습니다. GCS를 활용해 생성된 오디오 파일을 저장, 클라우드 기반 모델 서빙 경험을 통해 클라우드 기반 데이터 처리 및 서빙 구조 설계 역량을 키울 수 있었습니다. 배포에서 발생하는 오류를 GCP Logging을 활용한 분석을 통해 모델 배포 시 발생한 문제를 해결하고 Arifact Registry와 Docker를 사용하여 모델을 컨테이너화하고 클라우드 서빙을 구축하는 과정에서 MLOps와 배포 자동화에 대한 실무 경험을 얻을 수 있었습니다.
- **ML 서비스의 안정성과 확장성 고려**
    - 모델 추론 결과를 GCS에 저장하여 효율적인 데이터 관리 구조를 설계했습니다. GCP Vertex AI Pipeline을 활용해 재사용 가능한 ML 컴포넌트로 설계하여 향후 프로젝트에서도 쉽게 활용할 수 있도록 구조화했습니다.
- **클라우드 환경 ML 프로젝트**
    - 클라우드 환경에서 ML모델을 효율적으로 배포하고 운영하는 방법을 익혔습니다. GCP를 활용하며 컴퓨팅 자원을 결정하고 비용이 발생하면서, Compute Engine을 비교하여 프로젝트나 ML모델에 맞는 Compute Engine 혹은 Vertex AI를 활용하였습니다. 또한, GCP IAM을 통해 클라우드 환경에서 역할을 적절하게 할당하여 보안성을 강화하고, 서비스 간 권한을 세분화하는 방법을 배웠습니다. 팀 프로젝트 내에서 ML을 담당하여 클라우드 환경 ML 서비스 구축 역량을 키울 수 있었습니다.
 
---


## 참고 자료
- [발표 자료 PDF 보기](./portfolio_images/Melodify.pdf)

---

### 문의
궁금한 사항은 이메일로 연락해주세요: [jhs789654123@gmail.com]{팀장}
                                  [kwonhc226@gmail.com]{ML}

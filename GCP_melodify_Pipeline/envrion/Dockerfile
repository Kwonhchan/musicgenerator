FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu20.04

# Python 3.9 설치
RUN apt-get update && apt-get install -y software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa \
    && apt-get update && apt-get install -y python3.9 python3.9-distutils python3.9-venv \
    && apt-get install -y python3-pip

# pip3.9를 기본 pip로 링크
RUN ln -sf /usr/bin/python3.9 /usr/bin/python3 \
    && ln -sf /usr/bin/pip3 /usr/bin/pip

# PyTorch 및 관련 패키지 설치
RUN pip install torch==2.4.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# 나머지 패키지 설치 (기본 PyPI 인덱스에서 설치)
RUN pip install transformers scipy==1.11.4 google-cloud-aiplatform==1.60.0 vertexai==1.43.0 google-cloud-storage==2.14.0

# 작업 디렉토리 설정
WORKDIR /app

# Hugging Face 캐시 디렉토리 변경 (디스크 공간 확보를 위해)
ENV HF_HOME=/app/huggingface_cache

# 모델 미리 다운로드
RUN python3 -c "from transformers import AutoProcessor, MusicgenForConditionalGeneration; \
               AutoProcessor.from_pretrained('facebook/musicgen-medium'); \
               MusicgenForConditionalGeneration.from_pretrained('facebook/musicgen-medium')"

# 작업 디렉토리 설정
WORKDIR /app

# 기본 명령어 설정
CMD ["python3"]

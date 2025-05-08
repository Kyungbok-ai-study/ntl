#ntl
# 📘 제48회 물리치료사 국가시험 문제 처리

이 노트북은 스캔된 PDF 파일로부터 Google Vision OCR을 사용하여 전체 시험 문제를 추출하고, 각 문항과 보기 항목을 정제하여 구조화된 JSON 데이터로 저장하는 작업을 수행합니다. 이 데이터는 AI 모델 학습에 활용할 수 있습니다.

---

## 📌 주요 기능

1. **API 키 업로드 및 환경 설정**
   - Google Cloud에서 발급받은 `.json` 형식의 서비스 계정 키를 사용하여 Vision API를 인증합니다.

2. **PDF → 이미지 변환**
   - `pdf2image` 라이브러리를 이용해 PDF의 각 페이지를 `.png` 이미지로 변환합니다.

3. **헤더/푸터 제거 및 좌우 분할**
   - 각 이미지에서 상단과 하단을 잘라내고, 2단 형식의 시험지를 고려하여 좌/우로 분할합니다.

4. **Google Vision OCR 수행**
   - 처리된 이미지에서 텍스트를 추출합니다.

5. **문항 및 보기 추출**
   - `"번호. 질문"` 형식과 `"①~⑤ 보기"` 형식을 인식하여 분리합니다.
   - 질문이 여러 줄에 걸쳐 있을 경우도 처리하며, 글머리 기호 누락 등 OCR 오류를 보완합니다.

6. **JSON 형식으로 저장**
   - 최종적으로 각 문항은 `question_id`, `question`, `answer` 구조로 저장됩니다.

---

## 🛠️ 실행 환경

- Python 3.7+
- Google Colab 또는 로컬 환경에서 다음 패키지 설치:
  - `google-cloud-vision`
  - `google-cloud-storage`
  - `pdf2image`
  - `poppler-utils`
  - `opencv-python`

---

## 🔐 API 키 (.json 파일) 생성 방법

1. [Google Cloud Console](https://console.cloud.google.com/) 접속
2. 새 프로젝트 생성 또는 기존 프로젝트 선택
3. **APIs & Services > Credentials** 이동
4. **“Create credentials” → Service account** 선택
5. 서비스 계정 생성 후 **“Manage keys”** 클릭
6. **“Add key → Create new key → JSON”** 형식 선택
7. `.json` 파일이 자동 다운로드됩니다 → Colab에 업로드해서 사용

✅ Vision API 사용을 위해 아래 링크에서 활성화 필요:  
https://console.cloud.google.com/flows/enableapi?apiid=vision.googleapis.com

---

## 🚀 사용 방법

1. Colab에서 이 노트북 파일 열기
2. 다음 파일들을 업로드:
   - PDF 스캔 파일 (시험지 원본)
   - Google Cloud API 키 파일 (.json)
3. 셀을 순서대로 실행:
   - 라이브러리 설치
   - 이미지로 변환
   - OCR 수행
   - 문항 처리 및 JSON 저장

---

## 📁 결과물

- `output/page_x.txt`: 각 페이지의 OCR 결과 텍스트
- `48th_exam_full.json`: 정제된 모든 문항과 보기 리스트

---

## 📌 주의 사항

전체적으로 Google Vision OCR을 통해 PDF 파일의 텍스트를 잘 추출하였지만, 다음과 같은 OCR 한계로 인해 일부 문항은 추가 정제가 필요할 수 있습니다:

- 글머리 기호(예: •) 누락
- 보기 번호(①, ②...)가 잘못 인식되거나 다른 문자로 변환됨

이를 보완하기 위해 정규표현식(`re`)을 추가적으로 구성하여 더 정확하게 문항을 분리하도록 개선할 수 있습니다.

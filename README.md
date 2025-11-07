# 📰 Short Kinds – 자동 뉴스 요약 기반 비주얼 & 퀴즈 생성 모듈

> “긴 기사를 부담 없이, 재미있게!”  
> **Short Kinds**는 뉴스 요약문을 기반으로 **쇼츠를 생성**하는 AI 콘텐츠 생성 모듈입니다.  
> 본 레포지토리는 **요약 → 프롬프트 → 퀴즈 → 이미지 생성 파이프라인**을 담당합니다.

---

## 🚀 개요 (Overview)

이 모듈은 뉴스 요약문(`news_summaries`) 4개를 입력받아 다음을 수행합니다:

1. **Llama 3.1 8B Instruct 모델**을 이용해  
   - 영어 기반 **이미지 생성 프롬프트(prompt)** 생성  
   - 한국어 **퀴즈(JSON 포맷)** 자동 생성  
2. **OpenAI GPT-Image-1 API**를 호출하여  
   - 프롬프트를 기반으로 **2×2 형태의 4컷 만화 이미지**를 생성  

> ✅ 결과물: `"prompt"`(영문 프롬프트), `"quiz"`(한글 JSON), `"output_x.png"`(생성 이미지)

---

## 🧠 처리 과정 (Pipeline)

```text
입력: [뉴스 요약문 4개]
   ↓
[Llama 3.1-8B-Instruct]
   ├─ 영어 프롬프트 생성 (cartoon 4-panel style)
   └─ 한국어 객관식 퀴즈(JSON) 생성
   ↓
[GPT-Image-1]
   └─ 4컷 만화 이미지(1024×1024) 생성
출력: {"prompts": <str>, "quiz": <dict>, "images": <png files>}

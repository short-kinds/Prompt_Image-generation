📰 Short Kinds – 자동 뉴스 요약 기반 비주얼 & 퀴즈 생성 모듈

“긴 기사를 부담 없이, 재미있게!”
Short Kinds는 뉴스 요약문을 기반으로 프롬프트·퀴즈·4컷 만화 이미지를 자동 생성하는 모듈입니다.
본 레포지토리는 그중 콘텐츠 생성 파이프라인 (요약 → 프롬프트 → 퀴즈 → 이미지) 부분의 코드와 구조를 다룹니다.

🚀 개요 (Overview)

이 모듈은 뉴스 요약문(news_summaries) 4개를 입력받아 다음 과정을 수행합니다:

Llama 3.1 Instruct 모델을 통해

영어 기반 이미지 생성 프롬프트(prompt) 작성

한글 퀴즈(JSON 포맷) 자동 생성

OpenAI GPT-Image-1 API를 사용하여

프롬프트를 기반으로 2×2 구성의 4컷 만화 이미지 생성

✅ 최종 결과물은 "prompt"(영어 텍스트)와 "quiz"(한국어 JSON 데이터), "output_x.png"(생성 이미지 파일)입니다.

🧠 처리 과정 (Pipeline)
입력: [뉴스 요약문 4개]
   ↓
[Llama 3.1-8B-Instruct]
   ├─ 영어 프롬프트 생성 (cartoon 4-panel style)
   └─ 한국어 객관식 퀴즈(JSON) 생성
   ↓
[GPT-Image-1]
   └─ 4컷 만화 이미지(1024×1024) 생성
출력: {"prompts": <str>, "quiz": <dict>, "images": <png files>}

⚙️ 기술 스택 (Tech Stack)
구분	기술
모델	meta-llama/Llama-3.1-8B-Instruct
이미지 생성	OpenAI GPT-Image-1
프레임워크	PyTorch, HuggingFace Transformers
언어	Python 3.10+
출력 포맷	JSON (퀴즈), PNG (이미지)
🧩 주요 기능 (Features)
1. 프롬프트 생성

뉴스 요약문을 받아 GPT-Image용 영어 프롬프트를 생성합니다.

4개의 요약문을 각각 4컷으로 시각화할 수 있게 구성합니다.

자연스러운 인체 표현, 무문자(no-text), 일관된 색조 등의 제약 조건 포함.

2. 퀴즈 생성

4개 요약문을 종합하여 한국어 객관식 1문항을 자동 생성합니다.

JSON 포맷으로 출력되며, 필수 스키마는 다음과 같습니다:

{
  "language": "ko",
  "questions": [
    {
      "type": "mcq",
      "question": "문항 내용",
      "options": ["A", "B", "C", "D"],
      "answer": "A",
      "explanation": "정답 해설"
    }
  ]
}


정답은 무작위 위치로 재배치되어 항상 A가 되는 문제를 방지합니다.

3. 이미지 생성

생성된 프롬프트를 GPT-Image-1에 전달하여
1024×1024 크기의 2×2 4컷 만화를 생성합니다.

생성된 이미지는 output_1.png, output_2.png 형태로 저장됩니다.

🔩 코드 구조 (Code Structure)
📂 short-kinds-visualizer/
├── main.py               # 실행 엔트리
├── llama3_module.py      # 요약 → 프롬프트 & 퀴즈 생성
├── image_gen.py          # GPT-Image-1 호출 및 저장
└── examples/
    └── sample_input.json # 예시 입력(news_summaries)

주요 함수
함수명	설명
llama3(news_summaries)	Llama 3.1 모델로 프롬프트 및 퀴즈 생성
_safe_json_extract()	모델 출력에서 JSON 블록 안전 추출
gpt_image_1(prompts)	OpenAI GPT-Image-1로 이미지 생성
🧪 예시 실행 (Example Run)
news_summaries = [
  "행안부는 소비쿠폰 신청 첫날 415만 명이 참여했다고 밝혔다.",
  "일부 카드사 웹사이트는 접속 지연을 겪었다.",
  "행안부 차관은 시스템 안정성 확보를 강조했다.",
  "지원금은 계층별로 15~40만원이 지급된다."
]

out = llama3(news_summaries)
print(json.dumps(out["quiz"], ensure_ascii=False, indent=2))
gpt_image_1(out["prompts"])


출력 예시

프롬프트 생성 완료!!
퀴즈 생성 완료!!
이미지 생성 완료: output_1.png

🧱 나의 담당 역할 (My Contribution)
영역	담당 내용
✏️ Prompt Engineering	뉴스 요약문 기반 영어 프롬프트 설계 및 Llama 3.1 활용
🧩 Quiz Generation	한국어 객관식 퀴즈 자동 생성 및 JSON 파싱/검증 로직 구현
🎨 Image Automation	GPT-Image-1 API 연동 및 4컷 만화 이미지 자동 저장
🧱 Pipeline Integration	프롬프트–퀴즈–이미지 생성 전체 파이프라인 자동화
💡 기대 효과 (Expected Impact)

뉴스 접근성 향상: 요약·시각화를 통해 뉴스 문해력(Literacy) 향상

콘텐츠 다양화: AI 기반 뉴스 퀴즈·이미지 자동 생성으로 신규 서비스 모델 창출

사용자 참여 유도: 퀴즈·리워드 구조로 능동적 뉴스 소비 촉진

🧭 Reference

BIG KINDS API (뉴스 데이터 수집)

OpenAI API (GPT-Image-1, Llama 계열)

한국언론진흥재단 데이터 기반 뉴스 요약

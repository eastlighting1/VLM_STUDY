# OCR-free Document Understanding Transformer

## 서론

- Commercial invoice, receipt, business card와 같은 문서 이미지는 현대 업무 환경에서 흔하게 찾을 수 있음.
    - commercial invoice : 상업 송장 (최종 거래 명세서)

- 이러한 문서 이미지로부터 의미있는 정보를 추출하기 위해, VDU(Visual Document Understanding)은 산업에 필수적인 작업이 되었을 뿐만 아니라, 연구자들에게도 도전적으로 받아들이는 주제가 됨.
    - 대표적인 응용 분야 :
        - Document Classification
            - Deepdocclassifier : https://www.nature.com/articles/s41562-025-02203-8
            - CNN for Document Image Classification : https://ieeexplore.ieee.org/document/6977258
        - Information Extraction
            - POST OCR TAGGING BASED PARSER : https://openreview.net/pdf/eb1274f0d65b4c3e30e951b4668b7f982338a628.pdf
            - Representation Learning for Information Extraction from Form-like Documents : https://aclanthology.org/2020.acl-main.580/
        - Visual Question Answering (VQA)
            - Docvqa : https://arxiv.org/abs/2007.00398
            - ICDAR 2021 Competition on Document VisualQuestion Answering : https://arxiv.org/abs/2111.05547

- VDU 방법론들은 2단계로 이 과제를 해결함.
    1. 문서 이미지를 읽는다.
    2. 문서를 전체적으로 이해한다.
        - holistic understanding : 전체론적 이해
    - 대표적인 방법론들
        - BROS : https://arxiv.org/abs/2108.04539
        - POST OCR TAGGING BASED PARSER
        - SPADE : https://arxiv.org/abs/2005.00642
        - LayoutLM Series : https://arxiv.org/abs/2012.14740
            - v1 : https://arxiv.org/abs/1912.13318 (Pre-training of Text and Layout for Document Image Understanding)
            - v2 : https://arxiv.org/abs/2205.12682 (Multi-modal Pre-training for Visually-Rich Document Understanding)
            - v3 : https://arxiv.org/abs/2204.08387 (Pre-training for Document AI with Unified Text and Image Masking)
    - 이러한 방법론들은 주로 텍스트를 읽기 위해 딥러닝 기반의 OCR에 의존하며, 문서를 이해하는 부분을 모델링하는 것에 초점을 둚.
        - 3개의 모듈 : Text Detection (위치 파악), Text Recognition (실제 문자로 읽기), Parsing (읽은 정보를 의미 있는 구조로 분석)
    - 그러나, 이런 OCR 의존적인 접근들은 여러 문제점을 가지고 있음.
        1. OCR을 전처리 도구로 사용하는 것은 비쌈.
            - OCR 모델을 직접 학습시킬 경우 막대한 학습 비용과 대규모 데이터셋을 필요로 함.
            - 기성 OCR 엔진을 사용한다고 해도 고품질의 결과를 얻기 위한 추론 단계의 계산 비용이 클 수도 있으며, 일반화 성능이 떨어질 수 있음.
        2. OCR이 가진 오류가 VDU 시스템으로 전파되어 이후의 처리 과정에서 부정적인 영향을 줄 수 있음.
            - 특히 이 문제는 한국어나 중국어 같이 복잡한 문자 집합을 가지기 떼문에 OCR 품질이 비교적 더 낮은 언어에서 더 드러남.
            - 통상적으로, 이 문제를 해결하기 위해 Correction(보정) 모듈을 도입하기도 하지만 전체 시스템의 크기와 유지보수 비용을 증가시키는 문제를 낳음.

- DONUT에서는 이 문제를 'OCR 없이도 원본 입력 이미지에서 원하는 출력으로 직접 매핑되는 방식'을 모델링하여 해결함.
    - DONUT : DOcumeNt Understanding Transformers
    - Transformer-Only 구조를 기반으로 사전 학습 및 파인 튜닝을 진행하는 방식을 적용함.
    - 사전 학습 단계의 목표는 '텍스트를 읽는다'임.
        - 문서 이미지와 annotation을 기반으로, 'annotation의 일부분'에 해당하는 previous text context와 이미지를 주고 annotation의 다음 token을 맞히는 방식으로 진행함.
        - 단순히 텍스트를 읽는 작업이기 때문에 synthetic data를 사용하여 사전학습을 수행할 수 있으므로, 도메인과 언어에 대한 유언성을 쉽게 구현 가능함.
    - 파인 튜닝 단계에서는 downstream 과제에 맞추어 문서 전체를 이해하도록 함.


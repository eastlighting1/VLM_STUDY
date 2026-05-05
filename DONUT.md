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
    
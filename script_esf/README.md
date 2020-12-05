# ESFWN-based ESL Annotator 
**ESFWN-based ESL Annotator**는 **E**vent **S**tructure **F**rame-annotated **W**ord**N**et-based **E**vent **S**tructure **L**exicon **Annnotator**의 약자로 문장을 입력으로 받아서 문장 내의 동사에 대해 "사건구조프레임"과 "의미역" 등 단어 의미 정보를 출력하는 자동 주석기입니다.

## 주석기 아키텍쳐(Annotator Architecture)
![](C:\Users\seohy\ESFWN-based-ESL-Annotator\ESFWN-based_esf_annotation_architecture)

## 주석기 구성요소 (Annotator Components)
1. 단어 중의성 해소 및 의미 주석 알고리즘 (EWISER & ewiser_wrapper)
2. 의미역 라벨링 도구 (AllenNLP SRL)
3. 사건구조프레임 주석 워드넷 (ESFWN)
4. 사건구조프레임 목록 (ESF_list)
5. 동사 불규칙 굴절 사전 (vInflection)

## 설치 (Installation)
 ####1. [Anaconda3](https://www.anaconda.com/products/individual) 다운로드 및 설치 
    사이트에서 installer 다운로드 받아서 설치

 ####2. [pytorch 1.5](https://pytorch.org/)와 [torch_sparse](https://github.com/rusty1s/pytorch_sparse) 설치
    cuda 10.1 사용: CUDA=cu101,
    cpu 사용: CUDA = cpu
    `conda install pytorch=1.5.1 torchvision cudatoolkit=10.1 -c pytorch`
    `pip install torch-scatter torch-sparse -f https://pytorch-geometric.com/whl/torch-1.5.0+${CUDA}.html`

 ####3. [Spacy](https://spacy.io/usage) 설치
    `conda install -c conda-forge spacy`

 ####4. [ewiser](https://github.com/SapienzaNLP/ewiser) 설치 
    `git clone https://github.com/SapienzaNLP/eiwser.git`
    `cd ewiser`
    `pip install -r requirements.txt`
    `pip install -e .`
    
 ####5. ewiser English checkpoints를 [여기](https://drive.google.com/file/d/11RyHBu4PwS3U2wOk-Le9Ziu8R3Hc0NXV/view)에서 다운로드해서 ewiser 폴더에 넣으세요.

 ####6. [AllenNLP SRL](https://demo.allennlp.org/semantic-role-labeling) 설치
    `pip install allennlp==1.0.0 allennlp-models==1.0.0`
   
 ####7. ESL Annotator 패키지 설치
    `git clone https://github.com/ihaeyong/drama-graph/script_esf.git`

## 사용법

 - generate_event_structure_lexicon.py에서 'ewiser_path'와 'ewiser_input' 폴더 경로를 [your_path]로 수정하세요.
- 출력을 얻기 위해 아래의 코드를 실행하세요. <br>
`python generate_event_structure_lexicon.py """your sentence"""`

## 입출력 예시

> 입력: John arrived in Seoul yesterday.
> 출력: esl_annotation.result.json
> 

## 용어 정리 (terminology)

 1. **단어 중의성 해소**(Word Sense Disambiguation; WSD)
텍스트에 출현하는 단어의 의미를 결정해서 여러가지 뜻으로 쓰일 수 있는 가능성을 제거해 주는 NLP 태스크. 

> 단어 중의성 해소 예시: **arrive**  (문장: "John arrived in Seoul yesterday.")
> sense_number: arrive.v.01
> offset: wn:02005948v

2. **사건구조프레임**(Event Structure Frame; ESF)
문장에서 동사가 나타내는 사건 전후의 변화를 포착하기 위해 <전상태(pre-state), 진행(process), 후상태(post-state)>로 구조화한  의미구조. 
아래 예시는 t1에 John이 도착점인 Seoul에 있지 않고, 출발점(source_location)에 있다가 arriving (t2)후 t3에 John은 Seoul에 있게 됨을 의미한다.

> 사건구조프레임 예시: **arrive**  (문장: "John arrived in Seoul yesterday.")
> se1: pre-state: not_be (John, in_Seoul, t1)
> se2: pre-state: be (John, at_source_location, t1)
> se3: process: putting (John, the_book, on_the_table, t2)
> se4: post-state: be (the_book, on_the_table, t3)
> se5: pos-state: not_be (the_book, at_source_location, t3)

 3. **ESFWN** (Event Structure Frame-annotated WordNet)
ESFWN은 영어 워드넷의 모든 동사 신셋 24601개에 대해 사건구조프레임을 주석해 놓은 사전입니다. 아래는 ESFWN 항목중 *arrive.v.01* 의미의 예.
> {"VERB": "arrive", "SENSE_NUMBER": "arrive.v.01", "SENSE_KEY": "arrive%2:38:00::", "OFFSET": "wn:02005948v", "ESF_TYPE": "MOVE_TO_GOAL", "SYNONYMS": ["arrive", "get", "come"], "HYPERNYMS": [], "VID": 861}

 4. **의미역**(Semantic Role)
의미역은 문장에 표현된 사건의 참여자와 시간, 장소, 원인, 목적, 방법 등. 다음은 AllenNLP SRL에 의해 주석된 의미역 예. ARG0은 참여자, ARGM-LOC은 장소, ARGM-TMP는 시간.

> 의미역 주석 예 (문장: "John arrived in Seoul yesterday.")
> {"ARG0": "John", "VERB": "arrived", "ARGM-LOC": "in_Seoul", "ARGM-TMP": "yesterday"}

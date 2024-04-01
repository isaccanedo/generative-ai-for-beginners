# 고급 프롬프트 생성

[![Creating Advanced Prompts](../../images/05-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/32GBH6BTWZQ?WT.mc_id=academic-105485-koreyst)

이전 장에서 배운 내용을 간단히 되짚어보겠습니다:

> 프롬프트 엔지니어링은 **모델이 더 관련성 있는 응답을 할 수 있도록** 더 유용한 지시사항이나 문맥을 제공하여 모델을 안내하는 과정입니다.

프롬프트 작성에는 두 가지 단계가 있습니다. 첫 번째는 관련 문맥을 제공하여 프롬프트를 구성하는 것이고, 두 번째는 프롬프트를 점진적으로 개선하는 _최적화_ 입니다.

이 시점에서 우리는 프롬프트를 작성하는 방법에 대해 기본적인 이해를 갖고 있지만, 더 깊게 파고들어야 합니다. 이 장에서는 다양한 프롬프트를 시도해보고, 왜 한 프롬프트가 다른 프롬프트보다 더 좋은지 이해하는 방법을 배우게 될 것입니다. 모든 LLM에 적용할 수 있는 몇 가지 기본 기술을 사용하여 프롬프트를 구성하는 방법을 배우게 될 것입니다.

## 소개

이 장에서는 다음과 같은 주제를 다룰 것입니다:

- 다양한 기법을 적용하여 프롬프트 엔지니어링에 대한 지식을 확장합니다.
- 출력을 다양하게 하도록 프롬프트를 구성합니다.

## 학습 목표

이 레슨을 완료한 후에는 다음을 할 수 있게 될 것입니다:

- 프롬프트 엔지니어링 기법을 적용하여 프롬프트의 결과를 개선합니다.
- 다양하거나 결정론적인 프롬프트를 수행합니다.

## 프롬프트 엔지니어링

프롬프트 엔지니어링은 원하는 결과를 얻을 수 있는 프롬프트를 생성하는 과정입니다. 텍스트 프롬프트를 작성하는 것 이상이 있습니다. 프롬프트 엔지니어링은 엔지니어링 학문이 아니라 원하는 결과를 얻기 위해 적용할 수 있는 기술의 집합입니다.

### 프롬프트의 예

> 지리에 대해 10개의 질문을 생성하세요.

이 프롬프트에서 여러 가지 다른 프롬프트 기법을 적용하고 있습니다.

자세히 살펴보겠습니다.

- **문맥**, "지리"에 대한 내용을 지정합니다.
- **출력 제한**, 10개 이상의 질문을 원하지 않습니다.

### 단순한 프롬프트의 한계

원하는 결과를 얻을 수도 있고, 얻지 못할 수도 있습니다. 질문은 생성되지만, 지리는 큰 주제이며 원하는 내용을 얻지 못할 수 있습니다. 이는 다음과 같은 이유로 인해 발생할 수 있습니다:

- **큰 주제**, 국가, 수도, 강 등에 대한 질문인지 알 수 없습니다.
- **형식**, 질문을 특정한 방식으로 포맷을 지정하고 싶은 경우 어떻게 해야 할까요?

프롬프트를 작성할 때 고려해야 할 사항이 많다는 것을 알 수 있습니다.

지금까지 간단한 프롬프트 예제를 살펴보았지만, 생성형 AI는 다양한 역할과 산업에서 사람들을 돕기 위해 더 많은 것을 할 수 있습니다. 다음에는 몇 가지 기본 기술을 살펴보겠습니다.

### 프롬프트 기법

먼저, 프롬프트는 LLM의 _비롯된_ 특성이라는 점을 이해해야 합니다. 이는 모델에 내장된 기능이 아니라 모델을 사용하면서 우리가 발견하는 것입니다.

LLM을 프롬프트하기 위해 몇 가지 기본적인 기법을 사용할 수 있습니다. 이제 그것들을 살펴보겠습니다.

- **퓨 샷 프롬프트 (Few-shot prompting)**, 이것은 가장 기본적인 프롬프트 형태입니다. 몇 가지 예제와 함께 한 개의 프롬프트로 구성됩니다.
- **생각의 사슬 (Chain-of-thought)**, 이 유형의 프롬프트는 LLM에게 문제를 단계별로 해결하는 방법을 알려줍니다.
- **생성된 지식 (Generated knowledge)**, 프롬프트의 응답을 개선하기 위해 프롬프트 외에도 생성된 사실이나 지식을 제공할 수 있습니다.
- **작은 것부터 크게 (Least to most)**, 생각의 사슬와 마찬가지로, 이 기법은 문제를 일련의 단계로 분해하고 이 단계들을 순서대로 수행하도록 요청하는 것입니다.
- **자기 개선 (Self-refine)**, 이 기법은 LLM의 출력을 비판하고 개선하도록 요청하는 것입니다.
- **Maieutic prompting**, 여기서 원하는 것은 LLM의 답변이 정확한지 확인하고, 답변의 다양한 부분을 설명하도록 요청하는 것입니다. 이는 Self-refine의 한 형태입니다.

### 퓨 샷 프롬프트 (Few-shot prompting)

이 프롬프트 스타일은 매우 간단하며, 단일 프롬프트와 몇 가지 예제로 구성될 수 있습니다. 이 기법은 LLM에 대해 배우기 시작하는 단계에서 사용할 것입니다. 다음은 예시입니다:

- 프롬프트: "대수학이란 무엇인가요?"
- 답변: "대수학은 수학 기호와 이러한 기호를 조작하는 규칙을 연구하는 수학의 한 분야입니다."

### 생각의 사슬 (Chain-of-thought)

Chain-of-thought는 LLM을 일련의 단계를 거쳐 가르치는 매우 흥미로운 기술입니다. 이 아이디어는 LLM에게 어떻게 해야 하는지 이해시키는 방법입니다. 다음 예시를 고려해보세요. Chain-of-thought를 사용하는 경우와 그렇지 않은 경우를 비교해보세요:

  - 프롬프트: "앨리스는 사과를 5개 가지고 있습니다. 3개의 사과를 던지고, 2개를 밥에게 주었고, 밥이 하나를 다시 돌려줬을 때, 앨리스는 몇 개의 사과를 가지고 있을까요?"
  - 답변: 5

LLM은 5라고 답했지만, 이는 잘못된 답입니다. 정답은 계산에 따라 (5 - 3 - 2 + 1 = 1) 1개의 사과입니다.

그렇다면 어떻게 LLM에게 올바르게 계산하는 방법을 가르칠 수 있을까요?

Chain-of-thought를 시도해보겠습니다. Chain-of-thought를 적용하는 방법은 다음과 같습니다:

1. LLM에게 비슷한 예제를 제공합니다.
2. 계산 방법과 올바르게 계산하는 방법을 보여줍니다.
3. 원래의 프롬프트를 제공합니다.

다음과 같이 해보겠습니다:

- 프롬프트: "리사는 사과를 7개 가지고 있습니다. 1개의 사과를 던지고, 4개의 사과를 바트에게 주고, 바트가 하나를 다시 돌려줍니다:
    7 - 1 = 6
    6 - 4 = 2
    2 + 1 = 3
    앨리스는 사과를 5개 가지고 있습니다. 3개의 사과를 던지고, 2개를 밥에게 주고, 밥이 하나를 다시 돌려준다면, 앨리스는 몇 개의 사과를 가지고 있을까요?"  
- 답변: 1

비슷한 예제, 계산 방법, 그리고 원래의 프롬프트를 포함한 긴 프롬프트를 작성하고, 올바른 답인 1에 도달합니다.

Chain-of-thought는 매우 강력한 기술임을 알 수 있습니다.

### 생성된 지식 (Generated knowledge)

회사의 데이터를 사용하여 프롬프트를 구성하고 싶을 때가 많습니다. 프롬프트의 일부는 회사에서 가져온 내용이어야 하고, 다른 일부는 실제로 관심 있는 프롬프트여야 합니다.

예를 들어, 보험업을 하는 경우 프롬프트는 다음과 같이 구성될 수 있습니다:

    ```text
    {{company}}: {{company_name}}
    {{products}}: 
    {{products_list}}
    다음 예산과 요구 사항에 따라 보험을 제안해주세요:
    예산: {{budget}}
    요구 사항: {{requirements}}
    ```

위에서는 템플릿을 사용하여 프롬프트가 구성되는 방법을 볼 수 있습니다. 템플릿에는 `{{변수}}`로 표시된 여러 변수가 있으며, 이 변수들은 회사 API에서 실제 값으로 대체될 것입니다.

회사의 내용으로 변수가 대체된 프롬프트 예시를 살펴보겠습니다:

    ```text
    보험 회사: ACME Insurance
    보험 상품 (월 별 비용): 
    - 자동차, 저렴함, 500 USD
    - 자동차, 비쌈, 1100 USD 
    - 주택, 저렴함, 600 USD
    - 주택, 비쌈, 1200 USD
    - 생명, 저렴함, 100 USD

    다음 예산과 요구 사항에 따라 보험을 제안해주세요:
    예산: $1000
    요구 사항: 자동차, 주택
    ```

이 프롬프트를 LLM에 적용하면 다음과 같은 응답이 생성됩니다:

    ```output
    , and Life insurance

    예산과 요구 사항에 따라 ACME 보험으로부터 다음과 같은 보험 패키지를 제안합니다:
    - 자동차, 저렴함, 500 USD 
    - 주택, 저렴함, 600 USD 
    - 생명, 저렴함, 100 USD 
    총 비용: $1,200 USD
    ```

보시다시피, 이는 생명 보험도 제안하는데, 그렇게 되면 안 됩니다. 이 결과는 우리가 원하는 결과를 얻기 위해 프롬프트를 최적화해야 함을 나타냅니다. _시행착오_ 를 거쳐 다음과 같은 프롬프트로 도달합니다:

    ```text
    보험 회사: ACME Insurance
    보험 상품 (월 별 비용): 
    - 유형: 자동차, 저렴함, 비용: 500 USD
    - 유형: 자동차, 비쌈, 비용: 1100 USD 
    - 유형: 주택, 저렴함, 비용: 600 USD
    - 유형: 주택, 비쌈, 비용: 1200 USD
    - 유형: 생명, 저렴함, 비용: 100 USD

    다음 비용과 요구 사항에 따라 보험을 제안해주세요:
    예산: $1000, 자동차, 주택 유형으로 선택 범위 제한
    ```

_유형_ 과 _비용_ 를 추가하고 _제한_ 키워드를 사용하는 것이 어떻게 우리가 원하는 바를 LLM에게 이해시키는 데 도움이 되는지 주목하세요.

이제 우리는 다음과 같은 응답을 얻습니다:

    ```output
    예산과 요구 사항에 따라, 월 별 500 USD인 자동차, 저렴한 보험을 제안합니다.
    ```

이 예시의 목적은 _Generated knowledge_ 과 같은 기본적인 기술을 사용하더라도 대부분의 경우 원하는 결과를 얻기 위해 프롬프트를 최적화해야 한다는 것을 보여주는 것이었습니다.

### 작은 것부터 크게 (Least to most)

Least-to-most 프롬프팅은 큰 문제를 작은 하위 문제로 분할하여 해결 방법을 안내하는 것입니다. 이렇게 하면 LLM에게 큰 문제를 "정복"하는 방법을 안내할 수 있습니다. 데이터 과학의 좋은 예로, LLM에게 다음과 같이 문제를 분할하도록 요청할 수 있습니다:

> 프롬프트: 5단계로 데이터 과학을 수행하는 방법은 무엇인가요?

AI 어시스턴트의 답변:

1. 데이터 수집
2. 데이터 정제
3. 데이터 분석
4. 데이터 시각화
5. 데이터 프레젠테이션

### 자기 개선, 결과 비판 (Self-refine, critique the results)

생성 모델과 LLM을 사용할 때, 출력에 절대적으로 신뢰할 수 없습니다. 반드시 검증해야 합니다. LLM은 단지 다음으로 가장 가능성이 높은 답변을 제시할 뿐이지, 정확한 답변을 제공하는 것은 아닙니다. 따라서, LLM에게 자기 비판을 요청하여 자기 개선 기법에 도달할 수 있습니다.

이 기법은 다음과 같은 단계를 따릅니다:

1. LLM에게 문제 해결을 요청하는 초기 프롬프트
2. LLM의 답변
3. 답변을 비판하고 AI에게 개선을 요청
4. LLM이 비판을 고려하여 다시 답변하고 제안한 해결책을 제시

이 과정을 원하는 만큼 반복할 수 있습니다.

이 기술을 사용한 예시는 다음과 같습니다:

> 프롬프트: "products와 customers라는 라우트를 가진 Python 웹 API를 생성하세요"

AI 답변:

    ```python
    import flask
    app = Flask(__name__)
    
    @app.route('/products')
    def products():
        return 'Products'
    
    @app.route('/customers')
    def customers():
        return 'Customers'
    ```

> 프롬프트: 위의 코드를 개선할 세 가지 제안을 해주세요.

AI 답변:

    ```text
    물론, 다음은 코드를 개선하기 위한 세 가지 제안입니다:
    
    올바른 모듈을 가져오세요: 첫 번째 줄은 flask 대신에 Flask를 가져와야 합니다. 올바른 import 문은 from flask import Flask여야 합니다.
    
    main 함수를 추가하세요: 파일이 실행될 때 애플리케이션을 실행하기 위해 if __name__ == '__main__': 블록을 파일의 끝에 추가하세요.
    
    f-strings를 사용하세요: route 함수에서 반환문을 포맷팅하기 위해 f-strings를 사용하세요. 예를 들어, return 'Products'를 return f'This is the {request.path} page'로 변경할 수 있습니다.
    ```

    ```python
    from flask import Flask, request
    
    app = Flask(__name__)
    
    @app.route('/products')
    def products():
        return f'This is the {request.path} page'
    
    @app.route('/customers')
    def customers():
        return f'This is the {request.path} page'
    
    if __name__ == '__main__':
        app.run()
    ```

위의 AI 응답은 첫 번째 답변을 비판함으로써 첫 번째 제안된 코드를 개선하고 있음을 확인할 수 있습니다.

### Maieutic prompting

Maieutic prompting은 자기 개선 (Self-refine)과 유사한 기술이지만, LLM에게 스스로를 설명하도록 요청하는 것입니다. 목표는 LLM의 출력에서 일관성을 줄여 올바른 답변에 도달하는 것입니다. 따라서 다음과 같은 워크플로우를 따릅니다:

1. LLM에게 질문에 대한 답변을 요청합니다.
2. 답변의 각 부분에 대해 LLM에게 더 자세히 설명하도록 요청합니다.
3. 일관성이 없는 부분이 있다면, 일관성이 없는 부분을 제외합니다.

2와 3을 반복하여 모든 부분을 확인하고 답변에 만족할 때까지 진행합니다.

다음은 예시 프롬프트입니다:

> 프롬프트: 5단계로 팬데믹을 완화하기 위한 위기 대응 계획을 어떻게 작성할 수 있을까요?  

LLM 답변:

    ```text
    1. 위험 요소 식별
    1. 이해 관계자 식별
    1. 자원 식별
    1. 조치 사항 식별
    1. 의사 소통 채널 식별
    ```

5단계가 식별되었지만, 이것이 맞는지 확인할 수 있을까요? 각 단계를 설명하도록 LLM에게 요청해봅시다:

> 프롬프트: 첫 번째 단계를 더 자세히 설명해주세요. 팬데믹에서의 위험은 무엇이며 자세히 설명해주세요?

    ```text
    다음은 위험 요소입니다:
    
    - 생명의 손실
    - 사업의 손실
    - 평판의 손실
    - 고객의 손실
    - 직원의 손실
    ```

이 시점에서, 우리는 LLM이 올바른지 확인하고자 첫 번째 위험을 더 자세히 설명하도록 요청하고, 위의 답변과 일관성이 있는지 확인합니다:

> 프롬프트: 팬데믹에서 가장 큰 위험은 무엇이며 그 이유는 무엇인가요?

    ```text
    가장 큰 위험은 생명의 손실입니다. 왜냐하면 가장 중요한 것을 보호해야 하기 때문입니다.
    ```

> 팬데믹에서 가장 큰 위험은 무엇인가요?

    ```text
    팬데믹에서 가장 큰 위험은 생명의 손실과 사업의 손실입니다.
    ```

이 시점에서, LLM은 일관성이 있으며 "생명"과 "사업"을 최상위 두 가지 위험으로 언급합니다. 이제 다음 단계로 진행하고 상당히 확신을 갖을 수 있습니다. 그러나 LLM을 맹목적으로 신뢰해서는 안 되며, 항상 출력을 확인해야 합니다.

## 출력 다양성 조절하기

LLM은 본질적으로 결정론적이지 않으므로, 동일한 프롬프트를 실행할 때마다 다른 결과를 얻게 됩니다. 예를 들어 다음 프롬프트를 시도해보세요:

> "파이썬 웹 API를 위한 코드 생성"

    ```python
    # 필요한 라이브러리 가져오기
    import flask
    from flask import request, jsonify
    
    # Flask 앱 인스턴스 생성
    app = flask.Flask(__name__)
    
    # API를 위한 엔드포인트 생성
    @app.route('/api', methods=['GET'])
    def api():
        # 요청 데이터 가져오기
        data = request.args
    
        # 데이터 처리
        result = process_data(data)
    
        # 결과를 JSON 형식으로 반환
        return jsonify(result)
    
    # 데이터 처리 함수
    def process_data(data):
        # 데이터 처리 수행
        result = data + 1
    
        # 결과 반환
        return result
    
    # 앱 실행
    if __name__ == '__main__':
        app.run()
    
    ```

동일한 프롬프트를 다시 실행하면 약간 다른 응답이 생성됩니다:

    ```text
    # 필요한 패키지 가져오기
    import flask
    from flask import request, jsonify
    
    # Flask 앱 생성
    app = flask.Flask(__name__)
    app.config["DEBUG"] = True
    
    # 책 목록 생성
    books = [
        {'id': 0,
         'title': 'A Fire Upon the Deep',
         'author': 'Vernor Vinge',
         'first_sentence': 'The coldsleep itself was dreamless.',
         'year_published': '1992'},
        {'id': 1,
         'title': 'The Ones Who Walk Away From Omelas',
         'author': 'Ursula K. Le Guin',
         'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.',
         'published': '1973'},
        {'id': 2,
         'title': 'Dhalgren',
         'author': 'Samuel R. Delany',
         'first_sentence': 'to wound the autumnal city.',
         'published': '1975'}
    ]
    
    # API를 위한 엔드포인트 생성
    @app.route('/', methods=['GET'])
    def home():
        return '''<h1>Books API</h1>
    <p>책을 검색하기 위한 프로토 타입 API입니다.</p>'''
    
    # 모든 책 반환을 위한 엔드포인트 생성
    @app.route('/api/v1/resources/books/all', methods=['GET'])
    def api_all():
        return jsonify(books)
    
    # 단일 책 반환을 위한 엔드포인트 생성
    @app.route('/api/v1/resources/books', methods=['GET'])
    def api_id():
        # URL의 일부로 ID가 제공되었는지 확인
        # ID가 제공되면 변수에 할당
        # ID가 제공되지 않으면 오류 메시지 표시
        if 'id' in request.args:
            id = int(request.args['id'])
        else:
            return "오류: ID 필드가 제공되지 않았습니다. ID를 지정해주세요."
    
        # 결과를 저장할 빈 리스트 생성
        results = []
    
        # 데이터를 반복하며 요청된 ID와 일치하는 결과 찾기
        # ID는 고유하지만 다른 필드는 여러 결과를 반환할 수 있음
        for book in books:
            if book['id'] == id:
                results.append(book)
    
        # Flask의 jsonify 함수를 사용하여 Python 사전의 리스트를 JSON 형식으로 변환
        return jsonify(results)
    
    app.run()
    ```

> 그렇다면 출력 다양성은 문제가 될까요?

그것은 당신이 무엇을 하려는지에 달려있습니다. 특정한 응답을 원한다면 문제가 될 수 있습니다. "지리학에 관한 3개의 질문을 생성하세요"와 같이 다양한 출력을 원한다면 문제가 되지 않습니다.

### temperature를 사용하여 출력 다양성 조절하기

우리는 출력을 더 예측 가능하고 결정적으로 제한하고 싶다고 결정했습니다. 그렇다면 어떻게 해야 할까요?

temperature는 0과 1 사이의 값으로, 0은 가장 결정적이고 1은 가장 다양한 출력을 생성합니다. 기본값은 0.7입니다. 같은 프롬프트를 temperature를 0.1로 설정하여 두 번 실행해 보겠습니다:

> "파이썬 웹 API를 생성하는 코드를 생성하세요"

    ```python
    # 필요한 라이브러리 가져오기
    import flask
    from flask import request, jsonify
    
    # Flask 앱 생성
    app = flask.Flask(__name__)
    
    # API를 위한 엔드포인트 생성
    @app.route('/api/v1/endpoint', methods=['GET'])
    def api_endpoint():
        # 요청 데이터 가져오기
        data = request.get_json()
    
        # 데이터 처리
        result = process_data(data)
    
        # 결과 반환
        return jsonify(result)
    
    # 데이터 처리 함수 정의
    def process_data(data):
        # 데이터 처리
        result = {'result': 'success'}
    
        # 결과 반환
        return result
    
    # Flask 앱 실행
    if __name__ == '__main__':
        app.run()
    
    ```

프롬프트를 다시 실행하면 다음과 같은 결과가 나옵니다:

    ```python
    # 필요한 라이브러리 가져오기
    import flask
    from flask import request, jsonify
    
    # Flask 앱 생성
    app = flask.Flask(__name__)
    
    # API를 위한 엔드포인트 생성
    @app.route('/api', methods=['GET'])
    def api_call():
        # 요청 데이터 가져오기
        data = request.get_json()
        
        # 데이터 처리
        result = process_data(data)
        
        # 결과 반환
        return jsonify(result)
    
    # 데이터 처리 함수 정의
    def process_data(data):
        # 데이터 처리
        result = data + 1
        
        # 결과 반환
        return result
    
    # Flask 앱 실행
    if __name__ == '__main__':
        app.run()
    
    ```

이 두 출력 사이에는 아주 작은 차이만 있습니다. 이번에는 반대로 temperature를 0.9로 설정해 보겠습니다:

    ```python
    # 필요한 라이브러리 가져오기
    import flask
    from flask import request, jsonify
    
    # Flask 앱 생성
    app = flask.Flask(__name__)
    
    # API를 위한 엔드포인트 생성
    @app.route('/api', methods=['GET'])
    def api_call():
        # 요청 데이터 가져오기
        data = request.args
    
        # 데이터 처리
        result = process_data(data)
    
        # 결과 반환
        return jsonify(result)
    
    # 데이터 처리 함수 정의
    def process_data(data):
        # 데이터 처리
        result = data + 1
    
        # 결과 반환
        return result
    
    # Flask 앱 실행
    if __name__ == '__main__':
        app.run()
    
    ```

그리고 temperature 값을 0.9로 설정한 두 번째 시도:

    ```python
    import flask
    from flask import request, jsonify
    
    # Flask 앱 생성
    app = flask.Flask(__name__)
    app.config['DEBUG'] = True
    
    # 테스트 데이터 생성
    books = [
        {'id': 0, 'title': 'A Fire Upon The Deep', 'author': 'Vernor Vinge', 'first_sentence': 'The coldsleep itself was dreamless.', 'year_published': '1992'},
        {'id': 1, 'title': 'The Ones Who Walk Away From Omelas', 'author': 'Ursula K. Le Guin', 'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.', 'published': '1973'},
        {'id': 2, 'title': 'Dhalgren', 'author': 'Samuel R. Delany', 'first_sentence': 'to wound the autumnal city.', 'published': '1975'}
    ]
    
    # 엔드포인트 생성
    @app.route('/', methods=['GET'])
    def home():
        return '''<h1>도서 API에 오신 것을 환영합니다!</h1>'''
    
    @app.route('/api/v1/resources/books
    
    ```

보시다시피, 결과는 매우 다양합니다.

> 참고로, 출력 다양성을 변화시키기 위해 변경할 수 있는 다른 매개변수들도 있습니다. 예를 들어, top-k, top-p, 반복 패널티, 길이 패널티, 다양성 패널티 등이 있지만 이러한 내용은 이 커리큘럼의 범위를 벗어납니다.

## 모범 사례

원하는 결과를 얻기 위해 적용할 수 있는 다양한 사례들이 있습니다. 프롬프팅을 더 많이 사용하면서 자신만의 스타일을 찾아갈 것입니다.

우리가 다룬 기술들 외에도, LLM을 프롬프팅할 때 고려해야 할 좋은 사례들이 있습니다.

다음은 고려해야 할 좋은 사례 몇 가지입니다:

- **문맥 명시하기**. 문맥은 중요합니다. 도메인, 주제 등과 같이 가능한 한 구체적으로 명시할수록 좋습니다.
- 출력 제한하기. 특정 개수의 항목이나 특정 길이를 원한다면 명시하세요.
- **무엇과 어떻게를 모두 명시하기**. 원하는 것과 원하는 방식을 모두 언급해야 합니다. 예를 들어 "3개의 파일로 나누어 제품과 고객을 위한 Python 웹 API를 생성하세요"와 같이 말이죠.
- **템플릿 사용하기**. 종종 회사의 데이터로 프롬프트를 보강하고 싶을 때가 있습니다. 이를 위해 템플릿을 사용하세요. 템플릿에는 실제 데이터로 대체할 변수가 포함될 수 있습니다.
- **맞춤법 확인하기**. LLM은 올바른 응답을 제공할 수 있지만, 맞춤법을 올바르게 사용하면 더 좋은 응답을 받을 수 있습니다.

## 과제

다음은 Flask를 사용하여 간단한 API를 구축하는 Python 코드입니다:

    ```python
    from flask import Flask, request
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello():
        name = request.args.get('name', 'World')
        return f'Hello, {name}!'
    
    if __name__ == '__main__':
        app.run()
    ```

GitHub Copilot이나 ChatGPT와 같은 AI 어시스턴트를 사용하여 "자기 개선 (self-refine)" 기법을 적용하여 코드를 개선해보세요.

## 해답

코드에 적절한 프롬프트를 추가하여 과제를 해결하려고 시도해보세요.

> [!TIP]  
> 개선 사항을 요청하는 프롬프트를 작성하는 것이 좋습니다. 개선 사항의 수를 제한하는 것도 좋은 아이디어입니다. 또한 아키텍처, 성능, 보안 등 특정한 방식으로 개선을 요청할 수도 있습니다.

[해답](../../solution.py?WT.mc_id=academic-105485-koreyst)

## 지식 확인

왜 생각의 사슬 (chain-of-thought) 프롬프팅을 사용해야 할까요? 올바른 응답 1개와 잘못된 응답 2개를 보여주세요.

1. A, 문제를 해결하는 방법을 LLM에게 가르치기 위해.
2. B, 코드에서 오류를 찾는 방법을 LLM에게 가르치기 위해.
3. C, 다양한 해결책을 생각해내기 위해 LLM에게 지시하기 위해.

A: 1, 생각의 사슬 (chain-of-thought) 는 LLM에게 문제를 해결하는 방법을 보여주는 것으로, 일련의 단계와 유사한 문제 및 그들이 어떻게 해결되었는지를 제공합니다.

## 🚀 도전과제

과제에서 자기 개선 (self-refine) 기법을 사용했습니다. 구축한 프로그램을 가져와서 적용할 개선 사항을 고려해보세요. 그리고 자기 개선 기법을 사용하여 제안된 변경 사항을 적용해보세요. 결과는 어떠셨나요, 더 좋아졌나요 아니면 더 나빠졌나요?

## 잘 하셨습니다! 계속해서 학습하세요

이 레슨을 완료한 후 [Generative AI 학습 컬렉션](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)을 확인하여 Generative AI 지식을 계속 향상시킬 수 있습니다!

[6번 레슨](../../../06-text-generation-apps/translations/ko/README.md?WT.mc_id=academic-105485-koreyst)으로 이동하여 Prompt Engineering 지식을 적용하여 텍스트 생성 앱을 구축해보세요.

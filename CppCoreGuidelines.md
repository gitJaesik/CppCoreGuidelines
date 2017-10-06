---
layout: default
---

# <a name="main"></a>C++ 핵심 가이드라인

2016년 9월 4일

편집자:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

이 문서는 초안 상태다. 아직 완벽하지 않으며, 심지어 잘못된 내용이 있을 수 있다.
오픈 소스 (코드) 프로젝트로, 0.7 버전까지 배포되었다.
이 프로젝트의 복사, 사용, 수정, 파생 성과물과 관련된 저작권은 MIT 라이선스를 따른다.
이 프로젝트에 기여하려면 기여자 라이선스(Contributor License)에 동의해야 한다.
자세한 내용은 첨부된 [라이선스 파일(LICENSE)](LICENSE)을 참조하길 바란다.
오픈 소스 프로젝트에 익숙한 사용자들이 사용, 복사, 수정, 파생 성과물들을 통해 좋은 정보를 제공해주기 바란다.

개선 사항에 대한 의견과 제안은 대부분 환영한다.
C++ 언어에 새로운 기능들이 추가되고 사용 가능한 라이브러리들이 많아지므로 이 문서를 지속적으로 수정하고 확장해 규칙들에 대한 이해를 돕고자 한다.
의견을 보내고 싶다면, [소개](#S-introduction)에 있는 목적과 방법을 참고하기 바란다.
기여자 목록은 [여기](#SS-ack)를 참고하기 바란다.

참고 사항:

* 규칙들은 아직 완성도, 일관성, 시행 가능성 등에 대해서 철저하게 검증되지 않았다.
* (???)로 표시되어 있는 부분은 아직 관련 정보를 기입하지 않았음을 의미한다.
* 참조 절에 있는 예제들을 갱신해야 한다. C++11 이전의 오래된 소스 코드들이 아직 많이 있다.
* 앞으로 해야 할 일에 대한 최신 목록은 [To-do: 미분류 규칙 초안](#S-unclassified)에서 확인할 수 있다.

[이 가이드의 범위와 구조](#S-abstract)를 확인하거나, 목차에서 읽고 싶은 부분을 클릭해 해당 내용으로 바로 이동할 수 있다.

* [In(Introduction): 소개](#S-introduction)
* [P(Philosophy): 철학](#S-philosophy)
* [I(Interfaces): 인터페이스](#S-interfaces)
* [F(Functions): 함수](#S-functions)
* [C(Classes and class hierarchies): 클래스와 클래스 계층](#S-class)
* [Enum(Enumerations): 열거형](#S-enum)
* [R(Resource management): 리소스 관리](#S-resource)
* [ES(Expressions and statements): 표현식과 문장](#S-expr)
* [Per(Performance): 성능](#S-performance)
* [CP(Concurrency): 동시성](#S-concurrency)
* [E(Error handling): 오류 처리](#S-errors)
* [Con(Constants and immutability): 상수와 불변성](#S-const)
* [T(Templates and generic programming): 템플릿과 제너릭 프로그래밍](#S-templates)
* [CPL(C-style programming): C 스타일 프로그래밍](#S-cpl)
* [SF(Source files): 소스 파일](#S-source)
* [SL(The Standard library): 표준 라이브러리](#S-stdlib)

참고할 만한 내용:

* [A(Architectural Ideas): 구조적 아이디어](#S-A)
* [NR(Non-Rules and myths): 규칙이 아닌 미신](#S-not)
* [RF(References): 참고 문헌](#S-references)
* [Pro(Profiles): 프로파일](#S-profile)
* [GSL(Guideline support library): 가이드라인 지원 라이브러리](#S-gsl)
* [NL(Naming and layout): 이름 명명 규칙과 레이아웃](#S-naming)
* [FAQ(Answers to frequently asked questions): 자주 묻는 질문에 대한 대답](#S-faq)
* [부록 A: 라이브러리](#S-libraries)
* [부록 B: 모던 C++ 스타일로 코딩하기](#S-modernizing)
* [부록 C: 토론](#S-discussion)
* [용어 설명](#S-glossary)
* [To-do(Unclassified proto-rules): 미분류 규칙](#S-unclassified)

구체적인 C++ 기능은 다음을 참고하기 바란다.

* [대입](#S-???)
* [`class`](#S-class)
* [생성자](#SS-ctor)
* [상속(파생)된 `class`](#SS-hier)
* [소멸자](#SS-dtor)
* [예외](#S-errors)
* [`for`](#S-???)
* [`inline`](#S-class)
* [초기화](#S-???)
* [람다 표현식](#SS-lambdas)
* [연산자](#S-???)
* [`public`, `private`, `protected`](#S-???)
* [`static_assert`](#S-???)
* [`struct`](#S-class)
* [`template`](#S-???)
* [`unsigned`](#S-???)
* [`virtual`](#SS-hier)

다음에 나오는 용어들의 정의는 규칙을 설명하기 위한 목적으로 사용되며, 특정 언어에 따른 기술적 용어들이 아니다.
하지만 디자인과 프로그래밍 테크닉에사 자주 등장하는 용어들이다.

* 오류(Error)
* 예외(Exception)
* 살패(Failure)
* 불변(Invariant)
* 누수(Leak)
* 사전조건(Precondition)
* 사후조건(Postcondition)
* 리소스(Resource)
* 예외 보증(Exception Guarantee)

# <a name="S-abstract"></a>개요

이 문서는 C++를 올바로 사용하기 위한 가이드라인의 모음이며,
개발자들이 모던 C++를 좀 더 효율적으로 사용할 수 있도록 돕기 위해 작성되었다.
"모던 C++"이란 C++11, C++14 (그리고 곧 나올 C++17)을 뜻한다.
지금 개발중인 코드가 5년, 또는 10년 후에 어떤 모습이었으면 좋겠는가?

C++ 핵심 가이드라인은 인터페이스, 리소스 관리, 메모리 관리, 동시성와 같이 상대적으로 고수준의 내용을 다루고 있으며,
이러한 내용들은 대체로 애플리케이션 아키텍쳐나 라이브러리 디자인에 영향을 미친다.
가이드라인에 나오는 규칙들을 준수하면 코드가 정적 타입 안정성을 갖도록 할 수 있으며, 리소스의 누수가 없고, 
흔히 저지르는 수많은 프로그래밍 로직 오류를 잡아낼 수 있으며, 더욱 빠르게 수행되는 코드를 작성할 수 있을 것이다.
즉, 올바른 코드를 만들 수 있다.

이름 지정 규칙이나 들여쓰기 스타일과 같이 세부적인 내용에 대해서는 크게 언급하지 않을 것이다.
하지만, 프로그래머에게 도움을 줄 수 있는 주제가 있다면 다루도록 하겠다. 

초기의 규칙들은 (다양한 형태의) 안정성과 간결함을 과도하게 강조한 측면이 있다.
실용성을 위해 여러 예외 사항들이 도입되기를 바란다. 또한 더 많은 규칙들이 필요하다.

규칙들 중에는 기대했던 것과 다르거나 기존에 경험했던 것과 전혀 다른 부분이 있을 것이다.
이렇든 저렇든 여러분의 코딩 스타일에 변화를 주지 못한다면, 우리는 실패한 것이다!
올바른 규칙인지 확인해 보고, 문제가 있다면 반박해주길 바란다!
특히, 측정 결과나 더 나은 예제들로 규칙들을 뒷받침해주길 바란다.

명백하거나 당연해 보이는 규칙도 일부 있을 것이다.
하지만 이 가이드라인은 숙련되지 않은 개발자들 또는 다른 배경 지식을 갖거나 다른 언어를 알고 있는 개발자들이 C++ 언어를 빨리 익힐 수 있도록 돕기 위한 문서임을 기억해 주었으면 한다.

우리는 분석 툴을 통해 지원할 수 있도록 대부분의 규칙들을 설계했다.
분석 툴을 이용하면 참조(또는 링크)를 통해 어떤 규칙을 위반했는지 파악할 수 있다.
모든 규칙을 알고 코드를 작성할 수는 없는 노릇이다.
이러한 가이드라인은 무슨 문제가 발생했는지 파악할 수 있게 해주는 툴에 대한 명세라고 볼 수 있다.

우리는 규칙들이 코드에 점진적으로 도입되길 바라며, 이를 위한 툴을 만들 계획을 갖고 있다.

규칙을 개선하기 위한 의견과 제안은 언제든 환영한다. 
C++ 언어에 새로운 기능들이 추가되고 사용 가능한 라이브러리들이 많아지므로 이 문서를 지속적으로 수정하고 확장해 규칙들에 대한 이해를 돕고자 한다.

# <a name="S-introduction"></a>In(Introduction): 소개

이 문서는 모던 C++, C++14는 물론이고 추후 개션될 내용과 ISO 기술 명세(Technical Specification; TS)까지 고려한 핵심 가이드라인이다.
이 문서의 목표는 C++ 프로그래머가 더욱 간단하고, 효과적이며, 유지 보수하기 좋은 코드를 작성하는데 도움을 주는 것이다.

소개 요약:

* [In.target(Target readership): 대상 독자](#SS-readers)
* [In.aims(Aims): 목표인 부분](#SS-aims)
* [In.not(Non-aims): 목표가 아닌 부분](#SS-non)
* [In.force(Enforcement): 적용](#SS-force)
* [In.struct(The structure of this document): 이 문서의 구조](#SS-struct)
* [In.sec(Major sections): 주요 목차](#SS-sec)

## <a name="SS-readers"></a>In.target(Target readership): 대상 독자

모든 C++ 프로그래머. 또한 [C를 고려하는 프로그래머](#S-cpl).

## <a name="SS-aims"></a>In.aims(Aims): 목표인 부분

이 문서는 개발자들이 모던 C++(C++11, C++14, 이후 C++17까지)을 익히는데 도움을 주고, 좀 더 일관된 스타일로 코드를 작성할 수 있게 해주려는 목적으로 작성되었다.

이 문서에서 다루고 있는 규칙들이 모든 코드에 효과적으로 적용할 수 있다고 생각하지는 않는다.
오래된 시스템을 새롭게 업그레이드하는 것은 쉽지 않은 일이다. 하지만 여기서 다루는 규칙들을 적용한다면 기존의 방식보다 오류가 발생할 가능성이 낮고, 유지 보수가 편리한 코드를 작성할 수 있을 것이라 확신한다.
또한 이러한 규칙들은 개발 초기에 좀 더 빠르고 쉽게 개발을 진행하는 데도 도움이 될 것이다.
분명히 말할 수 있는 것은, 이러한 규칙들을 적용하면 제로-비용 원칙(Zero-overhead principle)에 입각해 이전보다 더 나은 동작을 수행되는 코드를 만들 수 있다는 점이다. 
(제로-비용 원칙이란 "사용하지 않는 부분에 비용을 낭비하지 마라.", 또는 "올바른 추상화 메커니즘을 사용했을 경우, 적어도 저수준 언어로 하드코딩한 것 만큼의 성능을 얻을 수 있다." 정도로 설명할 수 있다.)
새로운 코드를 작성하거나 기존 코드를 개선할 여지가 생겼다면, 적용 가능한 수준에서 규칙들을 적용해 보기 바란다.

### <a name="R0"></a>In.0(Don't panic!): 당황하지 마라! 

여러분의 프로그램에 가이드라인에 나온 규칙을 적용한다면, 어떤 영향을 줄 지 충분히 고려해야 한다. 

가이드라인은 "상위 집합의 하위 집합(Subset of superset)" ([Stroustrup05](#Stroustrup05)) 원리에 따라 구성되어 있다.
우리는 신뢰성, 안정성, 성능 등을 고려해 단순히 C++의 한 부분 집합을 정의하지 않는다.
대신, 몇 가지 간단한 "확장" ([라이브러리 구성 요소](#S-gsl))을 사용하길 강력히 권고한다.
이를 통해 오류가 발생하기 쉬운 C++ 기능들의 대부분을 제거할 수 있다.

가이드라인에 포함된 규칙들은 정적 타입 안정성과 리소스 안정성에 특히 주안점을 두고 있다.
이러한 이유로 범위 확인 가능성, `nullptr`를 통한 역참조 회피 가능성, 댕글링 포인터(Dangling pointer) 회피 가능성, (RAII를 통한) 시스템적인 예외 사용 등을 강조한다.
잘 알려져 있지 않은 소스 코드의 오류 발생 가능성을 부분적으로나마 극복하고 최소화 하는 방법, 좀 더 단순한 표현 방식뿐 아니라 올바르게 정의된 인터페이스를 통해 복잡도를 드러내지 않는 방법 등을 강조한다.

많은 규칙들은 규범적인 성격을 띄고 있다.
대안도 없이 그저 "그렇게 하지 마라!"라고만 하는 규칙들은 불편할 수 밖에 없다.
일부 규칙들은 기계적으로 정밀 검증할 수 있다기 보다는 경험에 근거하여 작성되었다. 
다른 부류의 규칙들은 일반적인 원칙을 논리적으로 정리한 것이며, 널리 사용될 것이라 생각되는 규칙들은 더욱 자세하게, 그리고 부분적으로나마 검증이 가능하도록 구체적으로 설명했다.

또한 이 가이드라인은 C++에서 핵심이 되는 내용과 그 사용법을 다루고 있기도 하다. 
조직의 규모가 매우 크거나, 특화된 분야의 애플리케이션을 개발하거나, 프로젝트의 규모가 매우 크다면 여기서 다루는 내용보다 더 다양한 규칙과 제약, 라이브러리가 필요할 것이다.
예를 들어 고도의 실시간 애플리케이션을 개발하는 경우라면 자유 저장소(Free Store, 동적 메모리)를 아무렇게나 사용하면 안되기 때문에, 라이브러리를 선택하는데 제약이 있을 수 밖에 없다.
이처럼 특화된 개발 분야에만 적용 할 수 있는 규칙들은 핵심 가이드라인의 부록에 담았다.
어셈블리 코드와 같이 저수준의 프로그래밍 방식을 고수하기 보다는 핵심 기능을 구현하고 있는 소규모의 라이브러리를 만들고 사용하기 바란다.

규칙들은 [점진적으로 적용](#S-modernizing)해 볼 수 있다.

일부 규칙들은 안정성을 높이기 위해 다양한 방법들을 설명하고 있으며, 또 다른 규칙들은 문제 발생 가능성을 낮추는 방법을 설명하고 있다. 혹은 이 둘을 모두 고려해 만들어진 규칙들도 있다.
규칙을 통해 전달하고자 하는 내용을 기술할 때 통상 오류 발생 가능성이 높은 경우와 그렇지 않은 경우가 있다면, 가능한 오류 발생 가능성이 낮은 방법을 택했다.

## <a name="SS-non"></a>In.not(Non-aims): 목표가 아닌 부분

최소한의 규칙만을 정의하고, 그 규칙들이 정밀하게 들어 맞도록 가이드라인을 조직적으로 작성한 것은 아니다.
실제로 범용적인 규칙들 중에는 간단해 보이지만 실제로 적용하기가 매우 어려운 것들도 있으며, 그러한 규칙들이 지닌 함축적인 의미를 이해하기 어려울 수도 있다.
구체적인 규칙들이 좀 더 이해 및 적용하기 쉽지만, 범용적인 규칙들을 다루지 않고는 특별한 경우만을 장황하게 나열할 수 밖에 없었을 것이다.
각각의 규칙들은 초보 개발자 뿐만 아니라 전문가들에게도 도움이 될 수 있도록 작성했다.
일부 규칙은 반드시 적용되어야 하지만, 필요에 따라 적용 여부를 선택적으로 결정해야 하는 경우도 있다.

여기서 다루는 규칙들을 책에서 다루는 것인냥 너무 심각하게 받아들을 필요는 없다. 링크를 통해서 규칙들을 가볍게 살펴보는 것도 괜찮다.
사실 이처럼 규칙을 엄밀하게 정의한 이유는 규칙들을 위배한 코드를 찾고, 그에 대한 링크 정보를 보여주는 툴을 만들기 위해서이다.
즉, 규칙이 만들어진 이유를 설명하고, 만약 규칙을 따르지 않았을 때 어떤 문제가 발생할 수 있고, 그 문제를 어떻게 해결할 수 있는지를 알려주기 위해서다.

이 가이드라인은 C++ 튜토리얼을 대체할 용도로 작성된 것이 아니다.
개발자의 수준에 부합하는 튜토리얼이 필요하다면, [참고 문헌](#S-references)을 참조하기 바란다.

이 문서는 기존의 C++ 코드를 모던 C++ 코드로 변환하는 방법에 대해서 다루고 있는 것도 아니다.
다만, 새로운 C++ 코드에 대한 논리 정연한 생각들을 구체적으로 설명한다.
따라서 기존 코드를 모던하게, 젊고 활기차게, 업그레이드 하고 싶다면 [모던화](#S-modernizing)를 참조하기 바란다.
중요한 것은 이 문서에서 다루고 있는 규칙들을 점진적으로 적용할 수 있다는 점이다. 엄청난 양의 코드를 단번에 바꿀 수는 없는 노릇이다.

이 가이드라인이 언어의 기술 세부 사항을 완벽하게, 또는 정확하게 설명하지는 않는다. 
언어의 명세, 일반 규칙에 대한 예외 사항, 그 외 세부 기능들에 대해서는 ISO C++ 표준을 참조하기 바란다.

규칙들을 작성할 때 C++의 일부 기능만을 이용해 코드를 작성하도록 의도하지는 않았다.
마치 C++의 일부만을 떼어놓은 Java를 사용하는 것처럼 강제하기 위함은 *절대로* 아니라는 것이다.
"단 하나의 올바른 C++" 언어라는 식의 정의 또한 의도하는 바가 아니다.
다만 가이드라인에 포함된 규칙을 통해서 C++라는 언어가 성능과 타협하지 않으면서도 풍부한 표현력을 지닌 언어라는 점을 강조하고자 했다.

세부 규칙들을 통해서 전달하려는 가치는 명확하다.
기존의 C++ 코드보다 더욱 간단하고, 올바르고, 안전한 코드를 성능을 손해보지 않고 작성할 수 있도록 돕는 것이다.
또한 유효한 C++ 코드이긴 하지만 오류를 유발할 가능성이 높고, 불필요하게 복잡하고, 성능도 좋지 않은 코드를 피할 수 있도록 돕는 것이다.

The rules are not perfect.
A rule can do harm by prohibiting something that is useful in a given situation.
A rule can do harm by failing to prohibit something that enables a serious error in a given situation.
A rule can do a lot of harm by being vague, ambiguous, unenforceable, or by enabling every solution to a problem.
It is impossible to completely meet the "do no harm" criteria.
Instead, our aim is the less ambitious: "Do the most good for most programmers";
if you cannot live with a rule, object to it, ignore it, but don't water it down until it becomes meaningless.
Also, suggest an improvement.

## <a name="SS-force"></a>In.force(Enforcement): 적용

사실상 다양한 규칙들을 적용하도록 강제하지 않고서는 이러한 규칙들이 방대한 코드에 모두 적용되리라고 기대하기는 어렵다.
실상, 모든 규칙을 강제적으로 적용하는 것은 규칙의 수가 몇 개 되지 않거나 특수한 사용자 집단에서나 가능한 일일지도 모르겠다.

* 그러나 사람들은 모든 사람들이 사용할 수 있는 다양한 규칙들을 원한다.
* 그러나 사람들은 서로 다른 규칙들을 갖는다.
* 그러나 사람들은 방대한 규칙을 모두 읽고 싶어하지 않는다.
* 그러나 사람들은 수많은 규칙들을 기억할 수 없다.

이러한 이유로 다양한 요구 사항들의 공통적인 부분만을 뽑아내려고 해봤다.

* 그러나 임의의 규칙을 정하는 것 조차 혼돈을 초래할 것으로 생각한다.

우리는 더 많은 개발자들에게 도움이 되고, 코드를 좀 더 간결하게 만들 수 있으며, 기존 코드를 모던화 할 수 있는 가이드라인을 만들고 싶었다.
개인의 선택의 문제라거나 관리의 압박으로 인해 신경쓰지 않았던 부분들도 도외시하지 않고 최선의 실용적인 예를 다루고자 했다.
바라건대 모든 규칙들을 적용하는 것이 좋다고 생각한다. 그렇게 해야만 최고의 이득을 본다고 생각하기 때문이다.

이는 꽤나 심각한 딜레마가 아닐 수 없는데, 우리는 이러한 딜레마의 해결책이 툴을 개발하는 것이라고 생각했다.
각각의 규칙들은 적용 방법을 설명하고 있는 **적용** 단락을 갖고 있는데, 
코드 리뷰, 정적 분석, 컴파일러, 런타임 체크 등의 방법을 나열하고 있다.
어떤 방식이든 우리는 "기계적"이며(사람은 느리기도 하고, 쉽게 지루해 할 수 있으므로) 일관된 방법으로 개별 규칙들이 적용되기를 원했다.
이런 이유로 런타임 체크는 다른 대안이 없을 경우에 한해서만 언급했다.
이 같은 내용들을 여기저기에 흩어놓기 보다는 사용자가 원할 경우 쉽게 찾을 수 있도록 하고 싶었기에,
(**적용** 단락 내에) 적절한 위치라고 생각되는 곳에 연관된 규칙들을 "프로필"이라는 이름으로 나열해 두었다.
하나의 규칙은 여러 프로필에 속할 수 있으며, 어떤 프로필에도 속하지 않은 규칙들도 있다.
자주 사용되는 프로필 몇 가지를 먼저 살펴보자.

* **타입**: (캐스팅, 공용체, 가변 인수를 통해 `T`를 `U`로 재해석 하는) 타입 위반 없음
* **한계**: (배열의 범위를 넘어서 접근하는) 범위 위반 없음
* **수명**: (`delete`나 `delete[]`이 실패하는) 누수 없음, (`nullptr`를 역참조하고 댕글링된 참조를 사용해) 유효하지 않은 개체에 접근하지 않음

프로필은 툴에서 활용할 용도로 만들었지만, 내용을 읽어보면 많은 도움이 된다.
**적용** 단락을 단순히 우리가 알고 있는 적용 방법을 제시하는 것으로만 제한하고 싶지 않다.
단락 중 일부는 툴 개발자에게 도움이 될만한 내용일 것이다.

Tools that implement these rules shall respect the following syntax to explicitly suppress a rule:

    [[suppress(tag)]]

where "tag" is the anchor name of the item where the Enforcement rule appears (e.g., for [C.134](#Rh-public) it is "Rh-public"), the
name of a profile group-of-rules ("type", "bounds", or "lifetime"),
or a specific rule in a profile ([type.4](#Pro-type-cstylecast), or [bounds.2](#Pro-bounds-arrayindex)).

## <a name="SS-struct"></a>In.struct(The structure of this document): 이 문서의 구조

각 규칙(가이드라인, 제안)은 여러 부분으로 나뉜다.

* 규칙 그 자체 -- 예) **`new`를 무방비 상태로 사용하지 마라**
* 규칙 참조 번호 -- 예), **C.7** (클래스와 관련된 7번째 규칙).
  주요 단원이 순서대로 나열되지 않았기 때문에 규칙 참조 "번호"의 첫 번째 부분은 문자로 시작한다.
  규칙을 추가하거나 삭제할 때 혼란을 최소화하기 위해 숫자를 매길때 간격을 둔다.
* **이유**들 (근거) -- 이해할 수 없는 규칙을 따르고 싶지는 않을 것이므로 구체적인 이유를 설명한다.
* **예제**들 -- 추상적인 표현만으로 내용을 이해하기 어려울 경우 구체적인 예(좋은 예나 나쁜 예)를 기술한다.
* **대안**들 -- "이런 식으로 하지 마라"에 대한 대안을 제시한다.
* **예외**들 -- 단순하고 보편적인 규칙이 좋다. 많은 규칙들이 널리 사용되지만 만능은 아니므로 예외가 있을 경우 이를 나열한다.
* **적용** -- "기계적으로" 규칙을 확인하는 방법에 대한 아이디어를 설명한다.
* **참고 항목**들 -- (이 문서나 다른 문서에 대해) 규칙이나 관련 내용과 연관된 항목들을 안내한다.
* **비고**들 (언급) -- 다른데서 다루기 적합하지 않은 부분에 대해 추가로 설명한다.
* **논의** -- 규칙을 제시한 근본적인 이유를 담고 있는 다른 글에 대한 참조나 규칙의 주요 리스트에 포함되지 않는 예 등을 설명한다.

일부 규칙들은 기계적으로 확인하기에 어려울 수 있으나 전문적인 프로그래머라면 손쉽게 위반 여부를 발견할 수 있다.
We hope that "mechanical" tools will improve with time to approximate what <such></such> an expert programmer notices.
Also, we assume that the rules will be refined over time to make them more precise and checkable.

각 규칙에 적용 가능한 모든 대안과 특별한 예외사항까지 모두 언급하기를 바라지는 않는다. 가능한 단순하게 유지되길 바란다.
이러한 정보는 **대안** 단락과 [토론](#S-discussion) 절에서 찾을 수 있다.
규칙을 이해할 수 없거나 동의하지 않는다면, **논의**를 살펴보기 바란다.
또한, 논의가 없거나 불완전하다고 생각된다면 [이슈](https://github.com/isocpp/CppCoreGuidelines/issues)에 여러분이 염려하는 부분과 가능하다면 연관된 PR에 대한 설명을 적어주기 바란다.

이 문서는 언어어 대한 매뉴얼이 아니다.
따라서 기술적인 세부 사항을 자세히 다루기보다는 기존에 작성된 코드에 대한 가이드로써의 역할을 했으면 한다.
도움이 되는 정보의 출처는 [참고 문헌](#S-references)에서 찾을 수 있다.

## <a name="SS-sec"></a>In.sec(Major sections): 주요 목차

* [In(Introduction): 소개](#S-introduction)
* [P(Philosophy): 철학](#S-philosophy)
* [I(Interfaces): 인터페이스](#S-interfaces)
* [F(Functions): 함수](#S-functions)
* [C(Classes and class hierarchies): 클래스와 클래스 계층](#S-class)
* [Enum(Enumerations): 열거형](#S-enum)
* [R(Resource management): 리소스 관리](#S-resource)
* [ES(Expressions and statements): 표현식과 문장](#S-expr)
* [E(Error handling): 오류 처리](#S-errors)
* [Con(Constants and immutability): 상수와 불변성](#S-const)
* [T(Templates and generic programming): 템플릿과 제너릭 프로그래밍](#S-templates)
* [CP(Concurrency): 동시성](#S-concurrency)
* [SL(The Standard library): 표준 라이브러리](#S-stdlib)
* [SF(Source files): 소스 파일](#S-source)
* [CPL(C-style programming): C 스타일 프로그래밍](#S-cpl)
* [Pro(Profiles): 프로파일](#S-profile)
* [GSL(Guideline support library): 가이드라인 지원 라이브러리](#S-gsl)
* [FAQ(Answers to frequently asked questions): 자주 묻는 질문에 대한 대답](#S-faq)

참고할 만한 내용:

* [NL(Naming and layout): 이름 명명 규칙과 레이아웃](#S-naming)
* [Per(Performance): 성능](#S-performance)
* [NR(Non-Rules and myths): 규칙이 아닌 미신](#S-not)
* [RF(References): 참고 문헌](#S-references)
* [부록 A: 라이브러리](#S-libraries)
* [부록 B: 모던 C++ 스타일로 코딩하기](#S-modernizing)
* [부록 C: 토론](#S-discussion)
* [용어 설명](#S-glossary)
* [To-do(Unclassified proto-rules): 미분류 규칙](#S-unclassified)

각 목차에 있는 내용은 서로 연관되어 있다.

각 목차("P"는 "철학(Philosophy)")와 부 목차("C.hier"는 "클래스 계층(Class Hierarchies) (OOP)")은 검색, 참조의 편의를 위해 약어로 표기한다.
주요 목차에 대한 약어로 규칙 번호를 사용하기도 한다. (즉, "각 타입을 일반적인 타입으로 만들어라."라는 규칙을 "C.11"로 나타내기도 한다.)

# <a name="S-philosophy"></a>P(Philosophy): 철학

이 장의 규칙은 매우 일반적이다.

철학 규칙 요약:

* [P.1: 아이디어를 직접 코드로 표현하라](#Rp-direct)
* [P.2: ISO 표준 C++로 작성하라](#Rp-Cplusplus)
* [P.3: 의도를 표현하라](#Rp-what)
* [P.4: 이상적으로 프로그램은 정적으로 타입 안전해야 한다](#Rp-typesafe)
* [P.5: 런타임 검사보다는 컴파일 타임 검사를 선호하라](#Rp-compile-time)
* [P.6: 컴파일 타임에 검사할 수 없다면 런타임에 검사할 수 있어야 한다](#Rp-run-time)
* [P.7: 런타임 오류는 초기에 잡아라](#Rp-early)
* [P.8: 리소스가 새도록 하지 마라](#Rp-leak)
* [P.9: 시간이나 공간을 낭비하지 마라](#Rp-waste)
* [P.10: Prefer immutable data to mutable data](#Rp-mutable)
* [P.11: Encapsulate messy constructs, rather than spreading through the code](#Rp-library)

철학적 규칙은 보통 기계적으로 검사할 수 없다.
그러나 철학적인 테마를 반영하는 개별적인 규칙은 검사할 수 하다.
철학적인 기초가 없이 구체적이고/특수하고/검사 가능한 규칙은 근거가 부족하다.

### <a name="Rp-direct"></a>P.1: 아이디어를 직접 코드로 표현하라

##### 이유

컴파일러는 주석문(또는 디자인 문서)을 읽지 않는다. 수많은 프로그래머 또한 주석을 (일관되게) 읽지 않는다.
코드로 표현된 내용이라면 그 의미(의도)를 이미 정의했을 것이며 (대체로) 컴파일러나 다른 툴로 검사할 수 있다.

##### 예제

    class Date {
        // ...
    public:
        Month month() const;  // do
        int month();          // don't
        // ...
    };

첫번째 `month` 함수는 명확히 `Month`를 반환하도록 선언되어 있으며, `Date` 개체의 상태를 변경하지 않을 것처럼 보인다. 
두번째 버전은 코드를 읽는 개발자들을 고민하게 만들며, 발견하기 어려운 버그를 유발할 가능성이 있다.

##### 예제

    void f(vector<string>& v)
    {
        string val;
        cin >> val;
        // ...
        int index = -1;                    // bad
        for (int i = 0; i < v.size(); ++i)
            if (v[i] == val) {
                index = i;
                break;
            }
        // ...
    }

위 반복문은 `std::find`를 이용해 표현 가능하다. 
의도를 더 명확하게 드러내기 위해 아래와 같이 코드를 바꿀 수 있다.

    void f(vector<string>& v)
    {
        string val;
        cin >> val;
        // ...
        auto p = find(begin(v), end(v), val);  // better
        // ...
    }

언어가 제공하는 기능을 직접 사용하기보다 잘 설계된 라이브러리를 사용해 그 의도(어떻게 수행되는지보다 무엇이 수행되는지를) 표현하는 것이 훨씬 낫다.

C++ 프로그래머는 표준 라이브러리의 기본 내용을 반드시 이해하고 올바른 곳에 사용해야 한다.
어떤 프로그래머든 프로젝트에 기반하고 있는 핵심 라이브러리의 기본 내용을 반드시 이해하고 있어야 하며, 올바르게 사용할 줄 알아야 한다.
이 가이드라인을 사용하는 프로그래머는 [가이드라인 지원 라이브러리](#S-gsl)을 반드시 알아야 하고 적절히 사용할 줄 알아야 한다.

##### 예제

    change_speed(double s);   // bad: what does s signify?
    // ...
    change_speed(2.3);

더 좋은 접근법은 `double`(새 속도, 또는 이전 속도와의 차이)의 의미와 단위를 명확히 하는 것이다.

    change_speed(Speed s);    // better: the meaning of s is specified
    // ...
    change_speed(2.3);        // error: no unit
    change_speed(23m / 10s);  // meters per second

(단위가 없는) 단순한 `double`을 변화량(속도의 차)으로 받아들일 수도 있겠지만, 아무래도 오류가 발생하기 쉽다.
속도와 변화량 둘 다 필요하다면, `Delta` 타입을 정의해야 할 것이다.

##### 적용

일반적으로 매우 어렵다.

* 일관성 있게 `const`를 사용하라. (멤버 함수가 개체를 변경하는지 확인하라. 그리고 포인터나 레퍼런스로 넘어온 인자를 변경하는지 확인하라.)
* 타입 변환의 사용을 표시하라. (타입 변환은 타입 시스템을 무력화시킨다.)
* 표준 라이브러리를 흉내내는 코드를 찾아라. (하지만 찾기 어렵다.)

### <a name="Rp-Cplusplus"></a>P.2: ISO 표준 C++로 작성하라

##### 이유

이 문서는 ISO 표준 C++을 만드는 가이드라인들을 모아둔 것이다.

##### 비고

시스템 리소스에 접근하는 등의 작업을 수행하기 위해서 확장 기능이 필요할 수 있다.
이런 경우에는 필요한 확장 기능을 지역적으로 제한해서 사용하고, 비핵심 코딩 가이드라인을 활용해 관리하라.
If possible, build interfaces that encapsulate the extensions so they can be turned off or compiled away on systems that do not support those extensions.

Extensions often do not have rigorously defined semantics.  Even extensions that
are common and implemented by multiple compilers may have slightly different
behaviors and edge case behavior as a direct result of *not* having a rigorous
standard definition.  With sufficient use of any such extension, expected
portability will be impacted.

##### 비고

Using valid ISO C++ does not guarantee portability (let alone correctness).
Avoid dependence on undefined behavior (e.g., [undefined order of evaluation](#Res-order))
and be aware of constructs with implementation defined meaning (e.g., `sizeof(int)`).

##### 비고

표준 C++ 언어의 기능이나 라이브러리조차 제한적으로 사용할 수 밖에 없는 환경도 있다.
예를 들면, 항공기 제어 소프트웨어 개발 표준에는 동적 메모리 할당을 피할 것을 주문하고 있다. 
이런 경우에는 특정 환경에 맞춘 코딩 가이드라인을 확장해서 사용하거나 사용하지 않아야 하는 기능들을 관리하라.

##### 적용

확장을 허용하지 않도록 기능 설정이 가능한 최신 C++ 컴파일러(현재 C++11이나 C++14)를 사용하라.

### <a name="Rp-what"></a>P.3: 의도를 표현하라

##### 이유

(이름이나 주석을 통해) 코드의 의도를 제대로 드러내지 못한다면, 코드가 제대로 수행되는지조차 말할 수 없을 것이다.

##### 예제

    int i = 0;
    while (i < v.size()) {
        // ... do something with v[i] ...
    }

위 코드만 보았을 때는 `v`의 각 요소를 순회하겠다는 의도가 드러나지 않는다.
인덱스에 대한 세부적인 구현부가 노출된다 (따라서 잘못 사용될 지도 모른다). 그리고 의도적인지는 알 수 없지만 `i`를 반복문 밖에서도 여전히 사용할 수 있다. 이 부분의 코드만 읽어서는 그 의도를 알지 못한다.

개선:

    for (const auto& x : v) { /* do something with x */ }

위 코드를 보면 v에 대한 순회 메커니즘을 명시적으로 언급하지 않는다. 그리고 순회하는 동안 v의 각 `const` 요소의 레퍼런스에 대해 동작이 일어나기 때문에 각 요소를 수정할 수 없다. 만약 수정이 필요한 경우라면 아래와 같이 코드를 작성해야 한다.

    for (auto& x : v) { /* do something with x */ }

이전보다 개선되었지만, 이보다는 잘 알려진 알고리즘을 사용하라.

    for_each(v, [](int x) { /* do something with x */ });
    for_each(par, v, [](int x) { /* do something with x */ });

마지막 예는 `v`의 각 요소가 처리되는 순서에 관심이 없다는 점을 명확히 하고 있다.

프로그래머라면 다음에 익숙해져야 한다.

* [가이드라인 지원 라이브러리](#S-gsl)
* [ISO C++ 표준 라이브러리](#S-stdlib)
* 현재 프로젝트에서 사용되고 있는 모든 기본 라이브러리들

##### 비고

공식 대안: 어떻게 작업이 수행되는지를 말하지 말고 무엇이 수행될지를 말하라.

##### 비고

언어의 기본 요소는 다른 무엇보다도 그 의도를 더 잘 표현한다.

##### 예제

2개의 `int` 값으로 2차원 좌표를 표현하고 싶다면, 다음과 같이 써라.

    draw_line(int, int, int, int);  // obscure
    draw_line(Point, Point);        // clearer

##### 적용

좀 더 나은 대안이 있는 범용적인 패턴을 찾아보라.

* 단순한 `for`문 대 범위 기반 `for`문
* `f(T*, int)` 인터페이스 대 `f(span<T>)` 인터페이스
* 아주 큰 스코프에서 순회하는 변수
* 처리되지 않은 `new`와 `delete`
* 인자로 내장 타입을 여러개 갖는 함수

There is a huge scope for cleverness and semi-automated program transformation.

### <a name="Rp-typesafe"></a>P.4: 이상적으로 프로그램은 정적으로 타입 안전해야 한다

##### 이유

이상적으로 프로그램은 완전히 정적으로 타입 안전해야 한다.
하지만 불행하게도 불가능하다. 왜냐하면 다음처럼 문제가 되는 영역들이 존재하기 때문이다.

* 공용체
* 타입 변환
* 배열 붕괴
* 범위 오류
* 축소 타입 변환

##### 비고

이러한 영역들은 심각한 문제의 원인이 된다. (예를 들어, 크래시와 보안 위반)
따라서 다른 기법을 제공하고자 한다.

##### 적용

각 프로그램에 대해 필요하고 실현 가능하도록 각 문제 범주를 개별적으로 금지, 억제 또는 탐지할 수 있다.
항상 대안을 제시하라.
예를 들어,

* 공용체 -- (C++17에 있는) `variant`를 사용하라.
* 타입 변환 -- 사용을 최소화하라. 템플릿이 도움이 될 수 있다.
* 배열 붕괴 -- (GSL에 있는) `span`을 사용하라.
* 범위 오류 -- `span`을 사용하라.
* 축소 타입 변환 -- 사용을 최소화하라. 필요하면 `narrow`나 (GSL에 있는) `narrow_cast`를 사용하라.

### <a name="Rp-compile-time"></a>P.5: 런타임 검사보다는 컴파일 타임 검사를 선호하라

##### 이유

코드 명확성, 성능 향상.
컴파일 타임에 발견되는 오류에 대해서는 처리하는 부분을 따로 작성할 필요가 없다.

##### 예제

    // Int is an alias used for integers
    int bits = 0;         // don't: avoidable code
    for (Int i = 1; i; i <<= 1)
        ++bits;
    if (bits < 32)
        cerr << "Int too small\n"

이 예제는 다음과 같이 간단하게 만들 수 있다.

    // Int is an alias used for integers
    static_assert(sizeof(Int) >= 4);    // do: compile-time check

##### 예제

    void read(int* p, int n);   // read max n integers into *p

    int a[100];
    read(a, 1000);    // bad

개선:

    void read(span<int> r); // read into the range of integers r

    int a[100];
    read(a);        // better: let the compiler figure out the number of elements

**대체 표현**: 컴파일 타임에 할 수 있는 것을 런타임으로 연기하지 마라.

##### 적용

* 포인터 인자를 찾아라.
* 범위 위반에 대한 런타임 검사를 찾아라.

### <a name="Rp-run-time"></a>P.6: 컴파일 타임에 검사할 수 없다면 런타임에 검사할 수 있어야 한다

##### 이유

프로그램 안에 찾기 어려운 오류를 남겨둔다면 크래시나 나쁜 결과를 야기한다.

##### 비고

이상적으로 우리는 컴파일 타임, 런타임에 (프로그래머의 논리에서는 오류가 아닌) 모든 오류를 찾을 수 있다.
컴파일 타임에 모든 오류를 찾아내는 건 불가능하고 런타임에 남아 있는 모든 오류를 찾는 것도 불가능하다.
그러나 충분한 리소스를 준다면 원론적으로 검사 가능한 프로그램을 작성하려고 노력해야 한다. (분석 프로그램, 런타임 검사, 컴퓨터 리소스, 시간)

##### 나쁜 예제

    // separately compiled, possibly dynamically loaded
    extern void f(int* p);

    void g(int n)
    {
        // bad: the number of elements is not passed to f()
        f(new int[n]);
    }

여기서 결정적인 정보(요소의 갯수)가 아주 철저하게 숨겨져 있어서, `f()`가 ABI의 일부일 때 정적 분석은 아마도 불가능해 보이고 동적 검사는 매우 어려울 수 있다. 따라서 해당 포인터를 "측정"할 수 없다.
도움이 될만한 정보를 남은 공간에 넣을 수 있지만, 이는 시스템이나 컴파일러에게 전반적인 변경을 요구한다.
예제에 있는 코드는 오류 발견을 아주 어렵게 만드는 디자인이다.

##### 나쁜 예제

포인터와 함께 요소의 갯수도 같이 전달할 수 있다.

    // separately compiled, possibly dynamically loaded
    extern void f2(int* p, int n);

    void g2(int n)
    {
        f2(new int[n], m);  // bad: a wrong number of elements can be passed to f()
    }

인자로 요소의 갯수를 전달하는 것은 포인터를 전달하면서 관례에 따라 요소의 갯수를 구하는 것보다 낫다.
그러나, 단순히 철자 하나만 틀려도 심각한 오류를 야기한다. `f2()`에서 두 인자간의 연결은 구체적이지 않고 관례에 따른 것이다.

게다가 `f2()`가 인자를 `delete`할 것인지 알 수 없다. (호출자가 두번째 실수를 한 것인가?)

##### 나쁜 예제

표준 라이브러리에 있는 리소스 관리 포인터는 개체를 가리키고 있을 때 크기를 넘길 수 없다.

    // separately compiled, possibly dynamically loaded
    // NB: this assumes the calling code is ABI-compatible, using a
    // compatible C++ compiler and the same stdlib implementation
    extern void f3(unique_ptr<int[]>, int n);

    void g3(int n)
    {
        f3(make_unique<int[]>(n), m);    // bad: pass ownership and size separately
    }

##### 예제

포인터와 요소의 갯수를 하나의 개체로 합쳐서 전달해야 한다.

    extern void f4(vector<int>&);   // separately compiled, possibly dynamically loaded
    extern void f4(span<int>);      // separately compiled, possibly dynamically loaded
                                    // NB: this assumes the calling code is ABI-compatible, using a
                                    // compatible C++ compiler and the same stdlib implementation

    void g3(int n)
    {
        vector<int> v(n);
        f4(v);                     // pass a reference, retain ownership
        f4(span<int>{v});          // pass a view, retain ownership
    }

이 디자인은 개체의 필수 부분으로 요소의 갯수를 전달하므로 오류가 발생하지 않고 항상 저렴하지는 않지만 동적(런타임) 검사를 할 수 있다. 

##### 예제

올바른 사용을 위해 어떻게 소유권과 모든 정보를 전달할 것인가?

    vector<int> f5(int n)    // OK: move
    {
        vector<int> v(n);
        // ... initialize v ...
        return v;
    }

    unique_ptr<int[]> f6(int n)    // bad: loses n
    {
        auto p = make_unique<int[]>(n);
        // ... initialize *p ...
        return p;
    }

    owner<int*> f7(int n)    // bad: loses n and we might forget to delete
    {
        owner<int*> p = new int[n];
        // ... initialize *p ...
        return p;
    }

##### 예제

* ???
* 필요한 것을 실제로 알고 있을 때, 다형 기본 클래스를 전달하는 인터페이스가 어떻게 검사를 피할 수 있는지 보여준다.
  또는 "자유형" 옵션으로 문자열이 있다. 

##### 적용

* (포인터, 갯수)-스타일 인터페이스라면 표시한다. (호환성을 이유로 고칠 수 없는 많은 예제를 표시할 것이다.)
* ???

### <a name="Rp-early"></a>P.7: 런타임 오류는 초기에 잡아라

##### 이유

"미스터리"한 크래시를 피한다.
(아마 몰랐을 수도 있는) 잘못된 결과를 야기하는 오류를 피한다.

##### 예제

    void increment1(int* p, int n)    // bad: error prone
    {
        for (int i = 0; i < n; ++i) ++p[i];
    }

    void use1(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment1(a, m);   // maybe typo, maybe m <= n is supposed
                            // but assume that m == 20
        // ...
    }

여기서 우리는 `use1`에 데이터가 손실되거나 크래시를 야기할 수 있는 작은 오류를 범했다.
(포인터, 크기)-스타일 인터페이스는 범위 오류에 대해 `increment1()`에서 방어할 수 있는 현실적인 방안을 없애버린다.
배열 첨자가 범위를 벗어나는지 검사한다고 가정하면, `p[10]`까지 오류가 발견되지 않을 것이다.
좀 더 빨리 검사하도록 코드를 개선해 보자:

    void increment2(span<int> p)
    {
        for (int& x : p) ++x;
    }

    void use2(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment2({a, m});    // maybe typo, maybe m <= n is supposed
        // ...
    }

이제 `m<=n`은 호출 시점에서 (일찍) 확인할 수 있다.
우리가 가진 모든 것이 오타이므로 `n`을 범위로 사용한다면, 코드를 더 단순하게 만들 수 있다. (오류의 가능성 제거)

    void use3(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment2(a);   // the number of elements of a need not be repeated
        // ...
    }

##### 나쁜 예제

동일한 값을 반복적으로 검사하지 마라. 구조화된 데이터를 문자열로 넘기지 마라.

    Date read_date(istream& is);    // read date from istream

    Date extract_date(const string& s);    // extract date from string

    void user1(const string& date)    // manipulate date
    {
        auto d = extract_date(date);
        // ...
    }

    void user2()
    {
        Date d = read_date(cin);
        // ...
        user1(d.to_string());
        // ...
    }

(`Date` 생성자에 의해) 날짜가 두 번 계산되고 (비구조화된 데이터인) 문자열로 전달된다.

##### 예제

지나친 검사는 비용이 많이 든다.
값이 필요한지도 모르기 때문에 일찍 검사하는 것이 안 좋은 경우도 있고 전체가 아닌 값의 일부만 검사하는 것이 쉬운 경우도 있다.
Similarly, don't add validity checks that change the asymptotic behavior of your interface (e.g., don't add a `O(n)` check to an interface with an average complexity of `O(1)`).

    class Jet {    // Physics says: e * e < x * x + y * y + z * z
        float x;
        float y;
        float z;
        float e;
    public:
        Jet(float x, float y, float z, float e)
            :x(x), y(y), z(z), e(e)
        {
            // Should I check here that the values are physically meaningful?
        }

        float m() const
        {
            // Should I handle the degenerate case here?
            return sqrt(x * x + y * y + z * z - e * e);
        }

        ???
    };

제트기에 대한 물리적 법칙(`e * e < x * x + y * y + z * z`)은 측정 오류의 가능성 때문에 값이 바뀔 수 있다.

???

##### 적용

* 포인터와 배열을 찾아라: 범위를 빨리 검사하고 반복되지 않게 하라.
* 타입 변환을 찾아라: 축소 변환을 표시하거나 제거하라.
* 입력된 값 중 검사되지 않은 값을 찾아라.
* 문자열로 변환되고 있는 구조화된 데이터(불변 조건을 갖는 클래스의 개체)를 찾아라.
* ???

### <a name="Rp-leak"></a>P.8: 리소스가 새도록 하지 마라

##### 이유

Even a slow growth in resources will, over time, exhaust the availability of those resources.
This is particularly important for long-running programs, but is an essential piece of responsible programming behavior.

##### 나쁜 예제

    void f(char* name)
    {
        FILE* input = fopen(name, "r");
        // ...
        if (something) return;   // bad: if something == true, a file handle is leaked
        // ...
        fclose(input);
    }

[RAII](#Rr-raii)를 사용해 개선:

    void f(char* name)
    {
        ifstream input {name};
        // ...
        if (something) return;   // OK: no leak
        // ...
    }

**참고 항목**: [리소스 관리](#S-resource)

##### 비고

A leak is colloquially "anything that isn't cleaned up."
The more important classification is "anything that can no longer be cleaned up."
For example, allocating an object on the heap and then losing the last pointer that points to that allocation.
This rule should not be taken as requiring that allocations within long-lived objects must be returned during program shutdown.
For example, relying on system guaranteed cleanup such as file closing and memory deallocation upon process shutdown can simplify code.
However, relying on abstractions that implicitly clean up can be as simple, and often safer.

##### 비고

Enforcing [the lifetime profile](#In.force) eliminates leaks.
When combined with resource safety provided by [RAII](#Rr-raii), it eliminates the need for "garbage collection" (by generating no garbage).
Combine this with enforcement of [the type and bounds profiles](#In.force) and you get complete type- and resource-safety, guaranteed by tools.

##### 적용

* 포인터를 살펴봐라: 소유자와 비소유자로 구분해라.
  가능하다면 소유자를 (위의 예제처럼) 표준 라이브러리 리소스 핸들로 바꿔라.
  또는 [GSL](#S-gsl)에서 `owner`를 사용하는 것처럼 소유자를 표시하라.
* 처리되지 않은 `new`, `delete`를 찾아라.
* 처리되지 않은 포인터를 반환하는 잘 알려진 리소스 할당 함수를 찾아라. (예를 들어, `fopen`, `malloc`, `strdup`)

### <a name="Rp-waste"></a>P.9: 시간이나 공간을 낭비하지 마라

##### 이유

이것이 C++이다.

##### 비고

개발 속도, 리소스 안정성, 테스트 단순화 등의 목표를 달성하기 위해 소모하는 시간이나 공간은 낭비가 아니다.
"Another benefit of striving for efficiency is that the process forces you to understand the problem in more depth." - Alex Stepanov

##### 나쁜 예제

    struct X {
        char ch;
        int i;
        string s;
        char ch2;

        X& operator=(const X& a);
        X(const X&);
    };

    X waste(const char* p)
    {
        if (p == nullptr) throw Nullptr_error{};
        int n = strlen(p);
        auto buf = new char[n];
        if (buf == nullptr) throw Allocation_error{};
        for (int i = 0; i < n; ++i) buf[i] = p[i];
        // ... manipulate buffer ...
        X x;
        x.ch = 'a';
        x.s = string(n);    // give x.s space for *ps
        for (int i = 0; i < x.s.size(); ++i) x.s[i] = buf[i];  // copy buf into x.s
        delete buf;
        return x;
    }

    void driver()
    {
        X x = waste("Typical argument");
        // ...
    }

그렇다. 풍자를 위한 예제이기는 하지만, 실제 코드에서 이보다 심각한 실수도 본 적이 있다.
`X`의 레이아웃에 (더 많을지도 모르지만) 적어도 6바이트의 낭비가 있다는 점을 주목하라.
복사 동작을 그럴싸하게 정의해 두다 보니 이동의 의미가 없어져 버렸다. 그래서 반환 동작이 느려졌다.
(please note that the Return Value Optimization, RVO, is not guaranteed here).
`buf`에서 `new`, `delete`의 사용이 중복된다. 진짜 지역 문자열을 원했다면, `string` 지역 변수를 사용했을 것이다.
더 많은 성능 버그와 상황을 더 복잡하게 만드는 불필요한 문제가 있다.

##### 나쁜 예제

    void lower(zstring s)
    {
        for (int i = 0; i < strlen(s); ++s) s[i] = tolower(s[i]);
    }

Yes, this is an example from production code.
We leave it to the reader to figure out what's wasted.

##### 비고

낭비에 대한 각 예제는 별로 중요하지 않다. 그리고 중요했다면 이미 전문가들이 쉽게 제거했을 것이다.
그러나 코드 전체에 걸쳐 낭비가 퍼져버리면 중요해 질 수 있고, 낭비를 제거하기 위해서 전문가들을 항상 원하는데로 데려올 수는 없다.
이 규칙(그리고 보다 구체적인 규칙)의 목적은 C++ 사용과 관련된 대부분의 낭비를 없애기 위해서다.
그 후에 알고리즘이나 요구 사항과 관련된 낭비를 살펴볼 수 있다. 하지만 그건 이 가이드라인의 범위를 벗어난다.

##### 적용

더 많은 특정 규칙들은 단순함과 쓸데없는 코드의 제거를 전반적인 목표로 하고 있다.

### <a name="Rp-mutable"></a>P.10: Prefer immutable data to mutable data

##### Reason

It is easier to reason about constants than about variables.
Something immutable cannot change unexpectedly.
Sometimes immutability enables better optimization.
You can't have a data race on a constant.

See [Con: Constants and Immutability](#S-const)

### <a name="Rp-library"></a>P.11: Encapsulate messy constructs, rather than spreading through the code

##### Reason

Messy code is more likely to hide bugs and harder to write.
A good interface is easier and safer to use.
Messy, low-level code breeds more such code.

##### Example

    int sz = 100;
    int* p = (int*) malloc(sizeof(int) * sz);
    int count = 0;
    // ...
    for (;;) {
        // ... read an int into x, exit loop if end of file is reached ...
        // ... check that x is valid ...
        if (count == sz)
            p = (int*) realloc(p, sizeof(int) * sz * 2);
        p[count++] = x;
        // ...
    }

This is low-level, verbose, and error-prone.
For example, we "forgot" to test for memory exhaustion.
Instead, we could use `vector`:

    vector<int> v;
    v.reserve(100);
    // ...
    for (int x; cin >> x; ) {
        // ... check that x is valid ...
        v.push_back(x);
    }

##### Note

The standards library and the GSL are examples of this philosophy.
For example, instead of messing with the arrays, unions, cast, tricky lifetime issues, `gsl::owner`, etc.
that is needed to implement key abstractions, such as `vector`, `span`, `lock_guard, and `future`, we use the libraries
designed and implemented by people with more time and expertise than we usually have.
Similarly, we can and should design and implement more specialized libraries, rather than leaving the users (often ourselves)
with the challenge of repeatedly getting low-level code well.
This is a variant of the [subset of superset principle](#R0) that underlies these guidelines.

##### Enforcement

* Look for "messy code" such as complex pointer manipulation and casting outside the implementation of abstractions.


# <a name="S-interfaces"></a>I(Interfaces): 인터페이스 

인터페이스는 프로그램 두 부분 사이의 계약이다. 서비스의 공급자와 사용자가 기대하는 바를 정확하게 기술하는 것이 핵심이다.
(이해하기 쉽고, 효율적인 사용을 장려하고, 쉽게 오류가 발생하지 않고, 테스트를 지원하는 등) 좋은 인터페이스는 아마도 코드 구성 중에서 가장 중요한 요소 중 하나다.

인터페이스 규칙 요약:

* [I.1: 인터페이스를 명확하게 만들어라](#Ri-explicit)
* [I.2: 전역 변수를 피하라](#Ri-global)
* [I.3: 싱글톤을 피하라](#Ri-singleton)
* [I.4: 인터페이스를 정확하게, 강타입으로 만들어라](#Ri-typed)
* [I.5: (있다면) 사전 조건을 기술하라](#Ri-pre)
* [I.6: 사전 조건을 표현하고 싶다면 `Expects()`를 사용하라](#Ri-expects)
* [I.7: 사후 조건을 기술하라](#Ri-post)
* [I.8: 사후 조건을 표현하고 싶다면 `Ensures()`를 사용하라](#Ri-ensures)
* [I.9: 인터페이스가 템플릿이라면 컨셉(Concept)을 사용해서 매개 변수를 문서화하라](#Ri-concepts)
* [I.10: 요구된 작업의 수행 실패를 알리기 위해 예외를 사용하라](#Ri-except)
* [I.11: 처리되지 않은 포인터(`T*`)로 소유권을 넘기지 마라](#Ri-raw)
* [I.12: NULL이 될 수 없는 포인터는 `not_null`로 선언하라](#Ri-nullptr)
* [I.13: 배열을 단일 포인터로 전달하지 마라](#Ri-array)
* [I.22: Avoid complex initialization of global objects](#Ri-global-init)
* [I.23: 함수 인자 갯수를 최소로 유지하라](#Ri-nargs)
* [I.24: 관련없는 동일 타입의 인접한 매개 변수는 피하라](#Ri-unrelated)
* [I.25: 인터페이스로 클래스 계층보다는 추상 클래스를 사용하라](#Ri-abstract)
* [I.26: 크로스 컴파일러 ABI를 원한다면 C 스타일 코드를 사용하라](#Ri-abi)

참고할 만한 내용:

* [F(Functions): 함수](#S-functions)
* [C.concrete: 실제 타입](#SS-concrete)
* [C.hier: 클래스 계층](#SS-hier)
* [C.over: 오버로딩과 오버로딩된 연산자](#SS-overload)
* [C.con: 컨테이너와 다른 리소스 핸들](#SS-containers)
* [E: 오류 처리](#S-errors)
* [T: 템플릿과 제너릭 프로그래밍](#S-templates)

### <a name="Ri-explicit"></a>I.1: 인터페이스를 명확하게 만들어라

##### 이유

정확성. 인터페이스에 언급되지 않은 가정은 간과되기 쉽고 테스트하기 어렵다.

##### 나쁜 예제

전역 (네임스페이스 범위에 있는) 변수를 통해 함수의 행동을 제어하는 것은 암시적이고 혼란스럽다. 예를 들어,

    int rnd(double d)
    {
        return (rnd_up) ? ceil(d) : d;    // don't: "invisible" dependency
    }

`rnd(7.2)`를 두 번 호출해서 서로 다른 결과가 발생한다면 호출하는 입장에서는 함수의 의미를 명확하게 알지 못할 것이다.

##### 예외

때때로 우리는 환경 변수를 통해 연산의 세부 사항을 제어한다. 예를 들어, 일반적인 출력 대 상세한 출력, 디버그 대 최적화 등이 있다.
전역 제어를 사용하면 잠재적인 문제가 있기는 하지만, 그렇게 하지 않으면 고정되었을 의도를 구현하는데 세밀하게 제어할 수 있다.

##### 나쁜 예제

`errno`와 같이 전역 변수를 통해 오류를 보고하는 것은 무시되기 쉽다. 예를 들어,

    // don't: no test of printf's return value
    fprintf(connection, "logging: %d %d %d\n", x, y, s);

만약 연결(connection)이 다운되어 로그가 만들어지지 않는다면 어떨까? I.?? 규칙을 봐라.

**대안**: 예외를 발생시켜라. 예외는 무시할 수 없다.

**공식 대안**: 전역 또는 암시적 상태의 값을 통해 인터페이스로 정보가 전달되는 것을 피하라.
`const`가 아닌 멤버 함수는 개체의 상태를 통해 다른 멤버 함수에게 정보를 전달한다는 점을 참고하라. 

**공식 대안**: 인터페이스는 함수나 함수의 집합이어야 한다.
함수는 템플릿 함수일 수 있고 함수 집합은 클래스나 클래스 템플릿일 수 있다.

##### 적용

* (간단함) 함수는 네임스페이스 범위에서 선언된 변수 값에 따라 제어 흐름을 결정해서는 안된다.
* (간단함) 함수는 네임스페이스 범위에서 선언된 변수에 값을 저장해서는 안된다.

### <a name="Ri-global"></a>I.2: 전역 변수를 피하라

##### 이유

const가 아닌 전역 변수는 의존성을 숨기고 예측하지 못한 변화에 의존하게 만든다.

##### 예제

    struct Data {
        // ... lots of stuff ...
    } data;            // non-const data

    void compute()     // don't
    {
        // ... use data ...
    }

    void output()     // don't
    {
        // ... use data ...
    }

다른 누군가가 `data`를 수정할 수 있는가?

##### 비고

전역 상수는 유용하다.

##### 비고

전역 변수에 반하는 규칙은 네임스페이스 스코프의 변수에도 동일하게 적용한다.

**대안**: 복사를 피하기 위해 전역 변수(좀 더 일반화해서 네임스페이스 스코프의 데이터)를 사용한다면 `const` 레퍼런스로 개체를 전달하는 것을 고려하라.
다른 해결책은 개체의 상태로서의 데이터와 멤버 함수로서의 연산을 정의하는 것이다. 

**경고**: 데이터 경합을 조심하라. 하나의 스레드가 실행되는 동안 다른 스레드가 전역 데이터(또는 레퍼런스로 전달된 데이터)에 접근하려고 한다면, 데이터 경합이 발생할 수 있다.
Every pointer or reference to mutable data is a potential data race.

##### 비고

변경할 수 없는 데이터에 대해서는 경합 조건이 생기지 않는다.

**참고 항목**: [함수 호출에 대한 규칙](#SS-call)을 보라.

##### 적용

(간단함) 네임스페이스 스코프 내에 정의된 `const`가 아닌 변수에 대해서 모두 보고하라.

### <a name="Ri-singleton"></a>I.3: 싱글톤을 피하라

##### 이유

싱글톤은 기본적으로 복잡한 전역 개체이다.

##### 예제

    class Singleton {
        // ... lots of stuff to ensure that only one Singleton object is created,
        // that it is initialized properly, etc.
    };

싱글톤 아이디어에는 많은 변형이 있다.
그것이 문제의 일부다.

##### 비고

전역 개체가 변경되지 않게 하려면, `const`나 `constexpr`로 선언하라.

##### 예외

제일 단순한 "싱글톤"(처음에는 싱글톤으로 간주하지 않으므로 간단함)을 사용해 처음 사용시 초기화를 할 수 있다.

    X& myX()
    {
        static X my_x {3};
        return my_x;
    }

이 방식은 초기화 순서와 관련된 문제에 대해 가장 효과적인 해결책 중 하나다.
다중 스레드 환경에서 정적 개체의 초기화는 경합 조건을 유발하지 않는다.
(단, 생성자 내에서 공유 개체에 부주의하게 접근하지 않아야 한다.)

Note that the initialization of a local `static` does not imply a race condition.
However, if the destruction of `X` involves an operation that needs to be synchronized we must use a less simple solution.
For example:

    X& myX()
    {
        static auto p = new X {3};
        return *p;  // potential leak
    }

Now someone has to `delete` that object in some suitably thread-safe way.
That's error-prone, so we don't use that technique unless

* `myX` is in multithreaded code,
* that `X` object needs to be destroyed (e.g., because it releases a resource), and
* `X`'s destructor's code needs to be synchronized.

하나의 개체만 생성해야 하는 클래스로 싱글톤을 정의한다면 `myX`와 같은 함수는 싱글톤이 아니다. 그리고 이 유용한 테크닉은 싱글톤이 아닌 규칙에 대한 예외는 아니다.

##### 적용

일반적으로 매우 힘들다.

* `singleton`을 포함하는 이름을 가진 클래스를 찾아라.
* 개체를 세거나 생성자를 검사해 단일 개체만 만들어진 클래스를 찾아라.
* If a class X has a public static function that contains a function-local static of the class' type X and returns a pointer or reference to it, ban that.

### <a name="Ri-typed"></a>I.4: 인터페이스를 정확하게, 강타입으로 만들어라

##### 이유

타입은 가장 단순하지만 최고의 문서이기도 하다. 잘 정의된 의미를 갖고 있고, 컴파일 타임 검사가 보장된다.
또한 정확하게 입력된 코드는 더 잘 최적화된다.

##### 하지 말아야 할 예제

다음을 고려해 보자:

    void pass(void* data);    // void* is suspicious

이제 피호출자에서 올바른 타입으로 사용하기 위해 `data` 포인터를 캐스팅해야 한다. 하지만 오류가 발생하기 쉽고, 구질구질하다.
특히 인터페이스에서 `void*`를 피하라.
대신 베이스 클래스를 가리키는 포인터나 `variant` 사용을 고려하라.

**대안**: 때때로 템플릿 매개 변수는 `void*`를 제거하고 `T*`나 `T&`로 변환할 수 있다.
For generic code these `T`s can be general or concept constrained template parameters.

##### 나쁜 예제

다음을 고려해 보자:

    void draw_rect(int, int, int, int);   // great opportunities for mistakes

    draw_rect(p.x, p.y, 10, 20);          // what does 10, 20 mean?

`int`는 임의 형태의 정보를 전달할 수 있어서, 네 `int`가 각각 어떤 의미를 갖는지를 유추해야 한다.
아마도 처음 두 `int`는 `x`, `y` 좌표일 것 같지만, 나머지 두 `int`는 무엇을 의미할까?
주석이나 매개 변수 이름이 도움을 줄 수 있다. 보다 명확히 한다면, 다음과 같다.

    void draw_rectangle(Point top_left, Point bottom_right);
    void draw_rectangle(Point top_left, Size height_width);

    draw_rectangle(p, Point{10, 20});  // two corners
    draw_rectangle(p, Size{10, 20});   // one corner and a (height, width) pair

분명히 정적 타입 시스템을 통해 모든 오류를 잡아낼 수는 없다.
(예를 들어, 첫번째 인자가 왼쪽 상단에 있는 점이라는 사실은 이름이나 주석 등을 통해 편의상 정해져 있을 뿐이다.)

##### 나쁜 예제

다음 예제에서 인터페이스만 보고는 `time_to_blink`이 무엇을 의미하는지 잘 모르겠다. 초를 의미할까? 밀리초를 의미할까?

    void blink_led(int time_to_blink) // bad -- the unit is ambiguous
    {
        // ...
        // do something with time_to_blink
        // ...
    }

    void use()
    {
        blink_led(2);
    }

##### 좋은 예제

C++11에 도입된 `std::chrono::duration` 타입은 지속 시간의 단위를 명시적으로 표현하는데 도움이 된다.

    void blink_led(milliseconds time_to_blink) // good -- the unit is explicit
    {
        // ...
        // do something with time_to_blink
        // ...
    }

    void use()
    {
        blink_led(1500ms);
    }

어떤 종류의 지속 시간의 단위도 허용하도록 함수를 다음과 같이 작성할 수도 있다.

    template<class rep, class period>
    void blink_led(duration<rep, period> time_to_blink) // good -- accepts any unit
    {
        // assuming that millisecond is the smallest relevant unit
        auto milliseconds_to_blink = duration_cast<milliseconds>(time_to_blink);
        // ...
        // do something with milliseconds_to_blink
        // ...
    }

    void use()
    {
        blink_led(2s);
        blink_led(1500ms);
    }

##### 적용

* (간단함) `void*`를 매개 변수나 리턴 타입으로 사용한다면 보고하라.
* (잘하기 어려움) 다수의 내장 타입 인자를 갖는 멤버 함수를 찾아라.

### <a name="Ri-pre"></a>I.5: (있다면) 사전 조건을 기술하라

##### 이유

인자는 피호출자에서 적절한 사용을 제한할 수 있는 의미를 갖는다.

##### 예제

다음을 고려해 보자:

    double sqrt(double x);

여기서 `x`는 반드시 음수가 아니어야 한다. 타입 시스템으로는 이를 (있는 그대로 쉽게) 표현할 수 없고, 그래서 다른 수단을 사용해야 한다. 예를 들어,

    double sqrt(double x); // x must be nonnegative

일부 사전 조건은 단정문으로 표현하기도 한다. 예를 들어,

    double sqrt(double x) { Expects(x >= 0); /* ... */ }

이상적으로 `Expects(x >= 0)` 조건이 `sqrt()`의 인터페이스에 일부분이 되는게 가장 좋지만 그렇게 하기는 쉽지 않다. 따라서 지금은 함수 정의부(함수 본문)에 위치시킨다.

**참고 항목**: `Expects()`는 [GSL](#S-gsl)에 기술되어 있다.

##### 비고

`Excepts(p != nullptr);`처럼 요구 사항의 공식적인 명세를 선호하라.
불가능하다면, `// the sequence [p:q) is ordered using <`와 같이 영문 텍스트 주석을 사용하라.

##### 비고

대부분의 멤버 함수는 클래스의 불변 조건 중 일부에 해당하는 선행 조건을 갖고 있다.
해당 불변 조건은 생성자에서 구성되는데 클래스 외부로부터 호출되는 모든 멤버 함수를 통해 재구성되어야 한다.
그래서 각 함수마다 언급할 필요는 없다.

##### 적용

(적용 불가능)

**참고 항목**: 포인터 전달에 대한 규칙. ???

### <a name="Ri-expects"></a>I.6: 사전 조건을 표현하고 싶다면 `Expects()`를 사용하라

##### 이유

사전 조건임을 명확하게 표시하고 툴 사용을 쉽게 만들기 위해서다.

##### 예제

    int area(int height, int width)
    {
        Expects(height > 0 && width > 0);            // good
        if (height <= 0 || width <= 0) my_error();   // obscure
        // ...
    }

##### 비고

사전 조건은 `if`문, `assert()`문, 주석문 등으로 기술할 수 있다.
하지만 이런 구문은 일반 코드와 구분, 갱신하거나 툴로 조작하기 어렵고 잘못된 의미를 가질 수도 있다. (디버그 모드일 때는 중단시키고, 릴리즈 모드일때는 검사하고 싶은가?)

##### 비고

사전 조건은 구현 부분보다는 인터페이스 부분에 포함시켜야 한다. 하지만 아직은 그렇게 할 수 있는 언어 기능이 없다.
Once language support becomes available (e.g., see the [contract proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf)) we will adopt the standard version of preconditions, postconditions, and assertions.

##### 비고

`Expects()`은 알고리즘 중간에 조건을 검사하는데 사용할 수도 있다.

##### 적용

(적용 불가능) 사전 조건이 단정될 수 있는 다양한 방법을 찾는 것은 실현 가능하지 않다. 쉽게 식별할 수 있는 것들에 대해 경고(`assert()`)를 할 수 있는 언어 기능이 없다면 의심의 여지가 생길 수 밖에 없다.

### <a name="Ri-post"></a>I.7: 사후 조건을 기술하라

##### 이유

결과에 대해 잘못 이해하고 있는 부분과 잘못된 구현을 찾아내기 위해서다.
To detect misunderstandings about the result and possibly catch erroneous implementations.

##### 나쁜 예제

다음을 고려해 보자:

    int area(int height, int width) { return height * width; }  // bad

여기서 부주의하게 높이와 폭이 양수여야 한다는 사전 조건 명세를 빠뜨렸다.
역시 넓이를 구하는 알고리즘(`hieght * width`)이 정수의 최댓값보다 클 수 있다는 사후 조건 명세를 빠뜨렸다.
오버플로우가 발생할 수 있다.

다음 사용을 고려해 보자:

    int area(int height, int width)
    {
        auto res = height * width;
        Ensures(res > 0);
        return res;
    }

##### 나쁜 예제

악명 높은 보안 버그를 고려해 보자:

    void f()    // problematic
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, MAX);
    }

버퍼가 초기화되어야 하고 최적화하면서 중복되는 `memset()`의 호출을 제거했음을 설명하는 사후 조건이 없다.

    void f()    // better
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, MAX);
        Ensures(buffer[0] == 0);
    }

##### 비고

사후 조건은 종종 함수의 목적을 기술하는 주석문에 비공식적으로 명시되어 있다. `Ensures()`를 사용함으로서 더 시스템적으로 검사하는 모습을 보여줄 수 있다.

##### 비고

사후 조건은 사용되는 자료 구조의 상태처럼 반환된 결과에 간접적으로 영향을 미치는 무언가에 연관되어 있을 때 특히 중요하다.

##### 예제

경합 조건을 피할 목적으로 `mutex`를 사용해 `Record`를 조작하는 함수를 고려해 보자:

    mutex m;

    void manipulate(Record& r)    // don't
    {
        m.lock();
        // ... no m.unlock() ...
    }

여기서 `mutex`를 해제해야 한다는 언급을 "잊어"버렸는데, `mutex` 해제를 언급하지 않은 것이 단순히 버그인지 의도한 것인지 알 수가 없다.
사후 조건을 기술함으로서 언급을 명확하게 할 수 있다.

    void manipulate(Record& r)    // postcondition: m is unlocked upon exit
    {
        m.lock();
        // ... no m.unlock() ...
    }

이제 버그가 분명하게 보인다. (단, 주석을 읽는 사람에게만 보인다.)

더 나은 방법은 [RAII](#Rc-raii)를 사용해 사후 조건("락을 해제해야 함")이 코드에 적용되도록 하는 것이다.

    void manipulate(Record& r)    // best
    {
        lock_guard<mutex> _ {m};
        // ...
    }

##### 비고

이상적으로, 사후 조건은 사용자들이 쉽게 볼 수 있도록 인터페이스/선언부에 기술해야 한다.
사용자들에게 언급되어야 하는 사후 조건만 인터페이스에 기술해야 한다.
내부 상태에 대한 사후 조건은 정의/구현부에만 기술한다.

##### 적용

(적용 불가능) This is a philosophical guideline that is infeasible to check
directly in the general case. Domain specific checkers (like lock-holding
checkers) exist for many toolchains.

### <a name="Ri-ensures"></a>I.8: 사후 조건을 표현하고 싶다면 `Ensures()`를 사용하라

##### 이유

사후 조건이라는 것을 분명히 하기 위해, 또 분석 툴을 사용하기 위해서다.

##### 예제

    void f()
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, MAX);
        Ensures(buffer[0] == 0);
    }

##### 비고

사후 조건은 주석문, `if`문, `assert()`문 등 다양한 방식으로 기술할 수 있다.
이는 일반적인 코드와 구분을 어렵게 만들고, 갱신하기 어렵게 만들고, 툴로 조작하기 어렵게 만들며 틀린 의미를 가질 수도 있다.

**대안**: "이 리소스는 반드시 해제되어야 한다" 형태의 사후 조건은 [RAII](#Rc-raii)를 통해 가장 잘 나타낼 수 있다.

##### 비고

이상적으로 `Ensures`는 인터페이스의 일부가 되어야 하지만, 쉽게 할 수 있는 작업이 아니다.
현재로서는 정의 부분(함수 본문)에 위치시킨다.
Once language support becomes available (e.g., see the [contract proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf)) we will adopt the standard version of preconditions, postconditions, and assertions.

##### 적용

(적용 불가능) 사후 조건을 단정할 수 있는 다양한 방식을 찾는 것은 실현 가능하지 않다. 언어 명세에 쉽게 확인할 수 있는 경고(`assert()`)문이 없다면 의문의 여지가 생길 수 밖에 없다.uage facility.

### <a name="Ri-concepts"></a>I.9: 인터페이스가 템플릿이라면 컨셉(Concept)을 사용해서 매개 변수를 문서화하라

##### 이유

인터페이스를 정확하게, 그리고 (멀지 않은) 미래에 컴파일 타임 검사를 할 수 있게 만들어라.

##### 예제

요구 사항 명세의 ISO 컨셉 TS 스타일을 사용하라. 예를 들어,

    template<typename Iter, typename Val>
    // requires InputIterator<Iter> && EqualityComparable<ValueType<Iter>>, Val>
    Iter find(Iter first, Iter last, Val v)
    {
        // ...
    }

##### 비고

곧 (아마도 2017년), `//`가 제거되면 대부분의 컴파일러가 `requires` 구문을 검사할 수 있을 것이다.
현재 컨셉 TS는 GCC 6.1에서만 지원된다.

**참고 항목**: [제너릭 프로그래밍](#SS-GP)과 [컨셉](#SS-t-concepts)을 보라.

##### 적용

(아직 적용 불가능) 언어 명세를 작성중이다. 언어 명세가 정의된 후, 비가변 템플릿 매개 변수가 (선언이나 `requires`문에 언급되지 않은) 컨셉으로 제한되지 않는다면 경고하라.

### <a name="Ri-except"></a>I.10: 요구된 작업의 수행 실패를 알리기 위해 예외를 사용하라

##### 이유

시스템이나 계산 결과를 정의되지 않은(예측 불가능한) 상태로 둘 수 없으므로 오류를 무시해서는 안된다.
이는 오류의 주 발생지가 된다.

##### 예제

    int printf(const char* ...);    // bad: return negative number if output fails

    template <class F, class ...Args>
    // good: throw system_error if unable to start the new thread
    explicit thread(F&& f, Args&&... args);

##### 비고

오류란 무엇인가?

오류란 함수가 (사후 조건 설정을 포함해) 의도한 목적을 이룰 수 없는 것을 의미한다.
오류를 무시하는 코드를 호출하면 정의되지 않은 시스템 상태나 잘못된 결과를 야기할 수 있다.
예를 들어, 리모트 서버에 연결할 수 없는 경우 자체적으로 오류가 발생하지 않는다.
서버는 다양한 이유로 연결을 거부할 수 있다. 따라서 호출하는 쪽에서 결과를 확인할 수 있도록 반환해 주는 것이 자연스럽다.
그러나 연결에 실패하는 것을 오류로 간주할 생각이라면, 실패할 경우 예외를 던져야 한다.

##### 예외

기존의 많은 인터페이스 함수들은 오류가 아닌 실제 상태 코드를 보고하기 위해 오류 코드(예를 들어, `errno`)를 사용한다. 이에 대해 별다른 대안이 없으므로 규칙을 위반하지는 않는다.

##### 대안

만약 (예를 들어, 여러분의 코드가 예전 스타일의 처리되지 않은 포인터로 가득하거나 실시간 제약이 심각하기 때문에) 예외를 사용할 수 없다면, 한 쌍의 값을 반환하는 스타일을 사용하는 것을 고려해 보라.

    int val;
    int error_code;
    tie(val, error_code) = do_something();
    if (error_code == 0) {
        // ... handle the error or exit ...
    }
    // ... use val ...

This style unfortunately leads to uninitialized variable.
A facility [structured bindings](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0144r1.pdf) to deal with that will become available in C++17.

    [val, error_code] = do_something();
    if (error_code == 0) {
        // ... handle the error or exit ...
    }
    // ... use val ...


##### 비고

"성능"이 예외를 사용하지 않아야 하는 타당한 이유라고는 생각하지 않는다.

* 명시적인 오류 검사 및 처리는 종종 예외 처리만큼 많은 시간과 공간을 쓰기도 한다.
* 간결한 코드는 종종 예외 처리를 포함하더라도 더 좋은 성능을 갖는다. (프로그램과 최적화를 통해 실행 경로를 단순화한다.)
* 성능이 중요한 코드의 좋은 규칙은 오류 검사를 코드의 핵심 부분 바깥으로 옮기는 것이다 ([검사](#Rper-checking)).
* 장기적으로 보면 정규 코드가 더 잘 최적화된다.
* Always carefully [measure](#Rper-measure) before making performance claims.

**참고 항목**: 사전 조건, 사후 조건의 위반 보고를 위한 [I.5](#Ri-pre) 및 [I.7](#Ri-post).

##### 적용

* (적용 불가능) 철저한 점검이 불가능한 철학적 가이드라인이다.
* `errno`를 살펴 봐라.

### <a name="Ri-raw"></a>I.11: 처리되지 않은 포인터(`T*`)로 소유권을 넘기지 마라

##### 이유

호출하는 쪽이나 받는 쪽 중 누가 개체를 소유할 것인지 모른다면, 메모리 누수나 조기 파괴가 발생할 것이다.

##### 예제

다음을 고려해 보자:

    X* compute(args)    // don't
    {
        X* res = new X{};
        // ...
        return res;
    }

반환된 `X`를 누가 삭제할 것인가? 만약 레퍼런스를 반환한다면 문제는 더 어려워질 것이다.
결과가 값으로 반환되었다고 고려해 보자 (결과로 반환되는 값의 크기가 크다면 이동 문법을 사용하라).

    vector<double> compute(args)  // good
    {
        vector<double> res(10000);
        // ...
        return res;
    }

**대안**: (독점적인 소유를 위한) `unique_ptr` 또는 (소유권 공유를 위한) `shared_ptr`와 같이 "스마트 포인터"를 사용해서 소유권을 넘겨라.
그러나 레퍼런스 의미가 필요하지 않다면 덜 우아하고 덜 효율적일 것이다.

**대안**: ABI 호환성 요구 사항 또는 리소스 부족으로 인해 오래된 코드를 수정할 수 없는 경우가 있다.
이 경우, [가이드라인 지원 라이브러리](#S-gsl)의 `owner`를 사용해 포인터의 소유권을 표시하라.

    owner<X*> compute(args)    // It is now clear that ownership is transferred
    {
        owner<X*> res = new X{};
        // ...
        return res;
    }

위의 코드는 분석 툴에게 `res`가 소유권자라고 알려준다.
즉, 이 값은 `return`을 통해 행해진 것처럼 `delete`되거나 다른 소유권자에게 넘겨줘야 한다.

`owner`는 리소스 핸들의 구현에서 비슷하게 사용된다.

##### 비고

처리되지 않은 포인터(또는 반복자)로 전달된 모든 개체는 호출자가 소유한 것으로 간주되므로 호출자가 수명을 처리한다.
Viewed another way: ownership transferring APIs are relatively rare compared to pointer-passing APIs, so the default is "no ownership transfer."

**참고 항목**: [인수 전달](#Rf-conventional) 및 [값 반환](#Rf-T-return).

##### 적용

* (간단함) `owner`가 아닌 처리되지 않은 포인터의 `delete`에 대해 경고를 표시하라.
* (간단함) 모든 코드 경로에서 `owner` 포인터를 `reset`하거나 명시적으로 `delete`를 실패하게 되면 경고를 표시하라.
* (간단함) `new`의 반환 값이나 포인터 타입의 반환 값을 갖는 함수 호출이 처리되지 않은 포인터에 할당되면 경고를 표시하라.

### <a name="Ri-nullptr"></a>I.12: NULL이 될 수 없는 포인터는 `not_null`로 선언하라

##### 이유

`nullptr` 역참조 오류를 피하기 위해서다.
그리고 `nullptr`를 반복해서 검사하는 경우를 피해 성능을 향상시키기 위해서다.

##### 예제

    int length(const char* p);            // it is not clear whether length(nullptr) is valid

    length(nullptr);                      // OK?

    int length(not_null<const char*> p);  // better: we can assume that p cannot be nullptr

    int length(const char* p);            // we must assume that p can be nullptr

소스 코드에 의도를 명시함으로써, 컴파일러와 툴이 정적 분석을 통해 일부 오류 클래스를 찾아내는 등의 보다 나은 진단을 제공하고 분기 및 널(NULL) 검사를 제거하는 등의 최적화 작업을 수행할 수 있다.

##### 비고

`not_null`은 [가이드라인 지원 라이브러리](#S-gsl)에 정의되어 있다.

##### 비고

`char`에 대한 포인터가 C-스타일 문자열(`\0`으로 끝나는 문자열)을 가리키고 있다는 가정은 여전히 암묵적이며 혼란과 오류를 발생시키는 원인이 될 수 있다. `const char*`보다는 `zstring`을 사용하라.

    // we can assume that p cannot be nullptr
    // we can assume that p points to a zero-terminated array of characters
    int length(not_null<zstring> p);

비고: 물론 `length()`는 `std::strlen()`이다.

##### 적용

* (간단함) ((기초)) 함수가 모든 제어-흐름 경로에서 포인터 매개 변수에 접근하기 전에 `nullptr`인지 검사한다면, `not_null`으로 선언되어야 한다는 경고를 표시하라.
* (복잡함) 포인터 반환 값을 갖는 함수가 모든 반환 경로에서 `nullptr`이 아닌지 확인한다면, 리턴 타입을 `not_null`으로 선언해야 된다는 경고를 표시하라.

### <a name="Ri-array"></a>I.13: 배열을 단일 포인터로 전달하지 마라

##### 이유

(포인터, 크기)-스타일 인터페이스는 오류가 발생하기 쉽다. 또한 (배열에 대한) 일반 포인터는 호출을 받는 곳에서 크기를 결정할 수 있도록 몇 가지 관례에 의존해야 한다.

##### 예제

다음을 고려해 보자:

    void copy_n(const T* p, T* q, int n); // copy from [p:p+n) to [q:q+n)

만약 `q`가 가리키는 배열의 원소 갯수가 `n`보다 적다면 어떻게 될까? 관계없는 메모리를 덮어쓰게 된다.
만약 `p`가 가리키는 배열의 원소 갯수가 `n`보다 적다면 어떻게 될까? 관계없는 메모리를 읽을 것이다.
어느 쪽이나 정의되지 않은 동작을 수행하거나 매우 불쾌한 버그가 발생할 수 있다.

##### 대안

명시적인 범위 사용을 고려해 보라:

    void copy(span<const T> r, span<T> r2); // copy r to r2

##### 나쁜 예제

다음을 고려해 보자:

    void draw(Shape* p, int n);  // poor interface; poor code
    Circle arr[10];
    // ...
    draw(arr, 10);

`n`의 인수로 `10`을 전달하는 것은 실수일 수 있다. 가장 일반적인 관례는 \[`0`:`n`)이라고 가정하는 것이지만, 이러한 내용이 어디에도 언급되어 있지 않다.
더 나쁜 부분은 어쨌든 `draw()` 호출이 컴파일된다는 것이다. 배열에서 포인터로의 암시적 변환(배열 부패)이 있었고, `Circle`에서 `Shape`로의 또 다른 암시적 변환이 있었다.
`draw()`가 그 배열을 통해 안전하게 반복문을 수행할 수 있는 방법은 없다. 왜냐하면 요소의 크기를 알 수 있는 방법이 없기 때문이다.

**대안**: 요소의 크기를 보장하고 위험한 암시적 변환을 방지하는 지원 클래스를 사용하라.

    void draw2(span<Circle>);
    Circle arr[10];
    // ...
    draw2(span<Circle>(arr));  // deduce the number of elements
    draw2(arr);    // deduce the element type and array size

    void draw3(span<Shape>);
    draw3(arr);    // error: cannot convert Circle[10] to span<Shape>

이 `draw2()`는 같은 양의 정보를 `draw()`에 전달하지만, 명시적으로 `Circle`이 되어야 한다는 사실을 알 수 있다. ???을 보라.

##### 예외

`zstring`과 `czstring`을 사용해 C-스타일의 `\0`로 끝나는 문자열을 나타내라.
But when doing so, use `string_span` from the [GSL](#GSL) to prevent range errors.

##### 적용

* (간단함) ((경계)) 배열 타입에서 포인터 타입으로의 암시적 변환에 의존하는 표현식에 대해 경고를 표시하라. zstring/czstring 포인터 타입에 대해서는 예외를 허용한다.
* (간단함) ((경계)) 포인터 타입의 값을 가져오는 포인터 타입의 표현식에 대한 산술 연산에 대해 경고를 표시하라. zstring/czstring 포인터 타입에 대해서는 예외를 허용한다.

### <a name="Ri-global-init"></a>I.22: Avoid complex initialization of global objects

##### Reason

Complex initialization can lead to undefined order of execution.

##### Example

    // file1.c

    extern const X x;

    const Y y = f(x);   // read x; write y

    // file2.c

    extern const Y y;

    const X x = g(y);   // read y; write x

Since `x` and `y` are in different translation units the order of calls to `f()` and `g()` is undefined;
one will access an uninitialized `const`.
This particular example shows that the order-of-initialization problem for global (namespace scope) objects is not limited to global *variables*.

##### Note

Order of initialization problems become particularly difficult to handle in concurrent code.
It is usually best to avoid global (namespace scope) objects altogether.

##### Enforcement

* Flag initializers of globals that call non-`constexpr` functions
* Flag initializers of globals that access `extern` objects

### <a name="Ri-nargs"></a>I.23: 함수 인자 갯수를 최소로 유지하라

##### 이유

인자 갯수가 많으면 혼란을 일으킬 수 있다.
인자를 많이 전달하는 것은 제시할 대안에 비해 비용이 많이 든다.

##### 예제

표준 라이브러리 `merge()`는 편하게 처리할 수 있는 한계에 있다.

    template<class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
    OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                         InputIterator2 first2, InputIterator2 last2,
                         OutputIterator result, Compare comp);

여기에 4개의 템플릿 인자와 6개의 함수 인자가 있다.
단순화하기 위해 가장 빈번하게 사용하는 많이 쓰는 방식으로 비교 인수의 디폴트 값인 `<`을 사용해 보자.

    template<class InputIterator1, class InputIterator2, class OutputIterator>
    OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                         InputIterator2 first2, InputIterator2 last2,
                         OutputIterator result);

이렇게 한다고 해서 전체적인 복잡성이 줄어들지는 않지만 사용자 입장에서 볼 때는 복잡성이 줄어든 것처럼 보인다.
실제로 인자 갯수를 줄이려면 인자를 좀 더 높은 수준의 추상화로 묶어야 한다.

    template<class InputRange1, class InputRange2, class OutputIterator>
    OutputIterator merge(InputRange1 r1, InputRange2 r2, OutputIterator result);

인자를 "묶어서" 그룹화하는 것은 인자의 갯수를 줄이고 검사할 기회를 늘리는 일반적인 기법이다.

Alternatively, we could use concepts (as defined by the ISO TS) to define the notion of three types that must be usable for merging:

    Mergeable{In1 In2, Out}
    OutputIterator merge(In1 r1, In2 r2, Out result);

##### 비고

얼마나 많은 인자가 있어야 너무 많다고 말할 수 있을까? 인자가 4개라면 많다고 말할 수 있다.
4개의 인자로 가장 잘 표현할 수 있는 함수들도 있지만, 많지는 않다.

**대안**: 인자를 의미있는 개체로 그룹화하고 개체를 전달하라. (값에 의한 전달 또는 레퍼런스에 의한 전달)

**대안**: 더 적은 인자 갯수로 가장 일반적인 형태의 호출을 할 수 있는 디폴트 인자나 오버로드를 사용하라.

##### 적용

* 범위 또는 뷰가 아닌 동일한 타입의 반복자(포인터 포함)를 2개 이상 선언하는 함수가 있다면 경고를 표시하라.
* (적용 불가능) 철저한 점검이 불가능한 철학적 가이드라인이다.

### <a name="Ri-unrelated"></a>I.24: 관련없는 동일 타입의 인접한 매개 변수는 피하라

##### 이유

실수로 동일 타입의 인접한 인수를 쉽게 바꿀 수 있다.

##### 나쁜 예제

다음을 고려해 보자:

    void copy_n(T* p, T* q, int n);  // copy from [p:p+n) to [q:q+n)

위 코드는 K&R C-스타일 인터페이스의 적절하지 못한 변형이다. "to"와 "from" 인수를 쉽게 바꿀 수 있다.

"from" 인자에 `const`를 사용하라.

    void copy_n(const T* p, T* q, int n);  // copy from [p:p+n) to [q:q+n)

##### 예외

If the order of the parameters is not important, there is no problem:

    int max(int a, int b);

##### 대안

배열을 포인터로 전달하지 말고 범위를 나타내는 개체(예를 들어, `span`)로 전달하라.

    void copy_n(span<const T> p, span<T> q);  // copy from p to q

##### 대안

Define a `struct` as the parameter type and name the fields for those parameters accordingly:

    struct SystemParams {
        string config_file;
        string output_path;
        seconds timeout;
    };
    void initialize(SystemParams p);

This has a tendency to make invocations of this clear to future readers, as the parameters
are often filled in by name at the call site.

##### 적용

(간단함) 연속하는 두 매개 변수가 동일한 타입을 공유하는 경우 경고를 표시하라.

### <a name="Ri-abstract"></a>I.25: 인터페이스로 클래스 계층보다는 추상 클래스를 사용하라

##### 이유

추상 클래스는 상태가 있는 베이스 클래스보다 안정적이다.

##### 나쁜 예제

당신은 `Shape`가 어디선가 나타날 것이라고 믿고 있었을 것이다. :-)

    class Shape {  // bad: interface class loaded with data
    public:
        Point center() const { return c; }
        virtual void draw() const;
        virtual void rotate(int);
        // ...
    private:
        Point c;
        vector<Point> outline;
        Color col;
    };

이렇게 하면 파생된 모든 클래스가 중심을 계산하게 된다.
비록 중요하지 않고 중심이 사용되지 않더라도 말이다.
비슷하게, 모든 `Shape`가 `Color`를 갖고 있는 것은 아니며 많은 `Shape`들은 일련의 `Point`로 정의된 윤곽선없이 가장 잘 표현된다.
추상 클래스는 사용자가 그러한 클래스를 작성하지 못하도록 만들기 위해 고안되었다.

    class Shape {    // better: Shape is a pure interface
    public:
        virtual Point center() const = 0;   // pure virtual function
        virtual void draw() const = 0;
        virtual void rotate(int) = 0;
        // ...
        // ... no data members ...
    };

##### 적용

(간단함) `C` 클래스를 가리키는 포인터가 `C`의 베이스를 가리키는 포인터에 할당되고 베이스 클래스에 데이터 멤버가 있으면 경고를 표시하라.

### <a name="Ri-abi"></a>I.26: 크로스 컴파일러 ABI를 원한다면 C 스타일 코드를 사용하라

##### 이유

컴파일러마다 클래스, 예외 처리, 함수 이름 및 기타 구현 세부 사항에 대해 서로 다른 바이너리 레이아웃을 구현한다.

##### 예외

신중하게 선택한 몇 가지 고급 수준의 C++ 타입을 사용해 인터페이스를 신중하게 만들 수 있다. ???을 보라.

##### 예외

일반적인 ABI는 일부 플랫폼에서 점점 더 엄격한 제한으로부터 벗어나고 있다.

##### 비고

단일 컴파일러를 사용하는 경우 인터페이스에서 C++ 전체를 사용할 수 있다. 새 컴파일러 버전으로 업그레이드 한 후에는 다시 컴파일해야 할 수 있다.

##### 적용

(적용 불가능) 인터페이스가 ABI의 일부가 되는 부분을 확실하게 식별하기는 어렵다.

# <a name="S-functions"></a>F: 함수

함수는 어떤 동작이나 계산을 명세하는데, 하나의 상태에서 다른 상태로 일관성 있게 넘어가는 시스템이다. 함수는 프로그램의 기초적인 설계 단위다.

함수 이름은 인자의 요구사항, 인자간의 관계와 결과를 명확하게 기술할 수 있도록 의미있게 지어야 한다. 함수의 구현은 사양서가 아니다. 함수가 무엇을 해야하는지, 어떻게 동작하는지를 고려해야 한다. 함수는 모든 인터페이스에서 가장 중요한 부분이다. 인터페이스 규칙을 참고 하라.

함수 규칙 요약:

함수 정의 규칙:

* [F.1: 의미있는 동작들을 모아서 심사숙고해 함수 이름을 지어라](#Rf-package)
* [F.2: 함수는 하나의 논리적 동작만 수행하도록 하라](#Rf-logical)
* [F.3: 함수는 간결하고 단순하게 유지시켜라](#Rf-single)
* [F.4: 함수가 컴파일 타임에 평가되어야 한다면 `constexpr`로 선언하라](#Rf-constexpr)
* [F.5: 함수가 매우 짧고 수행시간이 중요하다면 inline으로 선언하라](#Rf-inline)
* [F.6: 함수가 예외를 던지지 않는다면 `noexcept`로 선언하라](#Rf-noexcept)
* [F.7: 보편성을 고려한다면, 스마트 포인터 대신에 `T*`나 `T&`형 인자를 사용하라](#Rf-smart)
* [F.8: 순수 함수를 선호하라](#Rf-pure)

매개 변수 전달 표현 규칙:

* [F.15: 정보를 전달 할 때 단순하고 관습적인 방법을 선호하라](#Rf-conventional)
* [F.16: "입력" 매개 변수는 복사 비용이 적게드는 값의 형을 사용하거나 상수 참조형으로 전달하라](#Rf-in)
* [F.17: "입출력" 매개 변수는 비상수 참조형으로 전달하라](#Rf-inout)
* [F.18: "소모성" 매개 변수는 `X&&`형과 `std::move`로 전달하라](#Rf-consume)
* [F.19: "forward" 매개 변수는 `TP&&`형과 `std::forward`로만 전달하라](#Rf-forward)
* [F.20: For "out" output values, 출력 매개 변수로 값을 반환하는 방법을 선호하라](#Rf-out)
* [F.21: "출력"값 여러개를 반환할 때는 튜플이나 구조체를 선호하라](#Rf-out-multi)
* [F.60: "인자가 없을 수도" 있다면 `T&`보다는 `T*`를 선호하라](#Rf-ptr-ref)

매개 변수 의미론적 규칙:

* [F.22: Use `T*` or `owner<T*>` or a smart pointer to designate a single object](#Rf-ptr)
* [F.23: Use a `not_null<T>` to indicate "null" is not a valid value](#Rf-nullptr)
* [F.24: Use a `span<T>` or a `span_p<T>` to designate a half-open sequence](#Rf-range)
* [F.25: Use a `zstring` or a `not_null<zstring>` to designate a C-style string](#Rf-string)
* [F.26: Use a `unique_ptr<T>` to transfer ownership where a pointer is needed](#Rf-unique_ptr)
* [F.27: Use a `shared_ptr<T>` to share ownership](#Rf-shared_ptr)

값 반환 의미론적 규칙:

* [F.42: Return a `T*` to indicate a position (only)](#Rf-return-ptr)
* [F.43: Never (directly or indirectly) return a pointer or a reference to a local object](#Rf-dangle)
* [F.44: Return a `T&` when copy is undesirable and "returning no object" isn't an option](#Rf-return-ref)
* [F.45: Don't return a `T&&`](#Rf-return-ref-ref)
* [F.46: `int` is the return type for `main()`](#Rf-main)
* [F.47: Return `T&` from assignment operators.](#Rf-assignment-op)

기타 함수 규칙:

* [F.50: Use a lambda when a function won't do (to capture local variables, or to write a local function)](#Rf-capture-vs-overload)
* [F.51: Where there is a choice, prefer default arguments over overloading](#Rf-default-args)
* [F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms](#Rf-reference-capture)
* [F.53: Avoid capturing by reference in lambdas that will be used nonlocally, including returned, stored on the heap, or passed to another thread](#Rf-value-capture)
* [F.54: If you capture `this`, capture all variables explicitly (no default capture)](#Rf-this-capture)

함수는 람다와 함수객체와 매우 유사하다. 그러니 ???을 참고하라.

## <a name="SS-fct-def"></a>F.def: 함수 정의

함수 정의는 함수를 선언하고 몸체를 구현하는 것을 포함한다.

### <a name="Rf-package"></a>F.1: 의미있는 동작들을 모아서 심사숙고해 함수 이름을 지어라

##### 이유

공통 코드를 만드는 것은 가독성, 재사용성을 높이고 복잡한 코드에서 오류 발생을 제한시킨다.
만약 어떤 동작이 잘 정의되어 있다면 주변 코드로부터 분리하고 이름을 지어라.

##### 나쁜 예제,

    void read_and_print(istream& is)    // read and print an int
    {
        int x;
        if (is >> x)
            cout << "the int is " << x << '\n';
        else
            cerr << "no int on input\n";
    }

`read_and_print`는 대부분이 틀렸다.
이 함수는 읽고, (`ostream`에) 쓰고, (`ostream`에) 오류 메시지를 쓴다. 그리고 `int`변수만을 다룬다.
재사용되는 코드가 없고 논리적으로 분리된 동작들은 섞였고 지역변수는 사용이 끝난 후에도 존재한다.
간단한 이 예제는 문제가 없어보이지만, 입력동작, 출력동작 그리고 오류처리가 좀 더 복잡해지면 코드가 뒤엉켜서 이해하기 어려워 진다.

##### 비고

만약 한 곳 이상에서 사용 될 중요한 람다 함수를 작성한다면 (비지역)변수에 할당하고 이름을 지어줘라.

##### 예제

    sort(a, b, [](T x, T y) { return x.rank() < y.rank() && x.value() < y.value(); });

람다에 이름을 짓게되면 표현식을 여러개의 논리적 부분으로 나눌 수 있고, 람다가 어떤 일을 하는지 가늠할 수 있다.

    auto lessT = [](T x, T y) { return x.rank() < y.rank() && x.value() < y.value(); };

    sort(a, b, lessT);
    find_if(a, b, lessT);

유지보수나 성능을 고려하면 짧은 코드가 항상 좋은것은 아니다.

##### 예외

반복문 몸체, 반복문 몸체로 사용되는 람다는 이름을 가질 필요가 거의 없다.
하지만 수 십라인이 넘거나 수 페이지가 된다면 문제가 될 수 있다.
[함수를 간결하게 유지하라](#Rf-single) 규칙은 "반복문을 간결하게 유지하라"는 규칙을 내포한다. 콜백 인자로 사용되는 람다는 재사용하지 않지만 사소하게 볼 수 없는 경우도 있다.

##### 적용

* [함수를 간결하게 유지하라](#Rf-single)를 참고하라.
* 여러곳에서 사용되는 동일하거나 매우 비슷한 람다는 표시해 두어라.

### <a name="Rf-logical"></a>F.2: 함수는 논리적으로 한 가지 작업만 수행해야 한다

##### 이유

하나의 작업만 수행하는 함수는 이해하기 쉽고, 테스트하기 쉽고, 재사용하기 쉽다.

##### 예제

다음을 고려해 보자:

    void read_and_print()    // bad
    {
        int x;
        cin >> x;
        // check for errors
        cout << x << "\n";
    }

위 함수는 특정한 입력에 속박되어 있고 다른 쓰임새는 찾을 수 없다. 대신, 함수를 의미있는 논리적 작업으로 나누고 매개 변수화하라.

    int read(istream& is)    // better
    {
        int x;
        is >> x;
        // check for errors
        return x;
    }

    void print(ostream& os, int x)
    {
        os << x << "\n";
    }

필요하다면 두 함수를 결합하면 된다:

    void read_and_print()
    {
        auto x = read(cin);
        print(cout, x);
    }

또한 필요하다면 `read()`와 `print()`에서 사용하는 데이터 타입, 입력 메커니즘, 오류에 대한 응답 등을 템플릿화 할 수 있다. 예를 들어:

    auto read = [](auto& input, auto& value)    // better
    {
        input >> value;
        // check for errors
    };

    auto print(auto& output, const auto& value)
    {
        output << value << "\n";
    }

##### 적용

* 출력 매개 변수가 2개 이상인 함수를 의심하라. 대신 반환값을 사용하라. 여러 반환값을 저장 할 수 있는 `tuple`을 사용해도 좋다. 
* 편집기 화면에 다 나오지 않을 만큼 큰 함수를 의심하라. 이런 함수는 세부 동작을 갖는 더 작은 함수들로 (이름을 잘 지어서) 나누도록 한다.
* 7개 이상의 매개 변수를 갖는 함수를 의심하라.

### <a name="Rf-single"></a>F.3: Keep functions short and simple

##### Reason

Large functions are hard to read, more likely to contain complex code, and more likely to have variables in larger than minimal scopes.
Functions with complex control structures are more likely to be long and more likely to hide logical errors

##### Example

Consider:

    double simpleFunc(double val, int flag1, int flag2)
        // simpleFunc: takes a value and calculates the expected ASIC output,
        // given the two mode flags.
    {
        double intermediate;
        if (flag1 > 0) {
            intermediate = func1(val);
            if (flag2 % 2)
                 intermediate = sqrt(intermediate);
        }
        else if (flag1 == -1) {
            intermediate = func1(-val);
            if (flag2 % 2)
                 intermediate = sqrt(-intermediate);
            flag1 = -flag1;
        }
        if (abs(flag2) > 10) {
            intermediate = func2(intermediate);
        }
        switch (flag2 / 10) {
            case 1: if (flag1 == -1) return finalize(intermediate, 1.171);
                    break;
            case 2: return finalize(intermediate, 13.1);
            default: break;
        }
        return finalize(intermediate, 0.);
    }

This is too complex (and also pretty long).
How would you know if all possible alternatives have been correctly handled?
Yes, it breaks other rules also.

We can refactor:

    double func1_muon(double val, int flag)
    {
        // ???
    }

    double funct1_tau(double val, int flag1, int flag2)
    {
        // ???
    }

    double simpleFunc(double val, int flag1, int flag2)
        // simpleFunc: takes a value and calculates the expected ASIC output,
        // given the two mode flags.
    {
        if (flag1 > 0)
            return func1_muon(val, flag2);
        if (flag1 == -1)
            // handled by func1_tau: flag1 = -flag1;
            return func1_tau(-val, flag1, flag2);
        return 0.;
    }

##### Note

"It doesn't fit on a screen" is often a good practical definition of "far too large."
One-to-five-line functions should be considered normal.

##### Note

Break large functions up into smaller cohesive and named functions.
Small simple functions are easily inlined where the cost of a function call is significant.

##### Enforcement

* Flag functions that do not "fit on a screen."
  How big is a screen? Try 60 lines by 140 characters; that's roughly the maximum that's comfortable for a book page.
* Flag functions that are too complex. How complex is too complex?
  You could use cyclomatic complexity. Try "more than 10 logical path through." Count a simple switch as one path.

### <a name="Rf-constexpr"></a>F.4: If a function may have to be evaluated at compile time, declare it `constexpr`

##### Reason

 `constexpr` is needed to tell the compiler to allow compile-time evaluation.

##### Example

The (in)famous factorial:

    constexpr int fac(int n)
    {
        constexpr int max_exp = 17;      // constexpr enables max_exp to be used in Expects
        Expects(0 <= n && n < max_exp);  // prevent silliness and overflow
        int x = 1;
        for (int i = 2; i <= n; ++i) x *= i;
        return x;
    }

This is C++14.
For C++11, use a recursive formulation of `fac()`.

##### Note

`constexpr` does not guarantee compile-time evaluation;
it just guarantees that the function can be evaluated at compile time for constant expression arguments if the programmer requires it or the compiler decides to do so to optimize.

    constexpr int min(int x, int y) { return x < y ? x : y; }

    void test(int v)
    {
        int m1 = min(-1, 2);            // probably compile-time evaluation
        constexpr int m2 = min(-1, 2);  // compile-time evaluation
        int m3 = min(-1, v);            // run-time evaluation
        constexpr int m4 = min(-1, v);  // error: cannot evaluate at compile-time
    }

##### Note

`constexpr` functions are pure: they can have no side effects.

    int dcount = 0;
    constexpr int double(int v)
    {
        ++dcount;   // error: attempted side effect from constexpr function
        return v + v;
    }

This is usually a very good thing.

When given a non-constant argument, a `constexpr` function can throw.
If you consider exiting by throwing a side-effect, a `constexpr` function isn't completely pure;
if not, this is not an issue.
??? A question for the committee: can a constructor for an exception thrown by a `constexpr` function modify state?
"No" would be a nice answer that matches most practice.

##### Note

Don't try to make all functions `constexpr`.
Most computation is best done at run time.

##### Note

Any API that may eventually depend on high-level runtime configuration or
business logic should not be made `constexpr`. Such customization can not be
evaluated by the compiler, and any `constexpr` functions that depend upon that
API will have to be refactored or drop `constexpr`.

##### Enforcement

Impossible and unnecessary.
The compiler gives an error if a non-`constexpr` function is called where a constant is required.

### <a name="Rf-inline"></a>F.5: If a function is very small and time-critical, declare it `inline`

##### Reason

Some optimizers are good at inlining without hints from the programmer, but don't rely on it.
Measure! Over the last 40 years or so, we have been promised compilers that can inline better than humans without hints from humans.
We are still waiting.
Specifying `inline` encourages the compiler to do a better job.

##### Example

    inline string cat(const string& s, const string& s2) { return s + s2; }

##### Exception

Do not put an `inline` function in what is meant to be a stable interface unless you are really sure that it will not change.
An inline function is part of the ABI.

##### Note

`constexpr` implies `inline`.

##### Note

Member functions defined in-class are `inline` by default.

##### Exception

Template functions (incl. template member functions) must be in headers and therefore inline.

##### Enforcement

Flag `inline` functions that are more than three statements and could have been declared out of line (such as class member functions).

### <a name="Rf-noexcept"></a>F.6: If your function may not throw, declare it `noexcept`

##### Reason

If an exception is not supposed to be thrown, the program cannot be assumed to cope with the error and should be terminated as soon as possible. Declaring a function `noexcept` helps optimizers by reducing the number of alternative execution paths. It also speeds up the exit after failure.

##### Example

Put `noexcept` on every function written completely in C or in any other language without exceptions.
The C++ standard library does that implicitly for all functions in the C standard library.

##### Note

`constexpr` functions cannot throw, so you don't need to use `noexcept` for those.

##### Example

You can use `noexcept` even on functions that can throw:

    vector<string> collect(istream& is) noexcept
    {
        vector<string> res;
        for (string s; is >> s;)
            res.push_back(s);
        return res;
    }

If `collect()` runs out of memory, the program crashes.
Unless the program is crafted to survive memory exhaustion, that may be just the right thing to do;
`terminate()` may generate suitable error log information (but after memory runs out it is hard to do anything clever).

##### Note

You must be aware of the execution environment that your code is running when
deciding whether to tag a function `noexcept`, especially because of the issue
of throwing and allocation.  Code that is intended to be perfectly general (like
the standard library and other utility code of that sort) needs to support
environments where a `bad_alloc` exception may be handled meaningfully.
However, the majority of programs and execution environments cannot meaningfully
handle a failure to allocate, and aborting the program is the cleanest and
simplest response to an allocation failure in those cases.  If you know that
your application code cannot respond to an allocation failure, it may be
appropriate to add `noexcept` even on functions that allocate.

Put another way: In most programs, most functions can throw (e.g., because they
use `new`, call functions that do, or use library functions that reports failure
by throwing), so don't just sprinkle `noexcept` all over the place without
considering whether the possible exceptions can be handled.

`noexcept` is most useful (and most clearly correct) for frequently used,
low-level functions.

##### Note

Destructors, `swap` functions, move operations, and default constructors should never throw.

##### Enforcement

* Flag functions that are not `noexcept`, yet cannot throw.
* Flag throwing `swap`, `move`, destructors, and default constructors.

### <a name="Rf-smart"></a>F.7: For general use, take `T*` or `T&` arguments rather than smart pointers

##### Reason

Passing a smart pointer transfers or shares ownership and should only be used when ownership semantics are intended (see [R.30](#Rr-smartptrparam)).
Passing by smart pointer restricts the use of a function to callers that use smart pointers.
Passing a shared smart pointer (e.g., `std::shared_ptr`) implies a run-time cost.

##### Example

    // accepts any int*
    void f(int*);

    // can only accept ints for which you want to transfer ownership
    void g(unique_ptr<int>);

    // can only accept ints for which you are willing to share ownership
    void g(shared_ptr<int>);

    // doesn't change ownership, but requires a particular ownership of the caller
    void h(const unique_ptr<int>&);

    // accepts any int
    void h(int&);

##### Example, bad

    // callee
    void f(shared_ptr<widget>& w)
    {
        // ...
        use(*w); // only use of w -- the lifetime is not used at all
        // ...
    };

See further in [R.30](#Rr-smartptrparam).

##### Note

We can catch dangling pointers statically, so we don't need to rely on resource management to avoid violations from dangling pointers.

**See also**: [when to prefer `T*` and when to prefer `T&`](#Rf-ptr-ref).

**See also**: Discussion of [smart pointer use](#Rr-summary-smartptrs).

##### Enforcement

Flag a parameter of a smart pointer type (a type that overloads `operator->` or `operator*`) for which the ownership semantics are not used;
that is

* copyable but never copied/moved from or movable but never moved
* and that is never modified or passed along to another function that could do so.

### <a name="Rf-pure"></a>F.8: Prefer pure functions

##### Reason

Pure functions are easier to reason about, sometimes easier to optimize (and even parallelize), and sometimes can be memoized.

##### Example

    template<class T>
    auto square(T t) { return t * t; }

##### Note

`constexpr` functions are pure.

When given a non-constant argument, a `constexpr` function can throw.
If you consider exiting by throwing a side-effect, a `constexpr` function isn't completely pure;
if not, this is not an issue.
??? A question for the committee: can a constructor for an exception thrown by a `constexpr` function modify state?
"No" would be a nice answer that matches most practice.

##### Enforcement

Not possible.

## <a name="SS-call"></a>F.call: Parameter passing

There are a variety of ways to pass parameters to a function and to return values.

### <a name="Rf-conventional"></a>F.15: Prefer simple and conventional ways of passing information

##### Reason

Using "unusual and clever" techniques causes surprises, slows understanding by other programmers, and encourages bugs.
If you really feel the need for an optimization beyond the common techniques, measure to ensure that it really is an improvement, and document/comment because the improvement may not be portable.

The following tables summarize the advice in the following Guidelines, F.16-21.

Normal parameter passing:

![Normal parameter passing table](./param-passing-normal.png "Normal parameter passing")

Advanced parameter passing:

![Advanced parameter passing table](./param-passing-advanced.png "Advanced parameter passing")

Use the advanced techniques only after demonstrating need, and document that need in a comment.

### <a name="Rf-in"></a>F.16: For "in" parameters, pass cheaply-copied types by value and others by reference to `const`

##### Reason

Both let the caller know that a function will not modify the argument, and both allow initialization by rvalues.

What is "cheap to copy" depends on the machine architecture, but two or three words (doubles, pointers, references) are usually best passed by value.
When copying is cheap, nothing beats the simplicity and safety of copying, and for small objects (up to two or three words) it is also faster than passing by reference because it does not require an extra indirection to access from the function.

##### Example

    void f1(const string& s);  // OK: pass by reference to const; always cheap

    void f2(string s);         // bad: potentially expensive

    void f3(int x);            // OK: Unbeatable

    void f4(const int& x);     // bad: overhead on access in f4()

For advanced uses (only), where you really need to optimize for rvalues passed to "input-only" parameters:

* If the function is going to unconditionally move from the argument, take it by `&&`. See [F.18](#Rf-consume).
* If the function is going to keep a copy of the argument, in addition to passing by `const&` (for lvalues),
  add an overload that passes the parameter by `&&` (for rvalues) and in the body `std::move`s it to its destination. Essentially this overloads a "consume"; see [F.18](#Rf-consume).
* In special cases, such as multiple "input + copy" parameters, consider using perfect forwarding. See [F.19](#Rf-forward).

##### Example

    int multiply(int, int); // just input ints, pass by value

    // suffix is input-only but not as cheap as an int, pass by const&
    string& concatenate(string&, const string& suffix);

    void sink(unique_ptr<widget>);  // input only, and consumes the widget

Avoid "esoteric techniques" such as:

* Passing arguments as `T&&` "for efficiency".
  Most rumors about performance advantages from passing by `&&` are false or brittle (but see [F.25](#Rf-pass-ref-move).)
* Returning `const T&` from assignments and similar operations (see [F.47](#Rf-assignment-op).)

##### Example

Assuming that `Matrix` has move operations (possibly by keeping its elements in a `std::vector`):

    Matrix operator+(const Matrix& a, const Matrix& b)
    {
        Matrix res;
        // ... fill res with the sum ...
        return res;
    }

    Matrix x = m1 + m2;  // move constructor

    y = m3 + m3;         // move assignment

##### Notes

The return value optimization doesn't handle the assignment case, but the move assignment does.

A reference may be assumed to refer to a valid object (language rule).
There is no (legitimate) "null reference."
If you need the notion of an optional value, use a pointer, `std::optional`, or a special value used to denote "no value."

##### Enforcement

* (Simple) ((Foundation)) Warn when a parameter being passed by value has a size greater than `4 * sizeof(int)`.
  Suggest using a reference to `const` instead.
* (Simple) ((Foundation)) Warn when a `const` parameter being passed by reference has a size less than `3 * sizeof(int)`. Suggest passing by value instead.
* (Simple) ((Foundation)) Warn when a `const` parameter being passed by reference is `move`d.

### <a name="Rf-inout"></a>F.17: For "in-out" parameters, pass by reference to non-`const`

##### Reason

This makes it clear to callers that the object is assumed to be modified.

##### Example

    void update(Record& r);  // assume that update writes to r

##### Note

A `T&` argument can pass information into a function as well as well as out of it.
Thus `T&` could be an in-out-parameter. That can in itself be a problem and a source of errors:

    void f(string& s)
    {
        s = "New York";  // non-obvious error
    }

    void g()
    {
        string buffer = ".................................";
        f(buffer);
        // ...
    }

Here, the writer of `g()` is supplying a buffer for `f()` to fill, but `f()` simply replaces it (at a somewhat higher cost than a simple copy of the characters).
If the writer of `g()` makes an assumption about the size of `buffer` a bad logic error can happen.

##### Enforcement

* (Moderate) ((Foundation)) Warn about functions with reference to non-`const` parameters that do *not* write to them.
* (Simple) ((Foundation)) Warn when a non-`const` parameter being passed by reference is `move`d.

### <a name="Rf-consume"></a>F.18: For "consume" parameters, pass by `X&&` and `std::move` the parameter

##### Reason

It's efficient and eliminates bugs at the call site: `X&&` binds to rvalues, which requires an explicit `std::move` at the call site if passing an lvalue.

##### Example

    void sink(vector<int>&& v) {   // sink takes ownership of whatever the argument owned
        // usually there might be const accesses of v here
        store_somewhere(std::move(v));
        // usually no more use of v here; it is moved-from
    }

Note that the `std::move(v)` makes it possible for `store_somewhere()` to leave `v` in a moved-from state.
[That could be dangerous](#Rc-move-semantic).


##### Exception

Unique owner types that are move-only and cheap-to-move, such as `unique_ptr`, can also be passed by value which is simpler to write and achieves the same effect. Passing by value does generate one extra (cheap) move operation, but prefer simplicity and clarity first.

For example:

    template <class T>
    void sink(std::unique_ptr<T> p) {
        // use p ... possibly std::move(p) onward somewhere else
    }   // p gets destroyed

##### Enforcement

* Flag all `X&&` parameters (where `X` is not a template type parameter name) where the function body uses them without `std::move`.
* Flag access to moved-from objects.
* Don't conditionally move from objects

### <a name="Rf-forward"></a>F.19: For "forward" parameters, pass by `TP&&` and only `std::forward` the parameter

##### Reason

If the object is to be passed onward to other code and not directly used by this function, we want to make this function agnostic to the argument `const`-ness and rvalue-ness.

In that case, and only that case, make the parameter `TP&&` where `TP` is a template type parameter -- it both *ignores* and *preserves* `const`-ness and rvalue-ness. Therefore any code that uses a `TP&&` is implicitly declaring that it itself doesn't care about the variable's `const`-ness and rvalue-ness (because it is ignored), but that intends to pass the value onward to other code that does care about `const`-ness and rvalue-ness (because it is preserved). When used as a parameter `TP&&` is safe because any temporary objects passed from the caller will live for the duration of the function call. A parameter of type `TP&&` should essentially always be passed onward via `std::forward` in the body of the function.

##### Example

    template <class F, class... Args>
    inline auto invoke(F f, Args&&... args) {
        return f(forward<Args>(args)...);
    }

    ??? calls ???

##### Enforcement

* Flag a function that takes a `TP&&` parameter (where `TP` is a template type parameter name) and does anything with it other than `std::forward`ing it exactly once on every static path.

### <a name="Rf-out"></a>F.20: For "out" output values, prefer return values to output parameters

##### Reason

A return value is self-documenting, whereas a `&` could be either in-out or out-only and is liable to be misused.

This includes large objects like standard containers that use implicit move operations for performance and to avoid explicit memory management.

If you have multiple values to return, [use a tuple](#Rf-out-multi) or similar multi-member type.

##### Example

    // OK: return pointers to elements with the value x
    vector<const int*> find_all(const vector<int>&, int x);

    // Bad: place pointers to elements with value x in out
    void find_all(const vector<int>&, vector<const int*>& out, int x);

##### Note

A `struct` of many (individually cheap-to-move) elements may be in aggregate expensive to move.

It is not recommended to return a `const` value.
Such older advice is now obsolete; it does not add value, and it interferes with move semantics.

    const vector<int> fct();    // bad: that "const" is more trouble than it is worth

    vector<int> g(const vector<int>& vx)
    {
        // ...
        f() = vx;   // prevented by the "const"
        // ...
        return f(); // expensive copy: move semantics suppressed by the "const"
    }

The argument for adding `const` to a return value is that it prevents (very rare) accidental access to a temporary.
The argument against is prevents (very frequent) use of move semantics.

##### Exceptions

* For non-value types, such as types in an inheritance hierarchy, return the object by `unique_ptr` or `shared_ptr`.
* If a type is expensive to move (e.g., `array<BigPOD>`), consider allocating it on the free store and return a handle (e.g., `unique_ptr`), or passing it in a reference to non-`const` target object to fill (to be used as an out-parameter).
* To reuse an object that carries capacity (e.g., `std::string`, `std::vector`) across multiple calls to the function in an inner loop: [treat it as an in/out parameter and pass by reference](#Rf-out-multi).

##### Example

    struct Package {      // exceptional case: expensive-to-move object
        char header[16];
        char load[2024 - 16];
    };

    Package fill();       // Bad: large return value
    void fill(Package&);  // OK

    int val();            // OK
    void val(int&);       // Bad: Is val reading its argument

##### Enforcement

* Flag reference to non-`const` parameters that are not read before being written to and are a type that could be cheaply returned; they should be "out" return values.
* Flag returning a `const` value. To fix: Remove `const` to return a non-`const` value instead.

### <a name="Rf-out-multi"></a>F.21: To return multiple "out" values, prefer returning a tuple or struct

##### Reason

A return value is self-documenting as an "output-only" value.
Note that C++ does have multiple return values, by convention of using a `tuple`,
possibly with the extra convenience of `tie` at the call site.

##### Example

    // BAD: output-only parameter documented in a comment
    int f(const string& input, /*output only*/ string& output_data)
    {
        // ...
        output_data = something();
        return status;
    }

    // GOOD: self-documenting
    tuple<int, string> f(const string& input)
    {
        // ...
        return make_tuple(status, something());
    }

C++98's standard library already used this style, because a `pair` is like a two-element `tuple`.
For example, given a `set<string> my_set`, consider:

    // C++98
    result = my_set.insert("Hello");
    if (result.second) do_something_with(result.first);    // workaround

With C++11 we can write this, putting the results directly in existing local variables:

    Sometype iter;                                // default initialize if we haven't already
    Someothertype success;                        // used these variables for some other purpose

    tie(iter, success) = my_set.insert("Hello");   // normal return value
    if (success) do_something_with(iter);

With C++17 we should be able to use "structured bindings" to declare and initialize the multiple variables:

    if (auto [ iter, success ] = my_set.insert("Hello"); success) do_something_with(iter);

##### Exception

Sometimes, we need to pass an object to a function to manipulate its state.
In such cases, passing the object by reference [`T&`](#Rf-inout) is usually the right technique.
Explicitly passing an in-out parameter back out again as a return value is often not necessary.
For example:

    istream& operator>>(istream& is, string& s);    // much like std::operator>>()

    for (string s; cin >> s; ) {
        // do something with line
    }

Here, both `s` and `cin` are used as in-out parameters.
We pass `cin` by (non-`const`) reference to be able to manipulate its state.
We pass `s` to avoid repeated allocations.
By reusing `s` (passed by reference), we allocate new memory only when we need to expand `s`'s capacity.
This technique is sometimes called the "caller-allocated out" pattern and is particularly useful for types,
such as `string` and `vector`, that needs to do free store allocations.

To compare, if we passed out all values as return values, we would something like this:

    pair<istream&, string> get_string(istream& is);  // not recommended
    {
        string s;
        cin >> s;
        return {is, s};
    }

    for (auto p = get_string(cin); p.first; ) {
        // do something with p.second
    }

We consider that significantly less elegant and definitely significantly slower.

For a really strict reading this rule (F.21), the exceptions isn't really an exception because it relies on in-out parameters,
rather than the plain out parameters mentioned in the rule.
However, we prefer to be explicit, rather than subtle.

##### Note

In many cases it may be useful to return a specific, user-defined "Value or error" type.
For example:

    struct

The overly-generic `pair` and `tuple` should be used only when the value returned represents to independent entities rather than an abstraction.

type along the lines of `variant<T, error_code>`, rather than using the generic `tuple`.

##### Enforcement

* Output parameters should be replaced by return values.
  An output parameter is one that the function writes to, invokes a non-`const` member function, or passes on as a non-`const`.

### <a name="Rf-ptr"></a>F.22: Use `T*` or `owner<T*>` to designate a single object

##### Reason

Readability: it makes the meaning of a plain pointer clear.
Enables significant tool support.

##### Note

In traditional C and C++ code, plain `T*` is used for many weakly-related purposes, such as:

* Identify a (single) object (not to be deleted by this function)
* Point to an object allocated on the free store (and delete it later)
* Hold the `nullptr`
* Identify a C-style string (zero-terminated array of characters)
* Identify an array with a length specified separately
* Identify a location in an array

The makes it hard to understand what code does and is supposed to do.
It complicates checking and tool support.

##### Example

    void use(int* p, int n, char* s, int* q)
    {
        p[n - 1] = 666; // Bad: we don't know if p points to n elements;
                        // assume it does not or use span<int>
        cout << s;      // Bad: we don't know if that s points to a zero-terminated array of char;
                        // assume it does not or use zstring
        delete q;       // Bad: we don't know if *q is allocated on the free store;
                        // assume it does not or use owner
    }

better

    void use2(span<int> p, zstring s, owner<int*> q)
    {
        p[p.size() - 1] = 666; // OK, a range error can be caught
        cout << s; // OK
        delete q;  // OK
    }

##### Note

`owner<T*>` represents ownership, `zstring` represents a C-style string.

**Also**: Assume that a `T*` obtained from a smart pointer to `T` (e.g., `unique_ptr<T>`) points to a single element.

**See also**: [Support library](#S-gsl).

##### Enforcement

* (Simple) ((Bounds)) Warn for any arithmetic operation on an expression of pointer type that results in a value of pointer type.

### <a name="Rf-nullptr"></a>F.23: Use a `not_null<T>` to indicate that "null" is not a valid value

##### Reason

Clarity. A function with a `not_null<T>` parameter makes it clear that the caller of the function is responsible for any `nullptr` checks that may be necessary.
Similarly, a function with a return value of `not_null<T>` makes it clear that the caller of the function does not need to check for `nullptr`.

##### Example

`not_null<T*>` makes it obvious to a reader (human or machine) that a test for `nullptr` is not necessary before dereference.
Additionally, when debugging, `owner<T*>` and `not_null<T>` can be instrumented to check for correctness.

Consider:

    int length(Record* p);

When I call `length(p)` should I test for `p == nullptr` first? Should the implementation of `length()` test for `p == nullptr`?

    // it is the caller's job to make sure p != nullptr
    int length(not_null<Record*> p);

    // the implementor of length() must assume that p == nullptr is possible
    int length(Record* p);

##### Note

A `not_null<T*>` is assumed not to be the `nullptr`; a `T*` may be the `nullptr`; both can be represented in memory as a `T*` (so no run-time overhead is implied).

##### Note

`not_null` is not just for built-in pointers. It works for `unique_ptr`, `shared_ptr`, and other pointer-like types.

##### Enforcement

* (Simple) Warn if a raw pointer is dereferenced without being tested against `nullptr` (or equivalent) within a function, suggest it is declared `not_null` instead.
* (Simple) Error if a raw pointer is sometimes dereferenced after first being tested against `nullptr` (or equivalent) within the function and sometimes is not.
* (Simple) Warn if a `not_null` pointer is tested against `nullptr` within a function.

### <a name="Rf-range"></a>F.24: Use a `span<T>` or a `span_p<T>` to designate a half-open sequence

##### Reason

Informal/non-explicit ranges are a source of errors.

##### Example

    X* find(span<X> r, const X& v);    // find v in r

    vector<X> vec;
    // ...
    auto p = find({vec.begin(), vec.end()}, X{});  // find X{} in vec

##### Note

Ranges are extremely common in C++ code. Typically, they are implicit and their correct use is very hard to ensure.
In particular, given a pair of arguments `(p, n)` designating an array \[`p`:`p+n`),
it is in general impossible to know if there really are `n` elements to access following `*p`.
`span<T>` and `span_p<T>` are simple helper classes designating a \[`p`:`q`) range and a range starting with `p` and ending with the first element for which a predicate is true, respectively.

##### Example

A `span` represents a range of elements, but how do we manipulate elements of that range?

    void f(span<int> s)
    {
        // range traversal (guaranteed correct)
        for (int x : s) cout << x << '\n';

        // C-style traversal (potentially checked)
        for (int i = 0; i < s.size(); ++i) cout << s[i] << '\n';

        // random access (potentially checked)
        s[7] = 9;

        // extract pointers (potentially checked)
        std::sort(&s[0], &s[s.size() / 2]);
    }

##### Note

A `span<T>` object does not own its elements and is so small that it can be passed by value.

Passing a `span` object as an argument is exactly as efficient as passing a pair of pointer arguments or passing a pointer and an integer count.

**See also**: [Support library](#S-gsl).

##### Enforcement

(Complex) Warn where accesses to pointer parameters are bounded by other parameters that are integral types and suggest they could use `span` instead.

### <a name="Rf-string"></a>F.25: Use a `zstring` or a `not_null<zstring>` to designate a C-style string

##### Reason

C-style strings are ubiquitous. They are defined by convention: zero-terminated arrays of characters.
We must distinguish C-style strings from a pointer to a single character or an old-fashioned pointer to an array of characters.

##### Example

Consider:

    int length(const char* p);

When I call `length(s)` should I test for `s == nullptr` first? Should the implementation of `length()` test for `p == nullptr`?

    // the implementor of length() must assume that p == nullptr is possible
    int length(zstring p);

    // it is the caller's job to make sure p != nullptr
    int length(not_null<zstring> p);

##### Note

`zstring` do not represent ownership.

**See also**: [Support library](#S-gsl).

### <a name="Rf-unique_ptr"></a>F.26: Use a `unique_ptr<T>` to transfer ownership where a pointer is needed

##### Reason

Using `unique_ptr` is the cheapest way to pass a pointer safely.

##### Example

    unique_ptr<Shape> get_shape(istream& is)  // assemble shape from input stream
    {
        auto kind = read_header(is); // read header and identify the next shape on input
        switch (kind) {
        case kCircle:
            return make_unique<Circle>(is);
        case kTriangle:
            return make_unique<Triangle>(is);
        // ...
        }
    }

##### Note

You need to pass a pointer rather than an object if what you are transferring is an object from a class hierarchy that is to be used through an interface (base class).

##### Enforcement

(Simple) Warn if a function returns a locally-allocated raw pointer. Suggest using either `unique_ptr` or `shared_ptr` instead.

### <a name="Rf-shared_ptr"></a>F.27: Use a `shared_ptr<T>` to share ownership

##### Reason

Using `std::shared_ptr` is the standard way to represent shared ownership. That is, the last owner deletes the object.

##### Example

    shared_ptr<const Image> im { read_image(somewhere) };

    std::thread t0 {shade, args0, top_left, im};
    std::thread t1 {shade, args1, top_right, im};
    std::thread t2 {shade, args2, bottom_left, im};
    std::thread t3 {shade, args3, bottom_right, im};

    // detach threads
    // last thread to finish deletes the image

##### Note

Prefer a `unique_ptr` over a `shared_ptr` if there is never more than one owner at a time.
`shared_ptr` is for shared ownership.

Note that pervasive use of `shared_ptr` has a cost (atomic operations on the `shared_ptr`'s reference count have a measurable aggregate cost).

##### Alternative

Have a single object own the shared object (e.g. a scoped object) and destroy that (preferably implicitly) when all users have completed.

##### Enforcement

(Not enforceable) This is a too complex pattern to reliably detect.

### <a name="Rf-ptr-ref"></a>F.60: Prefer `T*` over `T&` when "no argument" is a valid option

##### Reason

A pointer (`T*`) can be a `nullptr` and a reference (`T&`) cannot, there is no valid "null reference".
Sometimes having `nullptr` as an alternative to indicated "no object" is useful, but if it is not, a reference is notationally simpler and might yield better code.

##### Example

    string zstring_to_string(zstring p) // zstring is a char*; that is a C-style string
    {
        if (p == nullptr) return string{};    // p might be nullptr; remember to check
        return string{p};
    }

    void print(const vector<int>& r)
    {
        // r refers to a vector<int>; no check needed
    }

##### Note

It is possible, but not valid C++ to construct a reference that is essentially a `nullptr` (e.g., `T* p = nullptr; T& r = (T&)*p;`).
That error is very uncommon.

##### Note

If you prefer the pointer notation (`->` and/or `*` vs. `.`), `not_null<T*>` provides the same guarantee as `T&`.

##### Enforcement

* Flag ???

### <a name="Rf-return-ptr"></a>F.42: Return a `T*` to indicate a position (only)

##### Reason

That's what pointers are good for.
Returning a `T*` to transfer ownership is a misuse.

##### Example

    Node* find(Node* t, const string& s)  // find s in a binary tree of Nodes
    {
        if (t == nullptr || t->name == s) return t;
        if ((auto p = find(t->left, s))) return p;
        if ((auto p = find(t->right, s))) return p;
        return nullptr;
    }

If it isn't the `nullptr`, the pointer returned by `find` indicates a `Node` holding `s`.
Importantly, that does not imply a transfer of ownership of the pointed-to object to the caller.

##### Note

Positions can also be transferred by iterators, indices, and references.
A reference is often a superior alternative to a pointer [if there is no need to use `nullptr`](#Rf-ptr-ref) or [if the object referred to should not change](???).

##### Note

Do not return a pointer to something that is not in the caller's scope; see [F.43](#Rf-dangle).

**See also**: [discussion of dangling pointer prevention](#???).

##### Enforcement

* Flag `delete`, `std::free()`, etc. applied to a plain `T*`.
Only owners should be deleted.
* Flag `new`, `malloc()`, etc. assigned to a plain `T*`.
Only owners should be responsible for deletion.

### <a name="Rf-dangle"></a>F.43: Never (directly or indirectly) return a pointer or a reference to a local object

##### Reason

To avoid the crashes and data corruption that can result from the use of such a dangling pointer.

##### Example, bad

After the return from a function its local objects no longer exist:

    int* f()
    {
        int fx = 9;
        return &fx;  // BAD
    }

    void g(int* p)   // looks innocent enough
    {
        int gx;
        cout << "*p == " << *p << '\n';
        *p = 999;
        cout << "gx == " << gx << '\n';
    }

    void h()
    {
        int* p = f();
        int z = *p;  // read from abandoned stack frame (bad)
        g(p);        // pass pointer to abandoned stack frame to function (bad)
    }

Here on one popular implementation I got the output:

    *p == 999
    gx == 999

I expected that because the call of `g()` reuses the stack space abandoned by the call of `f()` so `*p` refers to the space now occupied by `gx`.

* Imagine what would happen if `fx` and `gx` were of different types.
* Imagine what would happen if `fx` or `gx` was a type with an invariant.
* Imagine what would happen if more that dangling pointer was passed around among a larger set of functions.
* Imagine what a cracker could do with that dangling pointer.

Fortunately, most (all?) modern compilers catch and warn against this simple case.

##### Note

This applies to references as well:

    int& f()
    {
        int x = 7;
        // ...
        return x;  // Bad: returns reference to object that is about to be destroyed
    }

##### Note

This applies only to non-`static` local variables.
All `static` variables are (as their name indicates) statically allocated, so that pointers to them cannot dangle.

##### Example, bad

Not all examples of leaking a pointer to a local variable are that obvious:

    int* glob;       // global variables are bad in so many ways

    template<class T>
    void steal(T x)
    {
        glob = x();  // BAD
    }

    void f()
    {
        int i = 99;
        steal([&] { return &i; });
    }

    int main()
    {
        f();
        cout << *glob << '\n';
    }

Here I managed to read the location abandoned by the call of `f`.
The pointer stored in `glob` could be used much later and cause trouble in unpredictable ways.

##### Note

The address of a local variable can be "returned"/leaked by a return statement, by a `T&` out-parameter, as a member of a returned object, as an element of a returned array, and more.

##### Note

Similar examples can be constructed "leaking" a pointer from an inner scope to an outer one;
such examples are handled equivalently to leaks of pointers out of a function.

A slightly different variant of the problem is placing pointers in a container that outlives the objects pointed to.

**See also**: Another way of getting dangling pointers is [pointer invalidation](#???).
It can be detected/prevented with similar techniques.

##### Enforcement

* Compilers tend to catch return of reference to locals and could in many cases catch return of pointers to locals.
* Static analysis can catch many common patterns of the use of pointers indicating positions (thus eliminating dangling pointers)

### <a name="Rf-return-ref"></a>F.44: Return a `T&` when copy is undesirable and "returning no object" isn't needed

##### Reason

The language guarantees that a `T&` refers to an object, so that testing for `nullptr` isn't necessary.

**See also**: The return of a reference must not imply transfer of ownership:
[discussion of dangling pointer prevention](#???) and [discussion of ownership](#???).

##### Example

    class Car
    {
        array<wheel, 4> w;
        // ...
    public:
        wheel& get_wheel(size_t i) { Expects(i < 4); return w[i]; }
        // ...
    };

    void use()
    {
        Car c;
        wheel& w0 = c.get_wheel(0); // w0 has the same lifetime as c
    }

##### Enforcement

Flag functions where no `return` expression could yield `nullptr`

### <a name="Rf-return-ref-ref"></a>F.45: Don't return a `T&&`

##### Reason

It's asking to return a reference to a destroyed temporary object. A `&&` is a magnet for temporary objects. This is fine when the reference to the temporary is being passed "downward" to a callee, because the temporary is guaranteed to outlive the function call. (See [F.24](#Rf-pass-ref-ref) and [F.25](#Rf-pass-ref-move).) However, it's not fine when passing such a reference "upward" to a larger caller scope. See also ???.

For passthrough functions that pass in parameters (by ordinary reference or by perfect forwarding) and want to return values, use simple `auto` return type deduction (not `auto&&`).

##### Example, bad

If `F` returns by value, this function returns a reference to a temporary.

    template<class F>
    auto&& wrapper(F f)
    {
        log_call(typeid(f)); // or whatever instrumentation
        return f();
    }

##### Example, good

Better:

    template<class F>
    auto wrapper(F f)
    {
        log_call(typeid(f)); // or whatever instrumentation
        return f();
    }

##### Exception

`std::move` and `std::forward` do return `&&`, but they are just casts -- used by convention only in expression contexts where a reference to a temporary object is passed along within the same expression before the temporary is destroyed. We don't know of any other good examples of returning `&&`.

##### Enforcement

Flag any use of `&&` as a return type, except in `std::move` and `std::forward`.

### <a name="Rf-main"></a>F.46: `int` is the return type for `main()`

##### Reason

It's a language rule, but violated through "language extensions" so often that it is worth mentioning.
Declaring `main` (the one global `main` of a program) `void` limits portability.

##### Example

        void main() { /* ... */ };  // bad, not C++

        int main()
        {
            std::cout << "This is the way to do it\n";
        }

##### Note

We mention this only because of the persistence of this error in the community.

##### Enforcement

* The compiler should do it
* If the compiler doesn't do it, let tools flag it

### <a name="Rf-assignment-op"></a>F.47: Return `T&` from assignment operators

##### Reason

The convention for operator overloads (especially on value types) is for
`operator=(const T&)` to perform the assignment and then return (non-const)
`*this`.  This ensures consistency with standard library types and follows the
principle of "do as the ints do."

##### Note

Historically there was some guidance to make the assignment operator return `const T&`.
This was primarily to avoid code of the form `(a = b) = c` -- such code is not common enough to warrant violating consistency with standard types.

##### Example

    class Foo
    {
     public:
        ...
        Foo& operator=(const Foo& rhs) {
          // Copy members.
          ...
          return *this;
        }
    };

##### Enforcement

This should be enforced by tooling by checking the return type (and return
value) of any assignment operator.

### <a name="Rf-capture-vs-overload"></a>F.50: Use a lambda when a function won't do (to capture local variables, or to write a local function)

##### Reason

Functions can't capture local variables or be declared at local scope; if you need those things, prefer a lambda where possible, and a handwritten function object where not. On the other hand, lambdas and function objects don't overload; if you need to overload, prefer a function (the workarounds to make lambdas overload are ornate). If either will work, prefer writing a function; use the simplest tool necessary.

##### Example

    // writing a function that should only take an int or a string
    // -- overloading is natural
    void f(int);
    void f(const string&);

    // writing a function object that needs to capture local state and appear
    // at statement or expression scope -- a lambda is natural
    vector<work> v = lots_of_work();
    for (int tasknum = 0; tasknum < max; ++tasknum) {
        pool.run([=, &v]{
            /*
            ...
            ... process 1 / max - th of v, the tasknum - th chunk
            ...
            */
        });
    }
    pool.join();

##### Exception

Generic lambdas offer a concise way to write function templates and so can be useful even when a normal function template would do equally well with a little more syntax. This advantage will probably disappear in the future once all functions gain the ability to have Concept parameters.

##### Enforcement

* Warn on use of a named non-generic lambda (e.g., `auto x = [](int i){ /*...*/; };`) that captures nothing and appears at global scope. Write an ordinary function instead.

### <a name="Rf-default-args"></a>F.51: Where there is a choice, prefer default arguments over overloading

##### Reason

Default arguments simply provides alternative interfaces to a single implementation.
There is no guarantee that a set of overloaded functions all implement the same semantics.
The use of default arguments can avoid code replication.

##### Note

There is a choice between using default argument and overloading when the alternatives are from a set of arguments of the same types.
For example:

    void print(const string& s, format f = {});

as opposed to

    void print(const string& s);  // use default format
    void print(const string& s, format f);

There is not a choice when a set of functions are used to do a semantically equivalent operation to a set of types. For example:

    void print(const char&);
    void print(int);
    void print(zstring);

##### See also


[Default arguments for virtual functions](#Rh-virtual-default-arg)

##### Enforcement

    ???

### <a name="Rf-reference-capture"></a>F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms

##### Reason

For efficiency and correctness, you nearly always want to capture by reference when using the lambda locally. This includes when writing or calling parallel algorithms that are local because they join before returning.

##### Example

This is a simple three-stage parallel pipeline. Each `stage` object encapsulates a worker thread and a queue, has a `process` function to enqueue work, and in its destructor automatically blocks waiting for the queue to empty before ending the thread.

    void send_packets(buffers& bufs)
    {
        stage encryptor([] (buffer& b){ encrypt(b); });
        stage compressor([&](buffer& b){ compress(b); encryptor.process(b); });
        stage decorator([&](buffer& b){ decorate(b); compressor.process(b); });
        for (auto& b : bufs) { decorator.process(b); }
    }  // automatically blocks waiting for pipeline to finish

##### Enforcement

???

### <a name="Rf-value-capture"></a>F.53: Avoid capturing by reference in lambdas that will be used nonlocally, including returned, stored on the heap, or passed to another thread

##### Reason

Pointers and references to locals shouldn't outlive their scope. Lambdas that capture by reference are just another place to store a reference to a local object, and shouldn't do so if they (or a copy) outlive the scope.

##### Example, bad

    int local = 42;

    // Want a reference to local.
    // Note, that after program exits this scope,
    // local no longer exists, therefore
    // process() call will have undefined behavior!
    thread_pool.queue_work([&]{ process(local); });

##### Example, good

    int local = 42;
    // Want a copy of local.
    // Since a copy of local is made, it will be
    // available at all times for the call.
    thread_pool.queue_work([=]{ process(local); });

##### Enforcement

* (Simple) Warn when capture-list contains a reference to a locally declared variable
* (Complex) Flag when capture-list contains a reference to a locally declared variable and the lambda is passed to a non-`const` and non-local context

### <a name="Rf-this-capture"></a>F.54: If you capture `this`, capture all variables explicitly (no default capture)

##### Reason

It's confusing. Writing `[=]` in a member function appears to capture by value, but actually captures data members by reference because it actually captures the invisible `this` pointer by value. If you meant to do that, write `this` explicitly.

##### Example

    class My_class {
        int x = 0;
        // ...

        void f() {
            int i = 0;
            // ...

            auto lambda = [=]{ use(i, x); };   // BAD: "looks like" copy/value capture
            // [&] has identical semantics and copies the this pointer under the current rules
            // [=,this] and [&,this] are not much better, and confusing

            x = 42;
            lambda(); // calls use(42);
            x = 43;
            lambda(); // calls use(43);

            // ...

            auto lambda2 = [i, this]{ use(i, x); }; // ok, most explicit and least confusing

            // ...
        }
    };

##### Note

This is under active discussion in standardization, and may be addressed in a future version of the standard by adding a new capture mode or possibly adjusting the meaning of `[=]`. For now, just be explicit.

##### Enforcement

* Flag any lambda capture-list that specifies a default capture and also captures `this` (whether explicitly or via default capture)

# <a name="S-class"></a>C: Classes and Class Hierarchies

A class is a user-defined type, for which a programmer can define the representation, operations, and interfaces.
Class hierarchies are used to organize related classes into hierarchical structures.

Class rule summary:

* [C.1: Organize related data into structures (`struct`s or `class`es)](#Rc-org)
* [C.2: Use `class` if the class has an invariant; use `struct` if the data members can vary independently](#Rc-struct)
* [C.3: Represent the distinction between an interface and an implementation using a class](#Rc-interface)
* [C.4: Make a function a member only if it needs direct access to the representation of a class](#Rc-member)
* [C.5: Place helper functions in the same namespace as the class they support](#Rc-helper)
* [C.7: Don't define a class or enum and declare a variable of its type in the same statement](#Rc-standalone)
* [C.8: use `class` rather that `struct` if any member is non-public](#Rc-class)
* [C.9: minimize exposure of members](#Rc-private)

Subsections:

* [C.concrete: Concrete types](#SS-concrete)
* [C.ctor: Constructors, assignments, and destructors](#S-ctor)
* [C.con: Containers and other resource handles](#SS-containers)
* [C.lambdas: Function objects and lambdas](#SS-lambdas)
* [C.hier: Class hierarchies (OOP)](#SS-hier)
* [C.over: Overloading and overloaded operators](#SS-overload)
* [C.union: Unions](#SS-union)

### <a name="Rc-org"></a>C.1: Organize related data into structures (`struct`s or `class`es)

##### Reason

Ease of comprehension. If data is related (for fundamental reasons), that fact should be reflected in code.

##### Example

    void draw(int x, int y, int x2, int y2);  // BAD: unnecessary implicit relationships
    void draw(Point from, Point to);          // better

##### Note

A simple class without virtual functions implies no space or time overhead.

##### Note

From a language perspective `class` and `struct` differ only in the default visibility of their members.

##### Enforcement

Probably impossible. Maybe a heuristic looking for data items used together is possible.

### <a name="Rc-struct"></a>C.2: Use `class` if the class has an invariant; use `struct` if the data members can vary independently

##### Reason

Readability.
Ease of comprehension.
The use of `class` alerts the programmer to the need for an invariant.
This is a useful convention.

##### Note

An invariant is a logical condition for the members of an object that a constructor must establish for the public member functions to assume.
After the invariant is established (typically by a constructor) every member function can be called for the object.
An invariant can be stated informally (e.g., in a comment) or more formally using `Expects`.

If all data members can vary independently of each other, no invariant is possible.

##### Example

    struct Pair {  // the members can vary independently
        string name;
        int volume;
    };

but:

    class Date {
    public:
        // validate that {yy, mm, dd} is a valid date and initialize
        Date(int yy, Month mm, char dd);
        // ...
    private:
        int y;
        Month m;
        char d;    // day
    };

##### Note

If a class has any `private` data, a user cannot completely initialize an object without the use of a constructor.
Hence, the class definer will provide a constructor and must specify its meaning.
This effectively means the definer need to define an invariant.

* See also [define a class with private data as `class`](#Rc-class).
* See also [Prefer to place the interface first in a class](#Rl-order).
* See also [minimize exposure of members](#Rc-private).
* See also [Avoid `protected` data](#Rh-protected).

##### Enforcement

Look for `struct`s with all data private and `class`es with public members.

### <a name="Rc-interface"></a>C.3: Represent the distinction between an interface and an implementation using a class

##### Reason

An explicit distinction between interface and implementation improves readability and simplifies maintenance.

##### Example

    class Date {
        // ... some representation ...
    public:
        Date();
        // validate that {yy, mm, dd} is a valid date and initialize
        Date(int yy, Month mm, char dd);

        int day() const;
        Month month() const;
        // ...
    };

For example, we can now change the representation of a `Date` without affecting its users (recompilation is likely, though).

##### Note

Using a class in this way to represent the distinction between interface and implementation is of course not the only way.
For example, we can use a set of declarations of freestanding functions in a namespace, an abstract base class, or a template function with concepts to represent an interface.
The most important issue is to explicitly distinguish between an interface and its implementation "details."
Ideally, and typically, an interface is far more stable than its implementation(s).

##### Enforcement

???

### <a name="Rc-member"></a>C.4: Make a function a member only if it needs direct access to the representation of a class

##### Reason

Less coupling than with member functions, fewer functions that can cause trouble by modifying object state, reduces the number of functions that needs to be modified after a change in representation.

##### Example

    class Date {
        // ... relatively small interface ...
    };

    // helper functions:
    Date next_weekday(Date);
    bool operator==(Date, Date);

The "helper functions" have no need for direct access to the representation of a `Date`.

##### Note

This rule becomes even better if C++ gets ["uniform function call"](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0251r0.pdf).

##### Enforcement

Look for member function that do not touch data members directly.
The snag is that many member functions that do not need to touch data members directly do.

### <a name="Rc-helper"></a>C.5: Place helper functions in the same namespace as the class they support

##### Reason

A helper function is a function (usually supplied by the writer of a class) that does not need direct access to the representation of the class, yet is seen as part of the useful interface to the class.
Placing them in the same namespace as the class makes their relationship to the class obvious and allows them to be found by argument dependent lookup.

##### Example

    namespace Chrono { // here we keep time-related services

        class Time { /* ... */ };
        class Date { /* ... */ };

        // helper functions:
        bool operator==(Date, Date);
        Date next_weekday(Date);
        // ...
    }

##### Note

This is especially important for [overloaded operators](#Ro-namespace).

##### Enforcement

* Flag global functions taking argument types from a single namespace.

### <a name="Rc-standalone"></a>C.7: Don't define a class or enum and declare a variable of its type in the same statement

##### Reason

Mixing a type definition and the definition of another entity in the same declaration is confusing and unnecessary.

##### Example; bad

    struct Data { /*...*/ } data{ /*...*/ };

##### Example; good

    struct Data { /*...*/ };
    Data data{ /*...*/ };

##### Enforcement

* Flag if the `}` of a class or enumeration definition is not followed by a `;`. The `;` is missing.

### <a name="Rc-class"></a>C.8: use `class` rather that `struct` if any member is non-public

##### Reason

Readability.
To make it clear that something is being hidden/abstracted.
This is a useful convention.

##### Example, bad

    struct Date {
        int d, m;

        Date(int i, Month m);
        // ... lots of functions ...
    private:
        int y;  // year
    };

There is nothing wrong with this code as far as the C++ language rules are concerned,
but nearly everything is wrong from a design perspective.
The private data is hidden far from the public data.
The data is split in different parts of the class declaration.
Different parts of the data has difference access.
All of this decreases readability and complicates maintenance.

##### Note

Prefer to place the interface first in a class [see](#Rl-order).

##### Enforcement

Flag classes declared with `struct` if there is a `private` or `public` member.

### <a name="Rc-private"></a>C.9: minimize exposure of members

##### Reason

Encapsulation.
Information hiding.
Minimize the chance of untended access.
This simplifies maintenance.

##### Example

    ???

##### Note

Prefer the order `public` members before `protected` members before `private` members [see](#Rl-order).

##### Enforcement

Flag protected data.

## <a name="SS-concrete"></a>C.concrete: Concrete types

One ideal for a class is to be a regular type.
That means roughly "behaves like an `int`." A concrete type is the simplest kind of class.
A value of regular type can be copied and the result of a copy is an independent object with the same value as the original.
If a concrete type has both `=` and `==`, `a = b` should result in `a == b` being `true`.
Concrete classes without assignment and equality can be defined, but they are (and should be) rare.
The C++ built-in types are regular, and so are standard-library classes, such as `string`, `vector`, and `map`.
Concrete types are also often referred to as value types to distinguish them from types uses as part of a hierarchy.

Concrete type rule summary:

* [C.10: Prefer a concrete type over more complicated classes](#Rc-concrete)
* [C.11: Make concrete types regular](#Rc-regular)

### <a name="Rc-concrete"></a>C.10 Prefer a concrete type over more complicated classes

##### Reason

A concrete type is fundamentally simpler than a hierarchy:
easier to design, easier to implement, easier to use, easier to reason about, smaller, and faster.
You need a reason (use cases) for using a hierarchy.

##### Example

    class Point1 {
        int x, y;
        // ... operations ...
        // ... no virtual functions ...
    };

    class Point2 {
        int x, y;
        // ... operations, some virtual ...
        virtual ~Point2();
    };

    void use()
    {
        Point1 p11 {1, 2};   // make an object on the stack
        Point1 p12 {p11};    // a copy

        auto p21 = make_unique<Point2>(1, 2);   // make an object on the free store
        auto p22 = p21.clone();                 // make a copy
        // ...
    }

If a class can be part of a hierarchy, we (in real code if not necessarily in small examples) must manipulate its objects through pointers or references.
That implies more memory overhead, more allocations and deallocations, and more run-time overhead to perform the resulting indirections.

##### Note

Concrete types can be stack allocated and be members of other classes.

##### Note

The use of indirection is fundamental for run-time polymorphic interfaces.
The allocation/deallocation overhead is not (that's just the most common case).
We can use a base class as the interface of a scoped object of a derived class.
This is done where dynamic allocation is prohibited (e.g. hard real-time) and to provide a stable interface to some kinds of plug-ins.

##### Enforcement

???

### <a name="Rc-regular"></a>C.11: Make concrete types regular

##### Reason

Regular types are easier to understand and reason about than types that are not regular (irregularities requires extra effort to understand and use).

##### Example

    struct Bundle {
        string name;
        vector<Record> vr;
    };

    bool operator==(const Bundle& a, const Bundle& b)
    {
        return a.name == b.name && a.vr == b.vr;
    }

    Bundle b1 { "my bundle", {r1, r2, r3}};
    Bundle b2 = b1;
    if (!(b1 == b2)) error("impossible!");
    b2.name = "the other bundle";
    if (b1 == b2) error("No!");

In particular, if a concrete type has an assignment also give it an equals operator so that `a = b` implies `a == b`.

##### Enforcement

???

## <a name="S-ctor"></a>C.ctor: Constructors, assignments, and destructors

These functions control the lifecycle of objects: creation, copy, move, and destruction.
Define constructors to guarantee and simplify initialization of classes.

These are *default operations*:

* a default constructor: `X()`
* a copy constructor: `X(const X&)`
* a copy assignment: `operator=(const X&)`
* a move constructor: `X(X&&)`
* a move assignment: `operator=(X&&)`
* a destructor: `~X()`

By default, the compiler defines each of these operations if it is used, but the default can be suppressed.

The default operations are a set of related operations that together implement the lifecycle semantics of an object.
By default, C++ treats classes as value-like types, but not all types are value-like.

Set of default operations rules:

* [C.20: If you can avoid defining any default operations, do](#Rc-zero)
* [C.21: If you define or `=delete` any default operation, define or `=delete` them all](#Rc-five)
* [C.22: Make default operations consistent](#Rc-matched)

Destructor rules:

* [C.30: Define a destructor if a class needs an explicit action at object destruction](#Rc-dtor)
* [C.31: All resources acquired by a class must be released by the class's destructor](#Rc-dtor-release)
* [C.32: If a class has a raw pointer (`T*`) or reference (`T&`), consider whether it might be owning](#Rc-dtor-ptr)
* [C.33: If a class has an owning pointer member, define or `=delete` a destructor](#Rc-dtor-ptr2)
* [C.34: If a class has an owning reference member, define or `=delete` a destructor](#Rc-dtor-ref)
* [C.35: A base class with a virtual function needs a virtual destructor](#Rc-dtor-virtual)
* [C.36: A destructor may not fail](#Rc-dtor-fail)
* [C.37: Make destructors `noexcept`](#Rc-dtor-noexcept)

Constructor rules:

* [C.40: Define a constructor if a class has an invariant](#Rc-ctor)
* [C.41: A constructor should create a fully initialized object](#Rc-complete)
* [C.42: If a constructor cannot construct a valid object, throw an exception](#Rc-throw)
* [C.43: Ensure that a class has a default constructor](#Rc-default0)
* [C.44: Prefer default constructors to be simple and non-throwing](#Rc-default00)
* [C.45: Don't define a default constructor that only initializes data members; use member initializers instead](#Rc-default)
* [C.46: By default, declare single-argument constructors `explicit`](#Rc-explicit)
* [C.47: Define and initialize member variables in the order of member declaration](#Rc-order)
* [C.48: Prefer in-class initializers to member initializers in constructors for constant initializers](#Rc-in-class-initializer)
* [C.49: Prefer initialization to assignment in constructors](#Rc-initialize)
* [C.50: Use a factory function if you need "virtual behavior" during initialization](#Rc-factory)
* [C.51: Use delegating constructors to represent common actions for all constructors of a class](#Rc-delegating)
* [C.52: Use inheriting constructors to import constructors into a derived class that does not need further explicit initialization](#Rc-inheriting)

Copy and move rules:

* [C.60: Make copy assignment non-`virtual`, take the parameter by `const&`, and return by non-`const&`](#Rc-copy-assignment)
* [C.61: A copy operation should copy](#Rc-copy-semantic)
* [C.62: Make copy assignment safe for self-assignment](#Rc-move-self)
* [C.63: Make move assignment non-`virtual`, take the parameter by `&&`, and return by non-`const&`](#Rc-move-assignment)
* [C.64: A move operation should move and leave its source in a valid state](#Rc-move-semantic)
* [C.65: Make move assignment safe for self-assignment](#Rc-copy-self)
* [C.66: Make move operations `noexcept`](#Rc-move-noexcept)
* [C.67: A base class should suppress copying, and provide a virtual `clone` instead if "copying" is desired](#Rc-copy-virtual)

Other default operations rules:

* [C.80: Use `=default` if you have to be explicit about using the default semantics](#Rc-eqdefault)
* [C.81: Use `=delete` when you want to disable default behavior (without wanting an alternative)](#Rc-delete)
* [C.82: Don't call virtual functions in constructors and destructors](#Rc-ctor-virtual)
* [C.83: For value-like types, consider providing a `noexcept` swap function](#Rc-swap)
* [C.84: A `swap` may not fail](#Rc-swap-fail)
* [C.85: Make `swap` `noexcept`](#Rc-swap-noexcept)
* [C.86: Make `==` symmetric with respect of operand types and `noexcept`](#Rc-eq)
* [C.87: Beware of `==` on base classes](#Rc-eq-base)
* [C.89: Make a `hash` `noexcept`](#Rc-hash)

## <a name="SS-defop"></a>C.defop: Default Operations

By default, the language supply the default operations with their default semantics.
However, a programmer can disable or replace these defaults.

### <a name="Rc-zero"></a>C.20: If you can avoid defining default operations, do

##### Reason

It's the simplest and gives the cleanest semantics.

##### Example

    struct Named_map {
    public:
        // ... no default operations declared ...
    private:
        string name;
        map<int, int> rep;
    };

    Named_map nm;        // default construct
    Named_map nm2 {nm};  // copy construct

Since `std::map` and `string` have all the special functions, no further work is needed.

##### Note

This is known as "the rule of zero".

##### Enforcement

(Not enforceable) While not enforceable, a good static analyzer can detect patterns that indicate a possible improvement to meet this rule.
For example, a class with a (pointer, size) pair of member and a destructor that `delete`s the pointer could probably be converted to a `vector`.

### <a name="Rc-five"></a>C.21: If you define or `=delete` any default operation, define or `=delete` them all

##### Reason

The semantics of the special functions are closely related, so if one needs to be non-default, the odds are that others need modification too.

##### Example, bad

    struct M2 {   // bad: incomplete set of default operations
    public:
        // ...
        // ... no copy or move operations ...
        ~M2() { delete[] rep; }
    private:
        pair<int, int>* rep;  // zero-terminated set of pairs
    };

    void use()
    {
        M2 x;
        M2 y;
        // ...
        x = y;   // the default assignment
        // ...
    }

Given that "special attention" was needed for the destructor (here, to deallocate), the likelihood that copy and move assignment (both will implicitly destroy an object) are correct is low (here, we would get double deletion).

##### Note

This is known as "the rule of five" or "the rule of six", depending on whether you count the default constructor.

##### Note

If you want a default implementation of a default operation (while defining another), write `=default` to show you're doing so intentionally for that function.
If you don't want a default operation, suppress it with `=delete`.

##### Note

Compilers enforce much of this rule and ideally warn about any violation.

##### Note

Relying on an implicitly generated copy operation in a class with a destructor is deprecated.

##### Enforcement

(Simple) A class should have a declaration (even a `=delete` one) for either all or none of the special functions.

### <a name="Rc-matched"></a>C.22: Make default operations consistent

##### Reason

The default operations are conceptually a matched set. Their semantics are interrelated.
Users will be surprised if copy/move construction and copy/move assignment do logically different things. Users will be surprised if constructors and destructors do not provide a consistent view of resource management. Users will be surprised if copy and move don't reflect the way constructors and destructors work.

##### Example, bad

    class Silly {   // BAD: Inconsistent copy operations
        class Impl {
            // ...
        };
        shared_ptr<Impl> p;
    public:
        Silly(const Silly& a) : p{a.p} { *p = *a.p; }   // deep copy
        Silly& operator=(const Silly& a) { p = a.p; }   // shallow copy
        // ...
    };

These operations disagree about copy semantics. This will lead to confusion and bugs.

##### Enforcement

* (Complex) A copy/move constructor and the corresponding copy/move assignment operator should write to the same member variables at the same level of dereference.
* (Complex) Any member variables written in a copy/move constructor should also be initialized by all other constructors.
* (Complex) If a copy/move constructor performs a deep copy of a member variable, then the destructor should modify the member variable.
* (Complex) If a destructor is modifying a member variable, that member variable should be written in any copy/move constructors or assignment operators.

## <a name="SS-dtor"></a>C.dtor: Destructors

"Does this class need a destructor?" is a surprisingly powerful design question.
For most classes the answer is "no" either because the class holds no resources or because destruction is handled by [the rule of zero](#Rc-zero);
that is, its members can take care of themselves as concerns destruction.
If the answer is "yes", much of the design of the class follows (see [the rule of five](#Rc-five)).

### <a name="Rc-dtor"></a>C.30: Define a destructor if a class needs an explicit action at object destruction

##### Reason

A destructor is implicitly invoked at the end of an object's lifetime.
If the default destructor is sufficient, use it.
Only define a non-default destructor if a class needs to execute code that is not already part of its members' destructors.

##### Example

    template<typename A>
    struct final_action {   // slightly simplified
        A act;
        final_action(A a) :act{a} {}
        ~final_action() { act(); }
    };

    template<typename A>
    final_action<A> finally(A act)   // deduce action type
    {
        return final_action<A>{act};
    }

    void test()
    {
        auto act = finally([]{ cout << "Exit test\n"; });  // establish exit action
        // ...
        if (something) return;   // act done here
        // ...
    } // act done here

The whole purpose of `final_action` is to get a piece of code (usually a lambda) executed upon destruction.

##### Note

There are two general categories of classes that need a user-defined destructor:

* A class with a resource that is not already represented as a class with a destructor, e.g., a `vector` or a transaction class.
* A class that exists primarily to execute an action upon destruction, such as a tracer or `final_action`.

##### Example, bad

    class Foo {   // bad; use the default destructor
    public:
        // ...
        ~Foo() { s = ""; i = 0; vi.clear(); }  // clean up
    private:
        string s;
        int i;
        vector<int> vi;
    };

The default destructor does it better, more efficiently, and can't get it wrong.

##### Note

If the default destructor is needed, but its generation has been suppressed (e.g., by defining a move constructor), use `=default`.

##### Enforcement

Look for likely "implicit resources", such as pointers and references. Look for classes with destructors even though all their data members have destructors.

### <a name="Rc-dtor-release"></a>C.31: All resources acquired by a class must be released by the class's destructor

##### Reason

Prevention of resource leaks, especially in error cases.

##### Note

For resources represented as classes with a complete set of default operations, this happens automatically.

##### Example

    class X {
        ifstream f;   // may own a file
        // ... no default operations defined or =deleted ...
    };

`X`'s `ifstream` implicitly closes any file it may have open upon destruction of its `X`.

##### Example, bad

    class X2 {     // bad
        FILE* f;   // may own a file
        // ... no default operations defined or =deleted ...
    };

`X2` may leak a file handle.

##### Note

What about a sockets that won't close? A destructor, close, or cleanup operation [should never fail](#Rc-dtor-fail).
If it does nevertheless, we have a problem that has no really good solution.
For starters, the writer of a destructor does not know why the destructor is called and cannot "refuse to act" by throwing an exception.
See [discussion](#Sd-never-fail).
To make the problem worse, many "close/release" operations are not retryable.
Many have tried to solve this problem, but no general solution is known.
If at all possible, consider failure to close/cleanup a fundamental design error and terminate.

##### Note

A class can hold pointers and references to objects that it does not own.
Obviously, such objects should not be `delete`d by the class's destructor.
For example:

    Preprocessor pp { /* ... */ };
    Parser p { pp, /* ... */ };
    Type_checker tc { p, /* ... */ };

Here `p` refers to `pp` but does not own it.

##### Enforcement

* (Simple) If a class has pointer or reference member variables that are owners
  (e.g., deemed owners by using `gsl::owner`), then they should be referenced in its destructor.
* (Hard) Determine if pointer or reference member variables are owners when there is no explicit statement of ownership
  (e.g., look into the constructors).

### <a name="Rc-dtor-ptr"></a>C.32: If a class has a raw pointer (`T*`) or reference (`T&`), consider whether it might be owning

##### Reason

There is a lot of code that is non-specific about ownership.

##### Example

    ???

##### Note

If the `T*` or `T&` is owning, mark it `owning`. If the `T*` is not owning, consider marking it `ptr`.
This will aid documentation and analysis.

##### Enforcement

Look at the initialization of raw member pointers and member references and see if an allocation is used.

### <a name="Rc-dtor-ptr2"></a>C.33: If a class has an owning pointer member, define a destructor

##### Reason

An owned object must be `deleted` upon destruction of the object that owns it.

##### Example

A pointer member may represent a resource.
[A `T*` should not do so](#Rr-ptr), but in older code, that's common.
Consider a `T*` a possible owner and therefore suspect.

    template<typename T>
    class Smart_ptr {
        T* p;   // BAD: vague about ownership of *p
        // ...
    public:
        // ... no user-defined default operations ...
    };

    void use(Smart_ptr<int> p1)
    {
        // error: p2.p leaked (if not nullptr and not owned by some other code)
        auto p2 = p1;
    }

Note that if you define a destructor, you must define or delete [all default operations](#Rc-five):

    template<typename T>
    class Smart_ptr2 {
        T* p;   // BAD: vague about ownership of *p
        // ...
    public:
        // ... no user-defined copy operations ...
        ~Smart_ptr2() { delete p; }  // p is an owner!
    };

    void use(Smart_ptr2<int> p1)
    {
        auto p2 = p1;   // error: double deletion
    }

The default copy operation will just copy the `p1.p` into `p2.p` leading to a double destruction of `p1.p`. Be explicit about ownership:

    template<typename T>
    class Smart_ptr3 {
        owner<T*> p;   // OK: explicit about ownership of *p
        // ...
    public:
        // ...
        // ... copy and move operations ...
        ~Smart_ptr3() { delete p; }
    };

    void use(Smart_ptr3<int> p1)
    {
        auto p2 = p1;   // error: double deletion
    }

##### Note

Often the simplest way to get a destructor is to replace the pointer with a smart pointer (e.g., `std::unique_ptr`) and let the compiler arrange for proper destruction to be done implicitly.

##### Note

Why not just require all owning pointers to be "smart pointers"?
That would sometimes require non-trivial code changes and may affect ABIs.

##### Enforcement

* A class with a pointer data member is suspect.
* A class with an `owner<T>` should define its default operations.

### <a name="Rc-dtor-ref"></a>C.34: If a class has an owning reference member, define a destructor

##### Reason

A reference member may represent a resource.
It should not do so, but in older code, that's common.
See [pointer members and destructors](#Rc-dtor-ptr).
Also, copying may lead to slicing.

##### Example, bad

    class Handle {  // Very suspect
        Shape& s;   // use reference rather than pointer to prevent rebinding
                    // BAD: vague about ownership of *p
        // ...
    public:
        Handle(Shape& ss) : s{ss} { /* ... */ }
        // ...
    };

The problem of whether `Handle` is responsible for the destruction of its `Shape` is the same as for [the pointer case](#Rc-dtor-ptr):
If the `Handle` owns the object referred to by `s` it must have a destructor.

##### Example

    class Handle {        // OK
        owner<Shape&> s;  // use reference rather than pointer to prevent rebinding
        // ...
    public:
        Handle(Shape& ss) : s{ss} { /* ... */ }
        ~Handle() { delete &s; }
        // ...
    };

Independently of whether `Handle` owns its `Shape`, we must consider the default copy operations suspect:

    // the Handle had better own the Circle or we have a leak
    Handle x {*new Circle{p1, 17}};

    Handle y {*new Triangle{p1, p2, p3}};
    x = y;     // the default assignment will try *x.s = *y.s

That `x = y` is highly suspect.
Assigning a `Triangle` to a `Circle`?
Unless `Shape` has its [copy assignment `=deleted`](#Rc-copy-virtual), only the `Shape` part of `Triangle` is copied into the `Circle`.

##### Note

Why not just require all owning references to be replaced by "smart pointers"?
Changing from references to smart pointers implies code changes.
We don't (yet) have smart references.
Also, that may affect ABIs.

##### Enforcement

* A class with a reference data member is suspect.
* A class with an `owner<T>` reference should define its default operations.

### <a name="Rc-dtor-virtual"></a>C.35: A base class destructor should be either public and virtual, or protected and nonvirtual

##### Reason

To prevent undefined behavior.
If the destructor is public, then calling code can attempt to destroy a derived class object through a base class pointer, and the result is undefined if the base class's destructor is non-virtual.
If the destructor is protected, then calling code cannot destroy through a base class pointer and the destructor does not need to be virtual; it does need to be protected, not private, so that derived destructors can invoke it.
In general, the writer of a base class does not know the appropriate action to be done upon destruction.

##### Discussion

See [this in the Discussion section](#Sd-dtor).

##### Example, bad

    struct Base {  // BAD: no virtual destructor
        virtual void f();
    };

    struct D : Base {
        string s {"a resource needing cleanup"};
        ~D() { /* ... do some cleanup ... */ }
        // ...
    };

    void use()
    {
        unique_ptr<Base> p = make_unique<D>();
        // ...
    } // p's destruction calls ~Base(), not ~D(), which leaks D::s and possibly more

##### Note

A virtual function defines an interface to derived classes that can be used without looking at the derived classes.
If the interface allows destroying, it should be safe to do so.

##### Note

A destructor must be nonprivate or it will prevent using the type :

    class X {
        ~X();   // private destructor
        // ...
    };

    void use()
    {
        X a;                        // error: cannot destroy
        auto p = make_unique<X>();  // error: cannot destroy
    }

##### Exception

We can imagine one case where you could want a protected virtual destructor: When an object of a derived type (and only of such a type) should be allowed to destroy *another* object (not itself) through a pointer to base. We haven't seen such a case in practice, though.

##### Enforcement

* A class with any virtual functions should have a destructor that is either public and virtual or else protected and nonvirtual.

### <a name="Rc-dtor-fail"></a>C.36: A destructor may not fail

##### Reason

In general we do not know how to write error-free code if a destructor should fail.
The standard library requires that all classes it deals with have destructors that do not exit by throwing.

##### Example

    class X {
    public:
        ~X() noexcept;
        // ...
    };

    X::~X() noexcept
    {
        // ...
        if (cannot_release_a_resource) terminate();
        // ...
    }

##### Note

Many have tried to devise a fool-proof scheme for dealing with failure in destructors.
None have succeeded to come up with a general scheme.
This can be a real practical problem: For example, what about a socket that won't close?
The writer of a destructor does not know why the destructor is called and cannot "refuse to act" by throwing an exception.
See [discussion](#Sd-dtor).
To make the problem worse, many "close/release" operations are not retryable.
If at all possible, consider failure to close/cleanup a fundamental design error and terminate.

##### Note

Declare a destructor `noexcept`. That will ensure that it either completes normally or terminate the program.

##### Note

If a resource cannot be released and the program may not fail, try to signal the failure to the rest of the system somehow
(maybe even by modifying some global state and hope something will notice and be able to take care of the problem).
Be fully aware that this technique is special-purpose and error-prone.
Consider the "my connection will not close" example.
Probably there is a problem at the other end of the connection and only a piece of code responsible for both ends of the connection can properly handle the problem.
The destructor could send a message (somehow) to the responsible part of the system, consider that to have closed the connection, and return normally.

##### Note

If a destructor uses operations that may fail, it can catch exceptions and in some cases still complete successfully
(e.g., by using a different clean-up mechanism from the one that threw an exception).

##### Enforcement

(Simple) A destructor should be declared `noexcept`.

### <a name="Rc-dtor-noexcept"></a>C.37: Make destructors `noexcept`

##### Reason

 [A destructor may not fail](#Rc-dtor-fail). If a destructor tries to exit with an exception, it's a bad design error and the program had better terminate.

##### Note

A destructor (either user-defined or compiler-generated) is implicitly declared `noexcept` (independently of what code is in its body) if all of the members of its class have `noexcept` destructors.

##### Enforcement

(Simple) A destructor should be declared `noexcept`.

## <a name="SS-ctor"></a>C.ctor: Constructors

A constructor defines how an object is initialized (constructed).

### <a name="Rc-ctor"></a>C.40: Define a constructor if a class has an invariant

##### Reason

That's what constructors are for.

##### Example

    class Date {  // a Date represents a valid date
                  // in the January 1, 1900 to December 31, 2100 range
        Date(int dd, int mm, int yy)
            :d{dd}, m{mm}, y{yy}
        {
            if (!is_valid(d, m, y)) throw Bad_date{};  // enforce invariant
        }
        // ...
    private:
        int d, m, y;
    };

It is often a good idea to express the invariant as an `Ensures` on the constructor.

##### Note

A constructor can be used for convenience even if a class does not have an invariant. For example:

    struct Rec {
        string s;
        int i {0};
        Rec(const string& ss) : s{ss} {}
        Rec(int ii) :i{ii} {}
    };

    Rec r1 {7};
    Rec r2 {"Foo bar"};

##### Note

The C++11 initializer list rule eliminates the need for many constructors. For example:

    struct Rec2{
        string s;
        int i;
        Rec2(const string& ss, int ii = 0) :s{ss}, i{ii} {}   // redundant
    };

    Rec2 r1 {"Foo", 7};
    Rec2 r2 {"Bar"};

The `Rec2` constructor is redundant.
Also, the default for `int` would be better done as a [member initializer](#Rc-in-class-initializer).

**See also**: [construct valid object](#Rc-complete) and [constructor throws](#Rc-throw).

##### Enforcement

* Flag classes with user-defined copy operations but no constructor (a user-defined copy is a good indicator that the class has an invariant)

### <a name="Rc-complete"></a>C.41: A constructor should create a fully initialized object

##### Reason

A constructor establishes the invariant for a class. A user of a class should be able to assume that a constructed object is usable.

##### Example, bad

    class X1 {
        FILE* f;   // call init() before any other function
        // ...
    public:
        X1() {}
        void init();   // initialize f
        void read();   // read from f
        // ...
    };

    void f()
    {
        X1 file;
        file.read();   // crash or bad read!
        // ...
        file.init();   // too late
        // ...
    }

Compilers do not read comments.

##### Exception

If a valid object cannot conveniently be constructed by a constructor [use a factory function](#Rc-factory).

##### Note

If a constructor acquires a resource (to create a valid object), that resource should be [released by the destructor](#Rc-dtor-release).
The idiom of having constructors acquire resources and destructors release them is called [RAII](#Rr-raii) ("Resource Acquisition Is Initialization").

### <a name="Rc-throw"></a>C.42: If a constructor cannot construct a valid object, throw an exception

##### Reason

Leaving behind an invalid object is asking for trouble.

##### Example

    class X2 {
        FILE* f;   // call init() before any other function
        // ...
    public:
        X2(const string& name)
            :f{fopen(name.c_str(), "r")}
        {
            if (f == nullptr) throw runtime_error{"could not open" + name};
            // ...
        }

        void read();      // read from f
        // ...
    };

    void f()
    {
        X2 file {"Zeno"}; // throws if file isn't open
        file.read();      // fine
        // ...
    }

##### Example, bad

    class X3 {     // bad: the constructor leaves a non-valid object behind
        FILE* f;   // call init() before any other function
        bool valid;
        // ...
    public:
        X3(const string& name)
            :f{fopen(name.c_str(), "r")}, valid{false}
        {
            if (f) valid = true;
            // ...
        }

        bool is_valid() { return valid; }
        void read();   // read from f
        // ...
    };

    void f()
    {
        X3 file {"Heraclides"};
        file.read();   // crash or bad read!
        // ...
        if (file.is_valid()) {
            file.read();
            // ...
        }
        else {
            // ... handle error ...
        }
        // ...
    }

##### Note

For a variable definition (e.g., on the stack or as a member of another object) there is no explicit function call from which an error code could be returned.
Leaving behind an invalid object and relying on users to consistently check an `is_valid()` function before use is tedious, error-prone, and inefficient.

##### Exception

There are domains, such as some hard-real-time systems (think airplane controls) where (without additional tool support) exception handling is not sufficiently predictable from a timing perspective.
There the `is_valid()` technique must be used. In such cases, check `is_valid()` consistently and immediately to simulate [RAII](#Rr-raii).

**Alternative**: If you feel tempted to use some "post-constructor initialization" or "two-stage initialization" idiom, try not to do that.
If you really have to, look at [factory functions](#Rc-factory).

##### Note

One reason people have used `init()` functions rather than doing the initialization work in a constructor has been to avoid code replication.
[Delegating constructors](#Rc-delegating) and [default member initialization](#Rc-in-class-initializer) do that better.
Another reason is been to delay initialization until an object is needed; the solution to that is often [not to declare a variable until it can be properly initialized](#Res-init)

##### Enforcement

* (Simple) Every constructor should initialize every member variable (either explicitly, via a delegating ctor call or via default construction).
* (Unknown) If a constructor has an `Ensures` contract, try to see if it holds as a postcondition.

### <a name="Rc-default0"></a>C.43: Ensure that a class has a default constructor

##### Reason

Many language and library facilities rely on default constructors to initialize their elements, e.g. `T a[10]` and `std::vector<T> v(10)`.

##### Example , bad

    class Date { // BAD: no default constructor
    public:
        Date(int dd, int mm, int yyyy);
        // ...
    };

    vector<Date> vd1(1000);   // default Date needed here
    vector<Date> vd2(1000, Date{Month::october, 7, 1885});   // alternative

The default constructor is only auto-generated if there is no user-declared constructor, hence it's impossible to initialize the vector `vd1` in the example above.

There is no "natural" default date (the big bang is too far back in time to be useful for most people), so this example is non-trivial.
`{0, 0, 0}` is not a valid date in most calendar systems, so choosing that would be introducing something like floating-point's `NaN`.
However, most realistic `Date` classes have a "first date" (e.g. January 1, 1970 is popular), so making that the default is usually trivial.

##### Example

    class Date {
    public:
        Date(int dd, int mm, int yyyy);
        Date() = default; // See also C.45
        // ...
    private:
        int dd = 1;
        int mm = 1;
        int yyyy = 1970;
        // ...
    };

    vector<Date> vd1(1000);

##### Note

A class with members that all have default constructors implicitly gets a default constructor:

    struct X {
        string s;
        vector<int> v;
    };

    X x; // means X{{ "{{" }}}, {}}; that is the empty string and the empty vector

Beware that built-in types are not properly default constructed:

    struct X {
        string s;
        int i;
    };

    void f()
    {
        X x;    // x.s is initialized to the empty string; x.i is uninitialized

        cout << x.s << ' ' << x.i << '\n';
        ++x.i;
    }

Statically allocated objects of built-in types are by default initialized to `0`, but local built-in variables are not.
Beware that your compiler may default initialize local built-in variables, whereas an optimized build will not.
Thus, code like the example above may appear to work, but it relies on undefined behavior.
Assuming that you want initialization, an explicit default initialization can help:

    struct X {
        string s;
        int i {};   // default initialize (to 0)
    };

##### Enforcement

* Flag classes without a default constructor

### <a name="Rc-default00"></a>C.44: Prefer default constructors to be simple and non-throwing

##### Reason

Being able to set a value to "the default" without operations that might fail simplifies error handling and reasoning about move operations.

##### Example, problematic

    template<typename T>
    // elem points to space-elem element allocated using new
    class Vector0 {
    public:
        Vector0() :Vector0{0} {}
        Vector0(int n) :elem{new T[n]}, space{elem + n}, last{elem} {}
        // ...
    private:
        own<T*> elem;
        T* space;
        T* last;
    };

This is nice and general, but setting a `Vector0` to empty after an error involves an allocation, which may fail.
Also, having a default `Vector` represented as `{new T[0], 0, 0}` seems wasteful.
For example, `Vector0 v(100)` costs 100 allocations.

##### Example

    template<typename T>
    // elem is nullptr or elem points to space-elem element allocated using new
    class Vector1 {
    public:
        // sets the representation to {nullptr, nullptr, nullptr}; doesn't throw
        Vector1() noexcept {}
        Vector1(int n) :elem{new T[n]}, space{elem + n}, last{elem} {}
        // ...
    private:
        own<T*> elem = nullptr;
        T* space = nullptr;
        T* last = nullptr;
    };

Using `{nullptr, nullptr, nullptr}` makes `Vector1{}` cheap, but a special case and implies run-time checks.
Setting a `Vector1` to empty after detecting an error is trivial.

##### Enforcement

* Flag throwing default constructors

### <a name="Rc-default"></a>C.45: Don't define a default constructor that only initializes data members; use in-class member initializers instead

##### Reason

Using in-class member initializers lets the compiler generate the function for you. The compiler-generated function can be more efficient.

##### Example, bad

    class X1 { // BAD: doesn't use member initializers
        string s;
        int i;
    public:
        X1() :s{"default"}, i{1} { }
        // ...
    };

##### Example

    class X2 {
        string s = "default";
        int i = 1;
    public:
        // use compiler-generated default constructor
        // ...
    };

##### Enforcement

(Simple) A default constructor should do more than just initialize member variables with constants.

### <a name="Rc-explicit"></a>C.46: By default, declare single-argument constructors explicit

##### Reason

To avoid unintended conversions.

##### Example, bad

    class String {
        // ...
    public:
        String(int);   // BAD
        // ...
    };

    String s = 10;   // surprise: string of size 10

##### Exception

If you really want an implicit conversion from the constructor argument type to the class type, don't use `explicit`:

    class Complex {
        // ...
    public:
        Complex(double d);   // OK: we want a conversion from d to {d, 0}
        // ...
    };

    Complex z = 10.7;   // unsurprising conversion

**See also**: [Discussion of implicit conversions](#Ro-conversion).

##### Enforcement

(Simple) Single-argument constructors should be declared `explicit`. Good single argument non-`explicit` constructors are rare in most code based. Warn for all that are not on a "positive list".

### <a name="Rc-order"></a>C.47: Define and initialize member variables in the order of member declaration

##### Reason

To minimize confusion and errors. That is the order in which the initialization happens (independent of the order of member initializers).

##### Example, bad

    class Foo {
        int m1;
        int m2;
    public:
        Foo(int x) :m2{x}, m1{++x} { }   // BAD: misleading initializer order
        // ...
    };

    Foo x(1); // surprise: x.m1 == x.m2 == 2

##### Enforcement

(Simple) A member initializer list should mention the members in the same order they are declared.

**See also**: [Discussion](#Sd-order)

### <a name="Rc-in-class-initializer"></a>C.48: Prefer in-class initializers to member initializers in constructors for constant initializers

##### Reason

Makes it explicit that the same value is expected to be used in all constructors. Avoids repetition. Avoids maintenance problems. It leads to the shortest and most efficient code.

##### Example, bad

    class X {   // BAD
        int i;
        string s;
        int j;
    public:
        X() :i{666}, s{"qqq"} { }   // j is uninitialized
        X(int ii) :i{ii} {}         // s is "" and j is uninitialized
        // ...
    };

How would a maintainer know whether `j` was deliberately uninitialized (probably a poor idea anyway) and whether it was intentional to give `s` the default value `""` in one case and `qqq` in another (almost certainly a bug)? The problem with `j` (forgetting to initialize a member) often happens when a new member is added to an existing class.

##### Example

    class X2 {
        int i {666};
        string s {"qqq"};
        int j {0};
    public:
        X2() = default;        // all members are initialized to their defaults
        X2(int ii) :i{ii} {}   // s and j initialized to their defaults
        // ...
    };

**Alternative**: We can get part of the benefits from default arguments to constructors, and that is not uncommon in older code. However, that is less explicit, causes more arguments to be passed, and is repetitive when there is more than one constructor:

    class X3 {   // BAD: inexplicit, argument passing overhead
        int i;
        string s;
        int j;
    public:
        X3(int ii = 666, const string& ss = "qqq", int jj = 0)
            :i{ii}, s{ss}, j{jj} { }   // all members are initialized to their defaults
        // ...
    };

##### Enforcement

* (Simple) Every constructor should initialize every member variable (either explicitly, via a delegating ctor call or via default construction).
* (Simple) Default arguments to constructors suggest an in-class initializer may be more appropriate.

### <a name="Rc-initialize"></a>C.49: Prefer initialization to assignment in constructors

##### Reason

An initialization explicitly states that initialization, rather than assignment, is done and can be more elegant and efficient. Prevents "use before set" errors.

##### Example, good

    class A {   // Good
        string s1;
    public:
        A() : s1{"Hello, "} { }    // GOOD: directly construct
        // ...
    };

##### Example, bad

    class B {   // BAD
        string s1;
    public:
        B() { s1 = "Hello, "; }   // BAD: default constructor followed by assignment
        // ...
    };

    class C {   // UGLY, aka very bad
        int* p;
    public:
        C() { cout << *p; p = new int{10}; }   // accidental use before initialized
        // ...
    };

### <a name="Rc-factory"></a>C.50: Use a factory function if you need "virtual behavior" during initialization

##### Reason

If the state of a base class object must depend on the state of a derived part of the object, we need to use a virtual function (or equivalent) while minimizing the window of opportunity to misuse an imperfectly constructed object.

##### Example, bad

    class B {
    public:
        B()
        {
            // ...
            f();   // BAD: virtual call in constructor
            // ...
        }

        virtual void f() = 0;

        // ...
    };

##### Example

    class B {
    protected:
        B() { /* ... */ }              // create an imperfectly initialized object

        virtual void PostInitialize()  // to be called right after construction
        {
            // ...
            f();    // GOOD: virtual dispatch is safe
            // ...
        }

    public:
        virtual void f() = 0;

        template<class T>
        static shared_ptr<T> Create()  // interface for creating objects
        {
            auto p = make_shared<T>();
            p->PostInitialize();
            return p;
        }
    };

    class D : public B { /* ... */ };            // some derived class

    shared_ptr<D> p = D::Create<D>();  // creating a D object

By making the constructor `protected` we avoid an incompletely constructed object escaping into the wild.
By providing the factory function `Create()`, we make construction (on the free store) convenient.

##### Note

Conventional factory functions allocate on the free store, rather than on the stack or in an enclosing object.

**See also**: [Discussion](#Sd-factory)

### <a name="Rc-delegating"></a>C.51: Use delegating constructors to represent common actions for all constructors of a class

##### Reason

To avoid repetition and accidental differences.

##### Example, bad

    class Date {   // BAD: repetitive
        int d;
        Month m;
        int y;
    public:
        Date(int ii, Month mm, year yy)
            :i{ii}, m{mm}, y{yy}
            { if (!valid(i, m, y)) throw Bad_date{}; }

        Date(int ii, Month mm)
            :i{ii}, m{mm} y{current_year()}
            { if (!valid(i, m, y)) throw Bad_date{}; }
        // ...
    };

The common action gets tedious to write and may accidentally not be common.

##### Example

    class Date2 {
        int d;
        Month m;
        int y;
    public:
        Date2(int ii, Month mm, year yy)
            :i{ii}, m{mm}, y{yy}
            { if (!valid(i, m, y)) throw Bad_date{}; }

        Date2(int ii, Month mm)
            :Date2{ii, mm, current_year()} {}
        // ...
    };

**See also**: If the "repeated action" is a simple initialization, consider [an in-class member initializer](#Rc-in-class-initializer).

##### Enforcement

(Moderate) Look for similar constructor bodies.

### <a name="Rc-inheriting"></a>C.52: Use inheriting constructors to import constructors into a derived class that does not need further explicit initialization

##### Reason

If you need those constructors for a derived class, re-implementing them is tedious and error prone.

##### Example

`std::vector` has a lot of tricky constructors, so if I want my own `vector`, I don't want to reimplement them:

    class Rec {
        // ... data and lots of nice constructors ...
    };

    class Oper : public Rec {
        using Rec::Rec;
        // ... no data members ...
        // ... lots of nice utility functions ...
    };

##### Example, bad

    struct Rec2 : public Rec {
        int x;
        using Rec::Rec;
    };

    Rec2 r {"foo", 7};
    int val = r.x;   // uninitialized

##### Enforcement

Make sure that every member of the derived class is initialized.

## <a name="SS-copy"></a>C.copy: Copy and move

Value types should generally be copyable, but interfaces in a class hierarchy should not.
Resource handles may or may not be copyable.
Types can be defined to move for logical as well as performance reasons.

### <a name="Rc-copy-assignment"></a>C.60: Make copy assignment non-`virtual`, take the parameter by `const&`, and return by non-`const&`

##### Reason

It is simple and efficient. If you want to optimize for rvalues, provide an overload that takes a `&&` (see [F.24](#Rf-pass-ref-ref)).

##### Example

    class Foo {
    public:
        Foo& operator=(const Foo& x)
        {
            // GOOD: no need to check for self-assignment (other than performance)
            auto tmp = x;
            std::swap(*this, tmp);
            return *this;
        }
        // ...
    };

    Foo a;
    Foo b;
    Foo f();

    a = b;    // assign lvalue: copy
    a = f();  // assign rvalue: potentially move

##### Note

The `swap` implementation technique offers the [strong guarantee](???).

##### Example

But what if you can get significantly better performance by not making a temporary copy? Consider a simple `Vector` intended for a domain where assignment of large, equal-sized `Vector`s is common. In this case, the copy of elements implied by the `swap` implementation technique could cause an order of magnitude increase in cost:

    template<typename T>
    class Vector {
    public:
        Vector& operator=(const Vector&);
        // ...
    private:
        T* elem;
        int sz;
    };

    Vector& Vector::operator=(const Vector& a)
    {
        if (a.sz > sz) {
            // ... use the swap technique, it can't be bettered ...
            return *this
        }
        // ... copy sz elements from *a.elem to elem ...
        if (a.sz < sz) {
            // ... destroy the surplus elements in *this* and adjust size ...
        }
        return *this;
    }

By writing directly to the target elements, we will get only [the basic guarantee](#???) rather than the strong guarantee offered by the `swap` technique. Beware of [self assignment](#Rc-copy-self).

**Alternatives**: If you think you need a `virtual` assignment operator, and understand why that's deeply problematic, don't call it `operator=`. Make it a named function like `virtual void assign(const Foo&)`.
See [copy constructor vs. `clone()`](#Rc-copy-virtual).

##### Enforcement

* (Simple) An assignment operator should not be virtual. Here be dragons!
* (Simple) An assignment operator should return `T&` to enable chaining, not alternatives like `const T&` which interfere with composability and putting objects in containers.
* (Moderate) An assignment operator should (implicitly or explicitly) invoke all base and member assignment operators.
  Look at the destructor to determine if the type has pointer semantics or value semantics.

### <a name="Rc-copy-semantic"></a>C.61: A copy operation should copy

##### Reason

That is the generally assumed semantics. After `x = y`, we should have `x == y`.
After a copy `x` and `y` can be independent objects (value semantics, the way non-pointer built-in types and the standard-library types work) or refer to a shared object (pointer semantics, the way pointers work).

##### Example

    class X {   // OK: value semantics
    public:
        X();
        X(const X&);     // copy X
        void modify();   // change the value of X
        // ...
        ~X() { delete[] p; }
    private:
        T* p;
        int sz;
    };

    bool operator==(const X& a, const X& b)
    {
        return a.sz == b.sz && equal(a.p, a.p + a.sz, b.p, b.p + b.sz);
    }

    X::X(const X& a)
        :p{new T[a.sz]}, sz{a.sz}
    {
        copy(a.p, a.p + sz, a.p);
    }

    X x;
    X y = x;
    if (x != y) throw Bad{};
    x.modify();
    if (x == y) throw Bad{};   // assume value semantics

##### Example

    class X2 {  // OK: pointer semantics
    public:
        X2();
        X2(const X&) = default; // shallow copy
        ~X2() = default;
        void modify();          // change the value of X
        // ...
    private:
        T* p;
        int sz;
    };

    bool operator==(const X2& a, const X2& b)
    {
        return a.sz == b.sz && a.p == b.p;
    }

    X2 x;
    X2 y = x;
    if (x != y) throw Bad{};
    x.modify();
    if (x != y) throw Bad{};  // assume pointer semantics

##### Note

Prefer copy semantics unless you are building a "smart pointer". Value semantics is the simplest to reason about and what the standard library facilities expect.

##### Enforcement

(Not enforceable)

### <a name="Rc-copy-self"></a>C.62: Make copy assignment safe for self-assignment

##### Reason

If `x = x` changes the value of `x`, people will be surprised and bad errors will occur (often including leaks).

##### Example

The standard-library containers handle self-assignment elegantly and efficiently:

    std::vector<int> v = {3, 1, 4, 1, 5, 9};
    v = v;
    // the value of v is still {3, 1, 4, 1, 5, 9}

##### Note

The default assignment generated from members that handle self-assignment correctly handles self-assignment.

    struct Bar {
        vector<pair<int, int>> v;
        map<string, int> m;
        string s;
    };

    Bar b;
    // ...
    b = b;   // correct and efficient

##### Note

You can handle self-assignment by explicitly testing for self-assignment, but often it is faster and more elegant to cope without such a test (e.g., [using `swap`](#Rc-swap)).

    class Foo {
        string s;
        int i;
    public:
        Foo& operator=(const Foo& a);
        // ...
    };

    Foo& Foo::operator=(const Foo& a)   // OK, but there is a cost
    {
        if (this == &a) return *this;
        s = a.s;
        i = a.i;
        return *this;
    }

This is obviously safe and apparently efficient.
However, what if we do one self-assignment per million assignments?
That's about a million redundant tests (but since the answer is essentially always the same, the computer's branch predictor will guess right essentially every time).
Consider:

    Foo& Foo::operator=(const Foo& a)   // simpler, and probably much better
    {
        s = a.s;
        i = a.i;
        return *this;
    }

`std::string` is safe for self-assignment and so are `int`. All the cost is carried by the (rare) case of self-assignment.

##### Enforcement

(Simple) Assignment operators should not contain the pattern `if (this == &a) return *this;` ???

### <a name="Rc-move-assignment"></a>C.63: Make move assignment non-`virtual`, take the parameter by `&&`, and return by non-`const &`

##### Reason

It is simple and efficient.

**See**: [The rule for copy-assignment](#Rc-copy-assignment).

##### Enforcement

Equivalent to what is done for [copy-assignment](#Rc-copy-assignment).

* (Simple) An assignment operator should not be virtual. Here be dragons!
* (Simple) An assignment operator should return `T&` to enable chaining, not alternatives like `const T&` which interfere with composability and putting objects in containers.
* (Moderate) A move assignment operator should (implicitly or explicitly) invoke all base and member move assignment operators.

### <a name="Rc-move-semantic"></a>C.64: A move operation should move and leave its source in a valid state

##### Reason

That is the generally assumed semantics.
After `y = std::move(x)` the value of `y` should be the value `x` had and `x` should be in a valid state.

##### Example

    template<typename T>
    class X {   // OK: value semantics
    public:
        X();
        X(X&& a);          // move X
        void modify();     // change the value of X
        // ...
        ~X() { delete[] p; }
    private:
        T* p;
        int sz;
    };


    X::X(X&& a)
        :p{a.p}, sz{a.sz}  // steal representation
    {
        a.p = nullptr;     // set to "empty"
        a.sz = 0;
    }

    void use()
    {
        X x{};
        // ...
        X y = std::move(x);
        x = X{};   // OK
    } // OK: x can be destroyed

##### Note

Ideally, that moved-from should be the default value of the type.
Ensure that unless there is an exceptionally good reason not to.
However, not all types have a default value and for some types establishing the default value can be expensive.
The standard requires only that the moved-from object can be destroyed.
Often, we can easily and cheaply do better: The standard library assumes that it it possible to assign to a moved-from object.
Always leave the moved-from object in some (necessarily specified) valid state.

##### Note

Unless there is an exceptionally strong reason not to, make `x = std::move(y); y = z;` work with the conventional semantics.

##### Enforcement

(Not enforceable) Look for assignments to members in the move operation. If there is a default constructor, compare those assignments to the initializations in the default constructor.

### <a name="Rc-move-self"></a>C.65: Make move assignment safe for self-assignment

##### Reason

If `x = x` changes the value of `x`, people will be surprised and bad errors may occur. However, people don't usually directly write a self-assignment that turn into a move, but it can occur. However, `std::swap` is implemented using move operations so if you accidentally do `swap(a, b)` where `a` and `b` refer to the same object, failing to handle self-move could be a serious and subtle error.

##### Example

    class Foo {
        string s;
        int i;
    public:
        Foo& operator=(Foo&& a);
        // ...
    };

    Foo& Foo::operator=(Foo&& a)       // OK, but there is a cost
    {
        if (this == &a) return *this;  // this line is redundant
        s = std::move(a.s);
        i = a.i;
        return *this;
    }

The one-in-a-million argument against `if (this == &a) return *this;` tests from the discussion of [self-assignment](#Rc-copy-self) is even more relevant for self-move.

##### Note

There is no know general way of avoiding a `if (this == &a) return *this;` test for a move assignment and still get a correct answer (i.e., after `x = x` the value of `x` is unchanged).

##### Note

The ISO standard guarantees only a "valid but unspecified" state for the standard library containers. Apparently this has not been a problem in about 10 years of experimental and production use. Please contact the editors if you find a counter example. The rule here is more caution and insists on complete safety.

##### Example

Here is a way to move a pointer without a test (imagine it as code in the implementation a move assignment):

    // move from other.ptr to this->ptr
    T* temp = other.ptr;
    other.ptr = nullptr;
    delete ptr;
    ptr = temp;

##### Enforcement

* (Moderate) In the case of self-assignment, a move assignment operator should not leave the object holding pointer members that have been `delete`d or set to `nullptr`.
* (Not enforceable) Look at the use of standard-library container types (incl. `string`) and consider them safe for ordinary (not life-critical) uses.

### <a name="Rc-move-noexcept"></a>C.66: Make move operations `noexcept`

##### Reason

A throwing move violates most people's reasonably assumptions.
A non-throwing move will be used more efficiently by standard-library and language facilities.

##### Example

    template<typename T>
    class Vector {
        // ...
        Vector(Vector&& a) noexcept :elem{a.elem}, sz{a.sz} { a.sz = 0; a.elem = nullptr; }
        Vector& operator=(Vector&& a) noexcept { elem = a.elem; sz = a.sz; a.sz = 0; a.elem = nullptr; }
        // ...
    public:
        T* elem;
        int sz;
    };

These copy operations do not throw.

##### Example, bad

    template<typename T>
    class Vector2 {
        // ...
        Vector2(Vector2&& a) { *this = a; }             // just use the copy
        Vector2& operator=(Vector2&& a) { *this = a; }  // just use the copy
        // ...
    public:
        T* elem;
        int sz;
    };

This `Vector2` is not just inefficient, but since a vector copy requires allocation, it can throw.

##### Enforcement

(Simple) A move operation should be marked `noexcept`.

### <a name="Rc-copy-virtual"></a>C.67: A base class should suppress copying, and provide a virtual `clone` instead if "copying" is desired

##### Reason

To prevent slicing, because the normal copy operations will copy only the base portion of a derived object.

##### Example, bad

    class B { // BAD: base class doesn't suppress copying
        int data;
        // ... nothing about copy operations, so uses default ...
    };

    class D : public B {
        string more_data; // add a data member
        // ...
    };

    auto d = make_unique<D>();

    // oops, slices the object; gets only d.data but drops d.more_data
    auto b = make_unique<B>(d);

##### Example

    class B { // GOOD: base class suppresses copying
        B(const B&) = delete;
        B& operator=(const B&) = delete;
        virtual unique_ptr<B> clone() { return /* B object */; }
        // ...
    };

    class D : public B {
        string more_data; // add a data member
        unique_ptr<B> clone() override { return /* D object */; }
        // ...
    };

    auto d = make_unique<D>();
    auto b = d.clone(); // ok, deep clone

##### Note

It's good to return a smart pointer, but unlike with raw pointers the return type cannot be covariant (for example, `D::clone` can't return a `unique_ptr<D>`. Don't let this tempt you into returning an owning raw pointer; this is a minor drawback compared to the major robustness benefit delivered by the owning smart pointer.

##### Exception

If you need covariant return types, return an `owner<derived*>`. See [C.130](#Rh-copy).

##### Enforcement

A class with any virtual function should not have a copy constructor or copy assignment operator (compiler-generated or handwritten).

## C.other: Other default operation rules

In addition to the operations for which the language offer default implementations,
there are a few operations that are so foundational that it rules for their definition are needed:
comparisons, `swap`, and `hash`.

### <a name="Rc-eqdefault"></a>C.80: Use `=default` if you have to be explicit about using the default semantics

##### Reason

The compiler is more likely to get the default semantics right and you cannot implement these function better than the compiler.

##### Example

    class Tracer {
        string message;
    public:
        Tracer(const string& m) : message{m} { cerr << "entering " << message << '\n'; }
        ~Tracer() { cerr << "exiting " << message << '\n'; }

        Tracer(const Tracer&) = default;
        Tracer& operator=(const Tracer&) = default;
        Tracer(Tracer&&) = default;
        Tracer& operator=(Tracer&&) = default;
    };

Because we defined the destructor, we must define the copy and move operations. The `= default` is the best and simplest way of doing that.

##### Example, bad

    class Tracer2 {
        string message;
    public:
        Tracer2(const string& m) : message{m} { cerr << "entering " << message << '\n'; }
        ~Tracer2() { cerr << "exiting " << message << '\n'; }

        Tracer2(const Tracer2& a) : message{a.message} {}
        Tracer2& operator=(const Tracer2& a) { message = a.message; return *this; }
        Tracer2(Tracer2&& a) :message{a.message} {}
        Tracer2& operator=(Tracer2&& a) { message = a.message; return *this; }
    };

Writing out the bodies of the copy and move operations is verbose, tedious, and error-prone. A compiler does it better.

##### Enforcement

(Moderate) The body of a special operation should not have the same accessibility and semantics as the compiler-generated version, because that would be redundant

### <a name="Rc-delete"></a>C.81: Use `=delete` when you want to disable default behavior (without wanting an alternative)

##### Reason

In a few cases, a default operation is not desirable.

##### Example

    class Immortal {
    public:
        ~Immortal() = delete;   // do not allow destruction
        // ...
    };

    void use()
    {
        Immortal ugh;   // error: ugh cannot be destroyed
        Immortal* p = new Immortal{};
        delete p;       // error: cannot destroy *p
    }

##### Example

A `unique_ptr` can be moved, but not copied. To achieve that its copy operations are deleted. To avoid copying it is necessary to `=delete` its copy operations from lvalues:

    template <class T, class D = default_delete<T>> class unique_ptr {
    public:
        // ...
        constexpr unique_ptr() noexcept;
        explicit unique_ptr(pointer p) noexcept;
        // ...
        unique_ptr(unique_ptr&& u) noexcept;   // move constructor
        // ...
        unique_ptr(const unique_ptr&) = delete; // disable copy from lvalue
        // ...
    };

    unique_ptr<int> make();   // make "something" and return it by moving

    void f()
    {
        unique_ptr<int> pi {};
        auto pi2 {pi};      // error: no move constructor from lvalue
        auto pi3 {make()};  // OK, move: the result of make() is an rvalue
    }

##### Enforcement

The elimination of a default operation is (should be) based on the desired semantics of the class. Consider such classes suspect, but maintain a "positive list" of classes where a human has asserted that the semantics is correct.

### <a name="Rc-ctor-virtual"></a>C.82: Don't call virtual functions in constructors and destructors

##### Reason

The function called will be that of the object constructed so far, rather than a possibly overriding function in a derived class.
This can be most confusing.
Worse, a direct or indirect call to an unimplemented pure virtual function from a constructor or destructor results in undefined behavior.

##### Example, bad

    class Base {
    public:
        virtual void f() = 0;   // not implemented
        virtual void g();       // implemented with Base version
        virtual void h();       // implemented with Base version
    };

    class Derived : public Base {
    public:
        void g() override;   // provide Derived implementation
        void h() final;      // provide Derived implementation

        Derived()
        {
            // BAD: attempt to call an unimplemented virtual function
            f();

            // BAD: will call Derived::g, not dispatch further virtually
            g();

            // GOOD: explicitly state intent to call only the visible version
            Derived::g();

            // ok, no qualification needed, h is final
            h();
        }
    };

Note that calling a specific explicitly qualified function is not a virtual call even if the function is `virtual`.

**See also** [factory functions](#Rc-factory) for how to achieve the effect of a call to a derived class function without risking undefined behavior.

##### Note

There is nothing inherently wrong with calling virtual functions from constructors and destructors.
The semantics of such calls is type safe.
However, experience shows that such calls are rarely needed, easily confuse maintainers, and become a source of errors when used by novices.

##### Enforcement

* Flag calls of virtual functions from constructors and destructors.

### <a name="Rc-swap"></a>C.83: For value-like types, consider providing a `noexcept` swap function

##### Reason

A `swap` can be handy for implementing a number of idioms, from smoothly moving objects around to implementing assignment easily to providing a guaranteed commit function that enables strongly error-safe calling code. Consider using swap to implement copy assignment in terms of copy construction. See also [destructors, deallocation, and swap must never fail](#Re-never-fail).

##### Example, good

    class Foo {
        // ...
    public:
        void swap(Foo& rhs) noexcept
        {
            m1.swap(rhs.m1);
            std::swap(m2, rhs.m2);
        }
    private:
        Bar m1;
        int m2;
    };

Providing a nonmember `swap` function in the same namespace as your type for callers' convenience.

    void swap(Foo& a, Foo& b)
    {
        a.swap(b);
    }

##### Enforcement

* (Simple) A class without virtual functions should have a `swap` member function declared.
* (Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-swap-fail"></a>C.84: A `swap` function may not fail

##### Reason

 `swap` is widely used in ways that are assumed never to fail and programs cannot easily be written to work correctly in the presence of a failing `swap`. The standard-library containers and algorithms will not work correctly if a swap of an element type fails.

##### Example, bad

    void swap(My_vector& x, My_vector& y)
    {
        auto tmp = x;   // copy elements
        x = y;
        y = tmp;
    }

This is not just slow, but if a memory allocation occurs for the elements in `tmp`, this `swap` may throw and would make STL algorithms fail if used with them.

##### Enforcement

(Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-swap-noexcept"></a>C.85: Make `swap` `noexcept`

##### Reason

 [A `swap` may not fail](#Rc-swap-fail).
If a `swap` tries to exit with an exception, it's a bad design error and the program had better terminate.

##### Enforcement

(Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-eq"></a>C.86: Make `==` symmetric with respect to operand types and `noexcept`

##### Reason

Asymmetric treatment of operands is surprising and a source of errors where conversions are possible.
`==` is a fundamental operations and programmers should be able to use it without fear of failure.

##### Example

    class X {
        string name;
        int number;
    };

    bool operator==(const X& a, const X& b) noexcept {
        return a.name == b.name && a.number == b.number;
    }

##### Example, bad

    class B {
        string name;
        int number;
        bool operator==(const B& a) const {
            return name == a.name && number == a.number;
        }
        // ...
    };

`B`'s comparison accepts conversions for its second operand, but not its first.

##### Note

If a class has a failure state, like `double`'s `NaN`, there is a temptation to make a comparison against the failure state throw.
The alternative is to make two failure states compare equal and any valid state compare false against the failure state.

#### Note

This rule applies to all the usual comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

##### Enforcement

* Flag an `operator==()` for which the argument types differ; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.
* Flag member `operator==()`s; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

### <a name="Rc-eq-base"></a>C.87: Beware of `==` on base classes

##### Reason

It is really hard to write a foolproof and useful `==` for a hierarchy.

##### Example, bad

    class B {
        string name;
        int number;
        virtual bool operator==(const B& a) const
        {
             return name == a.name && number == a.number;
        }
        // ...
    };

`B`'s comparison accepts conversions for its second operand, but not its first.

    class D :B {
        char character;
        virtual bool operator==(const D& a) const
        {
            return name == a.name && number == a.number && character == a.character;
        }
        // ...
    };

    B b = ...
    D d = ...
    b == d;    // compares name and number, ignores d's character
    d == b;    // error: no == defined
    D d2;
    d == d2;   // compares name, number, and character
    B& b2 = d2;
    b2 == d;   // compares name and number, ignores d2's and d's character

Of course there are ways of making `==` work in a hierarchy, but the naive approaches do not scale

#### Note

This rule applies to all the usual comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

##### Enforcement

* Flag a virtual `operator==()`; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

### <a name="Rc-hash"></a>C.89: Make a `hash` `noexcept`

##### Reason

Users of hashed containers use hash indirectly and don't expect simple access to throw.
It's a standard-library requirement.

##### Example, bad

    template<>
    struct hash<My_type> {  // thoroughly bad hash specialization
        using result_type = size_t;
        using argument_type = My_type;

        size_t operator() (const My_type & x) const
        {
            size_t xs = x.s.size();
            if (xs < 4) throw Bad_My_type{};    // "Nobody expects the Spanish inquisition!"
            return hash<size_t>()(x.s.size()) ^ trim(x.s);
        }
    };

    int main()
    {
        unordered_map<My_type, int> m;
        My_type mt{ "asdfg" };
        m[mt] = 7;
        cout << m[My_type{ "asdfg" }] << '\n';
    }

If you have to define a `hash` specialization, try simply to let it combine standard-library `hash` specializations with `^` (xor).
That tends to work better than "cleverness" for non-specialists.

##### Enforcement

* Flag throwing `hash`es.

## <a name="SS-containers"></a>C.con: Containers and other resource handles

A container is an object holding a sequence of objects of some type; `std::vector` is the archetypical container.
A resource handle is a class that owns a resource; `std::vector` is the typical resource handle; its resource is its sequence of elements.

Summary of container rules:

* [C.100: Follow the STL when defining a container](#Rcon-stl)
* [C.101: Give a container value semantics](#Rcon-val)
* [C.102: Give a container move operations](#Rcon-move)
* [C.103: Give a container an initializer list constructor](#Rcon-init)
* [C.104: Give a container a default constructor that sets it to empty](#Rcon-empty)
* [C.105: Give a constructor and `Extent` constructor](#Rcon-val)
* ???
* [C.109: If a resource handle has pointer semantics, provide `*` and `->`](#rcon-ptr)

**See also**: [Resources](#S-resource)

## <a name="SS-lambdas"></a>C.lambdas: Function objects and lambdas

A function object is an object supplying an overloaded `()` so that you can call it.
A lambda expression (colloquially often shortened to "a lambda") is a notation for generating a function object.
Function objects should be cheap to copy (and therefore [passed by value](#Rf-in)).

Summary:

* [F.50: Use a lambda when a function won't do (to capture local variables, or to write a local function)](#Rf-capture-vs-overload)
* [F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms](#Rf-reference-capture)
* [F.53: Avoid capturing by reference in lambdas that will be used nonlocally, including returned, stored on the heap, or passed to another thread](#Rf-value-capture)
* [ES.28: Use lambdas for complex initialization, especially of `const` variables](#Res-lambda-init)

## <a name="SS-hier"></a>C.hier: Class hierarchies (OOP)

A class hierarchy is constructed to represent a set of hierarchically organized concepts (only).
Typically base classes act as interfaces.
There are two major uses for hierarchies, often named implementation inheritance and interface inheritance.

Class hierarchy rule summary:

* [C.120: Use class hierarchies to represent concepts with inherent hierarchical structure (only)](#Rh-domain)
* [C.121: If a base class is used as an interface, make it a pure abstract class](#Rh-abstract)
* [C.122: Use abstract classes as interfaces when complete separation of interface and implementation is needed](#Rh-separation)

Designing rules for classes in a hierarchy summary:

* [C.126: An abstract class typically doesn't need a constructor](#Rh-abstract-ctor)
* [C.127: A class with a virtual function should have a virtual or protected destructor](#Rh-dtor)
* [C.128: Use `override` to make overriding explicit in large class hierarchies](#Rh-override)
* [C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance](#Rh-kind)
* [C.130: Redefine or prohibit copying for a base class; prefer a virtual `clone` function instead](#Rh-copy)
* [C.131: Avoid trivial getters and setters](#Rh-get)
* [C.132: Don't make a function `virtual` without reason](#Rh-virtual)
* [C.133: Avoid `protected` data](#Rh-protected)
* [C.134: Ensure all non-`const` data members have the same access level](#Rh-public)
* [C.135: Use multiple inheritance to represent multiple distinct interfaces](#Rh-mi-interface)
* [C.136: Use multiple inheritance to represent the union of implementation attributes](#Rh-mi-implementation)
* [C.137: Use `virtual` bases to avoid overly general base classes](#Rh-vbase)
* [C.138: Create an overload set for a derived class and its bases with `using`](#Rh-using)
* [C.139: Use `final` sparingly](#Rh-final)
* [C.140: Do not provide different default arguments for a virtual function and an overrider](#Rh-virtual-default-arg)

Accessing objects in a hierarchy rule summary:

* [C.145: Access polymorphic objects through pointers and references](#Rh-poly)
* [C.146: Use `dynamic_cast` where class hierarchy navigation is unavoidable](#Rh-dynamic_cast)
* [C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error](#Rh-ptr-cast)
* [C.148: Use `dynamic_cast` to a pointer type when failure to find the required class is considered a valid alternative](#Rh-ref-cast)
* [C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`](#Rh-smart)
* [C.150: Use `make_unique()` to construct objects owned by `unique_ptr`s](#Rh-make_unique)
* [C.151: Use `make_shared()` to construct objects owned by `shared_ptr`s](#Rh-make_shared)
* [C.152: Never assign a pointer to an array of derived class objects to a pointer to its base](#Rh-array)

### <a name="Rh-domain"></a>C.120: Use class hierarchies to represent concepts with inherent hierarchical structure (only)

##### Reason

Direct representation of ideas in code eases comprehension and maintenance. Make sure the idea represented in the base class exactly matches all derived types and there is not a better way to express it than using the tight coupling of inheritance.

Do *not* use inheritance when simply having a data member will do. Usually this means that the derived type needs to override a base virtual function or needs access to a protected member.

##### Example

    ??? Good old Shape example?

##### Example, bad

Do *not* represent non-hierarchical domain concepts as class hierarchies.

    template<typename T>
    class Container {
    public:
        // list operations:
        virtual T& get() = 0;
        virtual void put(T&) = 0;
        virtual void insert(Position) = 0;
        // ...
        // vector operations:
        virtual T& operator[](int) = 0;
        virtual void sort() = 0;
        // ...
        // tree operations:
        virtual void balance() = 0;
        // ...
    };

Here most overriding classes cannot implement most of the functions required in the interface well.
Thus the base class becomes an implementation burden.
Furthermore, the user of `Container` cannot rely on the member functions actually performing a meaningful operations reasonably efficiently;
it may throw an exception instead.
Thus users have to resort to run-time checking and/or
not using this (over)general interface in favor of a particular interface found by a run-time type inquiry (e.g., a `dynamic_cast`).

##### Enforcement

* Look for classes with lots of members that do nothing but throw.
* Flag every use of a nonpublic base class `B` where the derived class `D` does not override a virtual function or access a protected member in `B`, and `B` is not one of the following: empty, a template parameter or parameter pack of `D`, a class template specialized with `D`.

### <a name="Rh-abstract"></a>C.121: If a base class is used as an interface, make it a pure abstract class

##### Reason

A class is more stable (less brittle) if it does not contain data.
Interfaces should normally be composed entirely of public pure virtual functions and a default/empty virtual destructor.

##### Example

    class My_interface {
    public:
        // ...only pure virtual functions here ...
        virtual ~My_interface() {}   // or =default
    };

##### Example, bad

    class Goof {
    public:
        // ...only pure virtual functions here ...
        // no virtual destructor
    };

    class Derived : public Goof {
        string s;
        // ...
    };

    void use()
    {
        unique_ptr<Goof> p {new Derived{"here we go"}};
        f(p.get()); // use Derived through the Goof interface
        g(p.get()); // use Derived through the Goof interface
    } // leak

The `Derived` is `delete`d through its `Goof` interface, so its `string` is leaked.
Give `Goof` a virtual destructor and all is well.


##### Enforcement

* Warn on any class that contains data members and also has an overridable (non-`final`) virtual function.

### <a name="Rh-separation"></a>C.122: Use abstract classes as interfaces when complete separation of interface and implementation is needed

##### Reason

Such as on an ABI (link) boundary.

##### Example

    struct Device {
        virtual void write(span<const char> outbuf) = 0;
        virtual void read(span<char> inbuf) = 0;
    };

    class D1 : public Device {
        // ... data ...

        void write(span<const char> outbuf) override;
        void read(span<char> inbuf) override;
    };

    class D2 : public Device {
        // ... different data ...

        void write(span<const char> outbuf) override;
        void read(span<char> inbuf) override;
    };

A user can now use `D1`s and `D2`s interchangeably through the interface provided by `Device`.
Furthermore, we can update `D1` and `D2` in a ways that are not binary compatible with older versions as long as all access goes through `Device`.

##### Enforcement

    ???

## C.hierclass: Designing classes in a hierarchy:

### <a name="Rh-abstract-ctor"></a>C.126: An abstract class typically doesn't need a constructor

##### Reason

An abstract class typically does not have any data for a constructor to initialize.

##### Example

    ???

##### Exception

* A base class constructor that does work, such as registering an object somewhere, may need a constructor.
* In extremely rare cases, you might find it reasonable for an abstract class to have a bit of data shared by all derived classes
  (e.g., use statistics data, debug information, etc.); such classes tend to have constructors. But be warned: Such classes also tend to be prone to requiring virtual inheritance.

##### Enforcement

Flag abstract classes with constructors.

### <a name="Rh-dtor"></a>C.127: A class with a virtual function should have a virtual or protected destructor

##### Reason

A class with a virtual function is usually (and in general) used via a pointer to base. Usually, the last user has to call delete on a pointer to base, often via a smart pointer to base, so the destructor should be public and virtual. Less commonly, if deletion through a pointer to base is not intended to be supported, the destructor should be protected and nonvirtual; see [C.35](#Rc-dtor-virtual).

##### Example, bad

    struct B {
        virtual int f() = 0;
        // ... no user-written destructor, defaults to public nonvirtual ...
    };

    // bad: class with a resource derived from a class without a virtual destructor
    struct D : B {
        string s {"default"};
    };

    void use()
    {
        auto p = make_unique<D>();
        // ...
    } // calls B::~B only, leaks the string

##### Note

There are people who don't follow this rule because they plan to use a class only through a `shared_ptr`: `std::shared_ptr<B> p = std::make_shared<D>(args);` Here, the shared pointer will take care of deletion, so no leak will occur from an inappropriate `delete` of the base. People who do this consistently can get a false positive, but the rule is important -- what if one was allocated using `make_unique`? It's not safe unless the author of `B` ensures that it can never be misused, such as by making all constructors private and providing a factory function to enforce the allocation with `make_shared`.

##### Enforcement

* A class with any virtual functions should have a destructor that is either public and virtual or else protected and nonvirtual.
* Flag `delete` of a class with a virtual function but no virtual destructor.

### <a name="Rh-override"></a>C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`

##### Reason

Readability.
Detection of mistakes.
Writing explicit `virtual`, `override`, or `final` is self-documenting and enables the compiler to catch mismatch of types and/or names between base and derived classes. However, writing more than one of these three is both redundant and a potential source of errors.

Use `virtual` only when declaring a new virtual function. Use `override` only when declaring an overrider. Use `final` only when declaring an final overrider. If a base class destructor is declared `virtual`, derived class destructors should neither be declared `virtual` nor `override`.

##### Example, bad

    struct B {
        void f1(int);
        virtual void f2(int) const;
        virtual void f3(int);
        // ...
    };

    struct D : B {
        void f1(int);        // bad (hope for a warning): D::f1() hides B::f1()
        void f2(int) const;  // bad (but conventional and valid): no explicit override
        void f3(double);     // bad (hope for a warning): D::f3() hides B::f3()
        // ...
    };

    struct Better : B {
        void f1(int) override;        // error (caught): D::f1() hides B::f1()
        void f2(int) const override;
        void f3(double) override;     // error (caught): D::f3() hides B::f3()
        // ...
    };

##### Enforcement

* Compare names in base and derived classes and flag uses of the same name that does not override.
* Flag overrides with neither `override` nor `final`.
* Flag function declarations that use more than one of `virtual`, `override`, and `final`.

### <a name="Rh-kind"></a>C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance

##### Reason

Implementation details in an interface makes the interface brittle;
that is, makes its users vulnerable to having to recompile after changes in the implementation.
Data in a base class increases the complexity of implementing the base and can lead to replication of code.

##### Note

Definition:

* interface inheritance is the use of inheritance to separate users from implementations,
in particular to allow derived classes to be added and changed without affecting the users of base classes.
* implementation inheritance is the use of inheritance to simplify implementation of new facilities
by making useful operations available for implementers of related new operations (sometimes called "programming by difference").

A pure interface class is simply a set of pure virtual functions; see [I.25](#Ri-abstract).

In early OOP (e.g., in the 1980s and 1990s), implementation inheritance and interface inheritance were often mixed
and bad habits die hard.
Even now, mixtures are not uncommon in old code bases and in old-style teaching material.

The importance of keeping the two kinds of inheritance increases

* with the size of a hierarchy (e.g., dozens of derived classes),
* with the length of time the hierarchy is used (e.g., decades), and
* with the number of distinct organizations in which a hierarchy is used
(e.g., it can be difficult to distribute an update to a base class)


##### Example, bad

    class Shape {   // BAD, mixed interface and implementation
    public:
        Shape();
        Shape(Point ce = {0, 0}, Color co = none): cent{ce}, col {co} { /* ... */}

        Point center() const { return cent; }
        Color color() const { return col; }

        virtual void rotate(int) = 0;
        virtual void move(Point p) { cent = p; redraw(); }

        virtual void redraw();

        // ...
    public:
        Point cent;
        Color col;
    };

    class Circle : public Shape {
    public:
        Circle(Point c, int r) :Shape{c}, rad{r} { /* ... */ }

        // ...
    private:
        int rad;
    };

    class Triangle : public Shape {
    public:
        Triangle(Point p1, Point p2, Point p3); // calculate center
        // ...
    };

Problems:

* As the hierarchy grows and more data is adder to `Shape`, the constructors gets harder to write and maintain.
* Why calculate the center for the `Triangle`? we may never us it.
* Add a data member to `Shape` (e.g., drawing style or canvas)
and all derived classes and all users needs to be reviewed, possibly changes, and probably recompiled.

The implementation of `Shape::move()` is an example of implementation inheritance:
we have defined `move()` once and for all for all derived classes.
The more code there is in such base class member function implementations and the more data is shared by placing it in the base,
the more benefits we gain - and the less stable the hierarchy is.

##### Example

This Shape hierarchy can be rewritten using interface inheritance:

    class Shape {  // pure interface
    public:
        virtual Point center() const = 0;
        virtual Color color() const = 0;

        virtual void rotate(int) = 0;
        virtual void move(Point p) = 0;

        virtual void redraw() = 0;

        // ...
    };

Note that a pure interface rarely have constructors: there is nothing to construct.

    class Circle : public Shape {
    public:
        Circle(Point c, int r, Color c) :cent{c}, rad{r}, col{c} { /* ... */ }

        Point center() const override { return cent; }
        Color color() const override { return col; }

        // ...
    private:
        Point cent;
        int rad;
        Color col;
    };

The interface is now less brittle, but there is more work in implementing the member functions.
For example, `center` has to be implemented by every class derived from `Shape`.

##### Example, dual hierarchy

How can we gain the benefit of the stable hierarchies from implementation hierarchies and the benefit of implementation reuse from implementation inheritance.
One popular technique is dual hierarchies.
There are many ways of implementing the idea of dual hierarchies; here, we use a multiple-inheritance variant.

First we devise a hierarchy of interface classes:

    class Shape {   // pure interface
    public:
        virtual Point center() const = 0;
        virtual Color color() const = 0;

        virtual void rotate(int) = 0;
        virtual void move(Point p) = 0;

        virtual void redraw() = 0;

        // ...
    };

    class Circle : public Shape {   // pure interface
    public:
        int radius() = 0;
        // ...
    };

To make this interface useful, we must provide its implementation classes (here, named equivalently, but in the `Impl` namespace):

    class Impl::Shape : public Shape { // implementation
    public:
        // constructors, destructor
        // ...
        virtual Point center() const { /* ... */ }
        virtual Color color() const { /* ... */ }

        virtual void rotate(int) { /* ... */ }
        virtual void move(Point p) { /* ... */ }

        virtual void redraw() { /* ... */ }

        // ...
    };

Now `Shape` is a poor example of a class with an implementation,
but bear with us because this is just a simple example of a technique aimed at more complex hierarchies.


    class Impl::Circle : public Circle, public Impl::Shape {   // implementation
    public:
        // constructors, destructor

        int radius() { /* ... */ }
        // ...
    };

And we could extend the hierarchies by adding a Smiley class (:-)):

    class Smiley : public Circle { // pure interface
    public:
        // ...
    };

    class Impl::Smiley : Public Smiley, public Impl::Circle {   // implementation
    public:
        // constructors, destructor
        // ...
    }

There are now two hierarchies:

* interface: Smiley -> Circle -> Shape
* implementation: Impl::Smiley -> Impl::Circle -> Impl::Shape

Since each implementation derived from its interface as well as its implementation base class we get a lattice (DAG):

    Smiley     ->         Circle     ->  Shape
      ^                     ^               ^
      |                     |               |
    Impl::Smiley -> Impl::Circle -> Impl::Shape

As mentioned, this is just one way to construct a dual hierarchy.

Another (related) technique for separating interface and implementation is [PIMPL](#???).

##### Note

There is often a choice between offering common functionality as (implemented) base class functions and free-standing functions
(in an implementation namespace).
Base classes gives a shorter notation and easier access to shared data (in the base)
at the cost of the functionality being available only to users of the hierarchy.

##### Enforcement

* Flag a derived to base conversion to a base with both data and virtual functions
(except for calls from a derived class member to a base class member)
* ???


### <a name="Rh-copy"></a>C.130: Redefine or prohibit copying for a base class; prefer a virtual `clone` function instead

##### Reason

Copying a base is usually slicing. If you really need copy semantics, copy deeply: Provide a virtual `clone` function that will copy the actual most-derived type and return an owning pointer to the new object, and then in derived classes return the derived type (use a covariant return type).

##### Example

    class Base {
    public:
        virtual owner<Base*> clone() = 0;
        virtual ~Base() = 0;

        Base(const Base&) = delete;
        Base& operator=(const Base&) = delete;
    };

    class Derived : public Base {
    public:
        owner<Derived*> clone() override;
        virtual ~Derived() override;
    };

Note that because of language rules, the covariant return type cannot be a smart pointer. See also [C.67](#Rc-copy-virtual).

##### Enforcement

* Flag a class with a virtual function and a non-user-defined copy operation.
* Flag an assignment of base class objects (objects of a class from which another has been derived).

### <a name="Rh-get"></a>C.131: Avoid trivial getters and setters

##### Reason

A trivial getter or setter adds no semantic value; the data item could just as well be `public`.

##### Example

    class Point {
        int x;
        int y;
    public:
        Point(int xx, int yy) : x{xx}, y{yy} { }
        int get_x() { return x; }
        void set_x(int xx) { x = xx; }
        int get_y() { return y; }
        void set_y(int yy) { y = yy; }
        // no behavioral member functions
    };

Consider making such a class a `struct` -- that is, a behaviorless bunch of variables, all public data and no member functions.

    struct Point {
        int x = 0;
        int y = 0;
    };

##### Note

A getter or a setter that converts from an internal type to an interface type is not trivial (it provides a form of information hiding).

##### Enforcement

Flag multiple `get` and `set` member functions that simply access a member without additional semantics.

### <a name="Rh-virtual"></a>C.132: Don't make a function `virtual` without reason

##### Reason

Redundant `virtual` increases run-time and object-code size.
A virtual function can be overridden and is thus open to mistakes in a derived class.
A virtual function ensures code replication in a templated hierarchy.

##### Example, bad

    template<class T>
    class Vector {
    public:
        // ...
        virtual int size() const { return sz; }   // bad: what good could a derived class do?
    private:
        T* elem;   // the elements
        int sz;    // number of elements
    };

This kind of "vector" isn't meant to be used as a base class at all.

##### Enforcement

* Flag a class with virtual functions but no derived classes.
* Flag a class where all member functions are virtual and have implementations.

### <a name="Rh-protected"></a>C.133: Avoid `protected` data

##### Reason

`protected` data is a source of complexity and errors.
`protected` data complicated the statement of invariants.
`protected` data inherently violates the guidance against putting data in base classes, which usually leads to having to deal virtual inheritance as well.

##### Example

    ???

##### Note

Protected member function can be just fine.

##### Enforcement

Flag classes with `protected` data.

### <a name="Rh-public"></a>C.134: Ensure all non-`const` data members have the same access level

##### Reason

Prevention of logical confusion leading to errors.
If the non-`const` data members don't have the same access level, the type is confused about what it's trying to do.
Is it a type that maintains an invariant or simply a collection of values?

##### Discussion

The core question is: What code is responsible for maintaining a meaningful/correct value for that variable?

There are exactly two kinds of data members:

* A: Ones that don't participate in the object's invariant. Any combination of values for these members is valid.
* B: Ones that do participate in the object's invariant. Not every combination of values is meaningful (else there'd be no invariant). Therefore all code that has write access to these variables must know about the invariant, know the semantics, and know (and actively implement and enforce) the rules for keeping the values correct.

Data members in category A should just be `public` (or, more rarely, `protected` if you only want derived classes to see them). They don't need encapsulation. All code in the system might as well see and manipulate them.

Data members in category B should be `private` or `const`. This is because encapsulation is important. To make them non-`private` and non-`const` would mean that the object can't control its own state: An unbounded amount of code beyond the class would need to know about the invariant and participate in maintaining it accurately -- if these data members were `public`, that would be all calling code that uses the object; if they were `protected`, it would be all the code in current and future derived classes. This leads to brittle and tightly coupled code that quickly becomes a nightmare to maintain. Any code that inadvertently sets the data members to an invalid or unexpected combination of values would corrupt the object and all subsequent uses of the object.

Most classes are either all A or all B:

* *All public*: If you're writing an aggregate bundle-of-variables without an invariant across those variables, then all the variables should be `public`.
  [By convention, declare such classes `struct` rather than `class`](#Rc-struct)
* *All private*: If you're writing a type that maintains an invariant, then all the non-`const` variables should be private -- it should be encapsulated.

##### Exception

Occasionally classes will mix A and B, usually for debug reasons. An encapsulated object may contain something like non-`const` debug instrumentation that isn't part of the invariant and so falls into category A -- it isn't really part of the object's value or meaningful observable state either. In that case, the A parts should be treated as A's (made `public`, or in rarer cases `protected` if they should be visible only to derived classes) and the B parts should still be treated like B's (`private` or `const`).

##### Enforcement

Flag any class that has non-`const` data members with different access levels.

### <a name="Rh-mi-interface"></a>C.135: Use multiple inheritance to represent multiple distinct interfaces

##### Reason

Not all classes will necessarily support all interfaces, and not all callers will necessarily want to deal with all operations. Especially to break apart monolithic interfaces into "aspects" of behavior supported by a given derived class.

##### Example

    ???

##### Note

This is a very common use of inheritance because the need for multiple different interfaces to an implementation is common
and such interfaces are often not easily or naturally organized into a single-rooted hierarchy.

##### Note

Such interfaces are typically abstract classes.

##### Enforcement

???

### <a name="Rh-mi-implementation"></a>C.136: Use multiple inheritance to represent the union of implementation attributes

##### Reason

 ??? Herb: Here's the second mention of implementation inheritance. I'm very skeptical, even of single implementation inheritance, never mind multiple implementation inheritance which just seems frightening -- I don't think that even policy-based design really needs to inherit from the policy types. Am I missing some good examples, or could we consider discouraging this as an anti-pattern?

##### Example

    ???

##### Note

This a relatively rare use because implementation can often be organized into a single-rooted hierarchy.

##### Enforcement

??? Herb: How about opposite enforcement: Flag any type that inherits from more than one non-empty base class?

### <a name="Rh-vbase"></a>C.137: Use `virtual` bases to avoid overly general base classes

##### Reason

 ???

##### Example

    ???

##### Note

???

##### Enforcement

???

### <a name="Rh-using"></a>C.138: Create an overload set for a derived class and its bases with `using`

##### Reason

???

##### Example

    ???

### <a name="Rh-final"></a>C.139: Use `final` sparingly

##### Reason

Capping a hierarchy with `final` is rarely needed for logical reasons and can be damaging to the extensibility of a hierarchy.
Capping an individual virtual function with `final` is error-prone as that `final` can easily be overlooked when defining/overriding a set of functions.

##### Example, bad

    class Widget { /* ... */ };

    // nobody will ever want to improve My_widget (or so you thought)
    class My_widget final : public Widget { /* ... */ };

    class My_improved_widget : public My_widget { /* ... */ };  // error: can't do that

##### Example, bad

    struct Interface {
        virtual int f() = 0;
        virtual int g() = 0;
    };

    class My_implementation : public Interface {
        int f() override;
        int g() final;  // I want g() to be FAST!
        // ...
    };

    class Better_implementation : public My_implementation {
        int f();
        int g();
        // ...
    };

    void use(Interface* p)
    {
        int x = p->f();    // Better_implementation::f()
        int y = p->g();    // My_implementation::g() Surprise?
    }

    // ...

    use(new Better_implementation{});

The problem is easy to see in a small example, but in a large hierarchy with many virtual functions, tools are required for reliably spotting such problems.
Consistent use of `override` would catch this.

##### Note

Claims of performance improvements from `final` should be substantiated.
Too often, such claims are based on conjecture or experience with other languages.

There are examples where `final` can be important for both logical and performance reasons.
One example is a performance-critical AST hierarchy in a compiler or language analysis tool.
New derived classes are not added every year and only by library implementers.
However, misuses are (or at least has been) far more common.

##### Enforcement

Flag uses of `final`.


## <a name="Rh-virtual-default-arg"></a>C.140: Do not provide different default arguments for a virtual function and an overrider

##### Reason

That can cause confusion: An overrider do not inherit default arguments.

##### Example, bad

    class Base {
    public:
        virtual int multiply(int value, int factor = 2) = 0;
    };

    class Derived : public Base {
    public:
        int multiply(int value, int factor = 10) override;
    };

    Derived d;
    Base& b = d;

    b.multiply(10);  // these two calls will call the same function but
    d.multiply(10);  // with different arguments and so different results

##### Enforcement

Flag default arguments on virtual functions if they differ between base and derived declarations.

## C.hier-access: Accessing objects in a hierarchy

### <a name="Rh-poly"></a>C.145: Access polymorphic objects through pointers and references

##### Reason

If you have a class with a virtual function, you don't (in general) know which class provided the function to be used.

##### Example

    struct B { int a; virtual int f(); };
    struct D : B { int b; int f() override; };

    void use(B b)
    {
        D d;
        B b2 = d;   // slice
        B b3 = b;
    }

    void use2()
    {
        D d;
        use(d);   // slice
    }

Both `d`s are sliced.

##### Exception

You can safely access a named polymorphic object in the scope of its definition, just don't slice it.

    void use3()
    {
        D d;
        d.f();   // OK
    }

##### Enforcement

Flag all slicing.

### <a name="Rh-dynamic_cast"></a>C.146: Use `dynamic_cast` where class hierarchy navigation is unavoidable

##### Reason

`dynamic_cast` is checked at run time.

##### Example

    struct B {   // an interface
        virtual void f();
        virtual void g();
    };

    struct D : B {   // a wider interface
        void f() override;
        virtual void h();
    };

    void user(B* pb)
    {
        if (D* pd = dynamic_cast<D*>(pb)) {
            // ... use D's interface ...
        }
        else {
            // ... make do with B's interface ...
        }
    }

##### Note

Like other casts, `dynamic_cast` is overused.
[Prefer virtual functions to casting](#???).
Prefer [static polymorphism](#???) to hierarchy navigation where it is possible (no run-time resolution necessary)
and reasonably convenient.

##### Note

Some people use `dynamic_cast` where a `typeid` would have been more appropriate;
`dynamic_cast` is a general "is kind of" operation for discovering the best interface to an object,
whereas `typeid` is a "give me the exact type of this object" operation to discover the actual type of an object.
The latter is an inherently simpler operation that ought to be faster.
The latter (`typeid`) is easily hand-crafted if necessary (e.g., if working on a system where RTTI is -- for some reason -- prohibited),
the former (`dynamic_cast`) is far harder to implement correctly in general.

Consider:

    struct B {
        const char * name {"B"};
        virtual const char* id() const { return name; }
        // ...
    };

    struct D : B {
        const char * name {"D"};
        const char* id() const override { return name; }
        // ...
    };

    void use()
    {
        B* pb1 = new B;
        B* pb2 = new D;

        cout << pb1->id(); // "B"
        cout << pb2->id(); // "D"

        if (pb1->id() == pb2->id()) // *pb1 is the same type as *pb2
        if (pb2->id() == "D") {         // looks innocent
            D* pd = static_cast<D*>(pb1);
            // ...
        }
        // ...
    }

The result of `pb2->id() == "D"` is actually implementation defined.
We added it to warn of the dangers of home-brew RTTI.
This code may work as expected for years, just to fail on a new machine, new compiler, or a new linker that does not unify character literals.

If you implement your own RTTI, be careful.

##### Exception

If your implementation provided a really slow `dynamic_cast`, you may have to use a workaround.
However, all workarounds that cannot be statically resolved involve explicit casting (typically `static_cast`) and are error-prone.
You will basically be crafting your own special-purpose `dynamic_cast`.
So, first make sure that your `dynamic_cast` really is as slow as you think it is (there are a fair number of unsupported rumors about)
and that your use of `dynamic_cast` is really performance critical.

We are of the opinion that current implementations of `dynamic_cast` are unnecessarily slow.
For example, under suitable conditions, it is possible to perform a `dynamic_cast` in [fast constant time](http://www.stroustrup.com/fast_dynamic_casting.pdf).
However, compatibility makes changes difficult even if all agree that an effort to optimize is worthwhile.

In very rare cases, if you have measured that the `dynamic_cast` overhead is material, you have other means to statically guarantee that a downcast will succeed (e.g., you are using CRTP carefully), and there is no virtual inheritance involved, consider tactically resorting `static_cast` with a prominent comment and disclaimer summarizing this paragraph and that human attention is needed under maintenance because the type system can't verify correctness. Even so, in our experience such "I know what I'm doing" situations are still a known bug source.

##### Enforcement

Flag all uses of `static_cast` for downcasts, including C-style casts that perform a `static_cast`.

### <a name="Rh-ptr-cast"></a>C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error

##### Reason

Casting to a reference expresses that you intend to end up with a valid object, so the cast must succeed. `dynamic_cast` will then throw if it does not succeed.

##### Example

    ???

##### Enforcement

???

### <a name="Rh-ref-cast"></a>C.148: Use `dynamic_cast` to a pointer type when failure to find the required class is considered a valid alternative

##### Reason

???

##### Example

    ???

##### Enforcement

???

### <a name="Rh-smart"></a>C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`

##### Reason

Avoid resource leaks.

##### Example

    void use(int i)
    {
        auto p = new int {7};           // bad: initialize local pointers with new
        auto q = make_unique<int>(9);   // ok: guarantee the release of the memory allocated for 9
        if (0 < i) return;              // maybe return and leak
        delete p;                       // too late
    }

##### Enforcement

* Flag initialization of a naked pointer with the result of a `new`
* Flag `delete` of local variable

### <a name="Rh-make_unique"></a>C.150: Use `make_unique()` to construct objects owned by `unique_ptr`s

##### Reason

 `make_unique` gives a more concise statement of the construction.
It also ensures exception safety in complex expressions.

##### Example

    unique_ptr<Foo> p {new<Foo>{7}};   // OK: but repetitive

    auto q = make_unique<Foo>(7);      // Better: no repetition of Foo

    // Not exception-safe: the compiler may interleave the computations of arguments as follows:
    //
    // 1. allocate memory for Foo,
    // 2. construct Foo,
    // 3. call bar,
    // 4. construct unique_ptr<Foo>.
    //
    // If bar throws, Foo will not be destroyed, and the memory allocated for it will leak.
    f(unique_ptr<Foo>(new Foo()), bar());

    // Exception-safe: calls to functions are never interleaved.
    f(make_unique<Foo>(), bar());

##### Enforcement

* Flag the repetitive usage of template specialization list `<Foo>`
* Flag variables declared to be `unique_ptr<Foo>`

### <a name="Rh-make_shared"></a>C.151: Use `make_shared()` to construct objects owned by `shared_ptr`s

##### Reason

 `make_shared` gives a more concise statement of the construction.
It also gives an opportunity to eliminate a separate allocation for the reference counts, by placing the `shared_ptr`'s use counts next to its object.

##### Example

    // OK: but repetitive; and separate allocations for the Foo and shared_ptr's use count
    shared_ptr<Foo> p {new<Foo>{7}};

    auto q = make_shared<Foo>(7);   // Better: no repetition of Foo; one object

##### Enforcement

* Flag the repetitive usage of template specialization list`<Foo>`
* Flag variables declared to be `shared_ptr<Foo>`

### <a name="Rh-array"></a>C.152: Never assign a pointer to an array of derived class objects to a pointer to its base

##### Reason

Subscripting the resulting base pointer will lead to invalid object access and probably to memory corruption.

##### Example

    struct B { int x; };
    struct D : B { int y; };

    void use(B*);

    D a[] = {{ "{{" }}1, 2}, {3, 4}, {5, 6}};
    B* p = a;     // bad: a decays to &a[0] which is converted to a B*
    p[1].x = 7;   // overwrite D[0].y

    use(a);       // bad: a decays to &a[0] which is converted to a B*

##### Enforcement

* Flag all combinations of array decay and base to derived conversions.
* Pass an array as a `span` rather than as a pointer, and don't let the array name suffer a derived-to-base conversion before getting into the `span`

## <a name="SS-overload"></a>C.over: Overloading and overloaded operators

You can overload ordinary functions, template functions, and operators.
You cannot overload function objects.

Overload rule summary:

* [C.160: Define operators primarily to mimic conventional usage](#Ro-conventional)
* [C.161: Use nonmember functions for symmetric operators](#Ro-symmetric)
* [C.162: Overload operations that are roughly equivalent](#Ro-equivalent)
* [C.163: Overload only for operations that are roughly equivalent](#Ro-equivalent-2)
* [C.164: Avoid conversion operators](#Ro-conversion)
* [C.165: Use `using` for customization points](#Ro-custom)
* [C.166: Overload unary `&` only as part of a system of smart pointers and references](#Ro-address-of)
* [C.167: Use an operator for an operation with its conventional meaning](#Ro-overload)
* [C.168: Define overloaded operators in the namespace of their operands](#Ro-namespace)
* [C.170: If you feel like overloading a lambda, use a generic lambda](#Ro-lambda)

### <a name="Ro-conventional"></a>C.160: Define operators primarily to mimic conventional usage

##### Reason

Minimize surprises.

##### Example

    class X {
    public:
        // ...
        X& operator=(const X&); // member function defining assignment
        friend bool operator==(const X&, const X&); // == needs access to representation
                                                    // after a = b we have a == b
        // ...
    };

Here, the conventional semantics is maintained: [Copies compare equal](#SS-copy).

##### Example, bad

    X operator+(X a, X b) { return a.v - b.v; }   // bad: makes + subtract

##### Note

Non-member operators should be either friends or defined in [the same namespace as their operands](#Ro-namespace).
[Binary operators should treat their operands equivalently](#Ro-symmetric).

##### Enforcement

Possibly impossible.

### <a name="Ro-symmetric"></a>C.161: Use nonmember functions for symmetric operators

##### Reason

If you use member functions, you need two.
Unless you use a non-member function for (say) `==`, `a == b` and `b == a` will be subtly different.

##### Example

    bool operator==(Point a, Point b) { return a.x == b.x && a.y == b.y; }

##### Enforcement

Flag member operator functions.

### <a name="Ro-equivalent"></a>C.162: Overload operations that are roughly equivalent

##### Reason

Having different names for logically equivalent operations on different argument types is confusing, leads to encoding type information in function names, and inhibits generic programming.

##### Example

Consider:

    void print(int a);
    void print(int a, int base);
    void print(const string&);

These three functions all print their arguments (appropriately). Conversely:

    void print_int(int a);
    void print_based(int a, int base);
    void print_string(const string&);

These three functions all print their arguments (appropriately). Adding to the name just introduced verbosity and inhibits generic code.

##### Enforcement

???

### <a name="Ro-equivalent-2"></a>C.163: Overload only for operations that are roughly equivalent

##### Reason

Having the same name for logically different functions is confusing and leads to errors when using generic programming.

##### Example

Consider:

    void open_gate(Gate& g);   // remove obstacle from garage exit lane
    void fopen(const char* name, const char* mode);   // open file

The two operations are fundamentally different (and unrelated) so it is good that their names differ. Conversely:

    void open(Gate& g);   // remove obstacle from garage exit lane
    void open(const char* name, const char* mode ="r");   // open file

The two operations are still fundamentally different (and unrelated) but the names have been reduced to their (common) minimum, opening opportunities for confusion.
Fortunately, the type system will catch many such mistakes.

##### Note

Be particularly careful about common and popular names, such as `open`, `move`, `+`, and `==`.

##### Enforcement

???

### <a name="Ro-conversion"></a>C.164: Avoid conversion operators

##### Reason

Implicit conversions can be essential (e.g., `double` to `int`) but often cause surprises (e.g., `String` to C-style string).

##### Note

Prefer explicitly named conversions until a serious need is demonstrated.
By "serious need" we mean a reason that is fundamental in the application domain (such as an integer to complex number conversion)
and frequently needed. Do not introduce implicit conversions (through conversion operators or non-`explicit` constructors)
just to gain a minor convenience.

##### Example, bad

    class String {   // handle ownership and access to a sequence of characters
        // ...
        String(czstring p); // copy from *p to *(this->elem)
        // ...
        operator zstring() { return elem; }
        // ...
    };

    void user(zstring p)
    {
        if (*p == "") {
            String s {"Trouble ahead!"};
            // ...
            p = s;
        }
        // use p
    }

The string allocated for `s` and assigned to `p` is destroyed before it can be used.

##### Enforcement

Flag all conversion operators.

### <a name="Ro-custom"></a>C.165: Use `using` for customization points

##### Reason

To find function objects and functions defined in a separate namespace to "customize" a common function.

##### Example

Consider `swap`. It is a general (standard library) function with a definition that will work for just about any type.
However, it is desirable to define specific `swap()`s for specific types.
For example, the general `swap()` will copy the elements of two `vector`s being swapped, whereas a good specific implementation will not copy elements at all.

    namespace N {
        My_type X { /* ... */ };
        void swap(X&, X&);   // optimized swap for N::X
        // ...
    }

    void f1(N::X& a, N::X& b)
    {
        std::swap(a, b);   // probably not what we wanted: calls std::swap()
    }

The `std::swap()` in `f1()` does exactly what we asked it to do: it calls the `swap()` in namespace `std`.
Unfortunately, that's probably not what we wanted.
How do we get `N::X` considered?

    void f2(N::X& a, N::X& b)
    {
        swap(a, b);   // calls N::swap
    }

But that may not be what we wanted for generic code.
There, we typically want the specific function if it exists and the general function if not.
This is done by including the general function in the lookup for the function:

    void f3(N::X& a, N::X& b)
    {
        using std::swap;  // make std::swap available
        swap(a, b);        // calls N::swap if it exists, otherwise std::swap
    }

##### Enforcement

Unlikely, except for known customization points, such as `swap`.
The problem is that the unqualified and qualified lookups both have uses.

### <a name="Ro-address-of"></a>C.166: Overload unary `&` only as part of a system of smart pointers and references

##### Reason

The `&` operator is fundamental in C++.
Many parts of the C++ semantics assumes its default meaning.

##### Example

    class Ptr { // a somewhat smart pointer
        Ptr(X* pp) :p(pp) { /* check */ }
        X* operator->() { /* check */ return p; }
        X operator[](int i);
        X operator*();
    private:
        T* p;
    };

    class X {
        Ptr operator&() { return Ptr{this}; }
        // ...
    };

##### Note

If you "mess with" operator `&` be sure that its definition has matching meanings for `->`, `[]`, `*`, and `.` on the result type.
Note that operator `.` currently cannot be overloaded so a perfect system is impossible.
We hope to remedy that: <http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4477.pdf>.
Note that `std::addressof()` always yields a built-in pointer.

##### Enforcement

Tricky. Warn if `&` is user-defined without also defining `->` for the result type.

### <a name="Ro-namespace"></a>C.168: Define overloaded operators in the namespace of their operands

##### Reason

Readability.
Ability for find operators using ADL.
Avoiding inconsistent definition in different namespaces

##### Example

    struct S { };
    bool operator==(S, S);   // OK: in the same namespace as S, and even next to S
    S s;

    bool x = (s == s);

This is what a default `==` would do, if we had such defaults.

##### Example

    namespace N {
        struct S { };
        bool operator==(S, S);   // OK: in the same namespace as S, and even next to S
    }

    N::S s;

    bool x = (s == s);  // finds N::operator==() by ADL

##### Example, bad

    struct S { };
    S s;

    namespace N {
        S::operator!(S a) { return true; }
        S not_s = !s;
    }

    namespace M {
        S::operator!(S a) { return false; }
        S not_s = !s;
    }

Here, the meaning of `!s` differs in `N` and `M`.
This can be most confusing.
Remove the definition of `namespace M` and the confusion is replaced by an opportunity to make the mistake.

##### Note

If a binary operator is defined for two types that are defined in different namespaces, you cannot follow this rule.
For example:

    Vec::Vector operator*(const Vec::Vector&, const Mat::Matrix&);

This may be something best avoided.

##### See also

This is a special case of the rule that [helper functions should be defined in the same namespace as their class](#Rc-helper).

##### Enforcement

* Flag operator definitions that are not it the namespace of their operands

### <a name="Ro-overload"></a>C.167: Use an operator for an operation with its conventional meaning

##### Reason

Readability. Convention. Reusability. Support for generic code

##### Example

    void cout_my_class(const My_class& c) // confusing, not conventional,not generic
    {
        std::cout << /* class members here */;
    }

    std::ostream& operator<<(std::ostream& os, const my_class& c) // OK
    {
        return os << /* class members here */;
    }

By itself, `cout_my_class` would be OK, but it is not usable/composable with code that rely on the `<<` convention for output:

    My_class var { /* ... */ };
    // ...
    cout << "var = " << var << '\n';

##### Note

There are strong and vigorous conventions for the meaning most operators, such as

* comparisons (`==`, `!=`, `<`, `<=`, `>`, and `>=`),
* arithmetic operations (`+`, `-`, `*`, `/`, and `%`)
* access operations (`.`, `->`, unary `*`, and `[]`)
* assignment (`=`)

Don't define those unconventionally and don't invent your own names for them.

##### Enforcement

Tricky. Requires semantic insight.

### <a name="Ro-lambda"></a>C.170: If you feel like overloading a lambda, use a generic lambda

##### Reason

You cannot overload by defining two different lambdas with the same name.

##### Example

    void f(int);
    void f(double);
    auto f = [](char);   // error: cannot overload variable and function

    auto g = [](int) { /* ... */ };
    auto g = [](double) { /* ... */ };   // error: cannot overload variables

    auto h = [](auto) { /* ... */ };   // OK

##### Enforcement

The compiler catches the attempt to overload a lambda.

## <a name="SS-union"></a>C.union: Unions

A `union` is a `struct` where all members start at the same address so that it can hold only one member at a time.
A `union` does not keep track of which member is stored so the programmer has to get it right;
this is inherently error-prone, but there are ways to compensate.

A type that is a `union` plus an indicator of which member is currently held is called a *tagged union*, a *discriminated union*, or a *variant*.

Union rule summary:

* [C.180: Use `union`s to save Memory](#Ru-union)
* [C.181: Avoid "naked" `union`s](#Ru-naked)
* [C.182: Use anonymous `union`s to implement tagged unions](#Ru-anonymous)
* [C.183: Don't use a `union` for type punning](#Ru-pun)
* ???

### <a name="Ru-union"></a>C.180: Use `union`s to save memory

##### Reason

A `union` allows a single piece of memory to be used for different types of objects at different times.
Consequently, it can be used to save memory when we have several objects that are never used at the same time.

##### Example

    union Value {
        int x;
        double d;
    };

    Value v = { 123 };  // now v holds an int
    cout << v.x << '\n';    // write 123
    v.d = 987.654;  // now v holds a double
    cout << v.d << '\n';    // write 987.654

But heed the warning: [Avoid "naked" `union`s](#Ru-naked)

##### Example

    // Short string optimization
    
    constexpr size_t buffer_size = 16; // Slightly larger than the size of a pointer
    
    class Immutable_string {
    public:
        Immutable_string(const char* str) :
            size(strlen(str))
        {
            if (size < buffer_size)
                strcpy_s(string_buffer, buffer_size, str);
            else {
                string_ptr = new char[size + 1];
                strcpy_s(string_ptr, size + 1, str);
            }
        }
    
        ~Immutable_string()
        {
            if (size >= buffer_size)
                delete string_ptr;
        }
    
        const char* get_str() const
        {
            return (size < buffer_size) ? string_buffer : string_ptr;
        }
    
    private:
        // If the string is short enough, we store the string itself
        // instead of a pointer to the string.
        union {
            char* string_ptr;
            char string_buffer[buffer_size];
        };
    
        const size_t size;
    };

##### Enforcement

???

### <a name="Ru-naked"></a>C.181: Avoid "naked" `union`s

##### Reason

A *naked union* is a union without an associated indicator which member (if any) it holds,
so that the programmer has to keep track.
Naked unions are a source of type errors.

###### Example, bad

    union Value {
        int x;
        double d;
    };

    Value v;
    v.d = 987.654;  // v holds a double

So far, so good, but we can easily misuse the `union`:

    cout << v.x << '\n';    // BAD, undefined behavior: v holds a double, but we read it as an int

Note that the type error happened without any explicit cast.
When we tested that program the last value printed was `1683627180` which it the integer value for the bit pattern for `987.654`.
What we have here is an "invisible" type error that happens to give a result that could easily look innocent.

And, talking about "invisible", this code produced no output:

    v.x = 123;
    cout << v.d << '\n';    // BAD: undefined behavior

###### Alternative

Wrap a `union` in a class together with a type field.

The soon-to-be-standard `variant` type (to be found in `<variant>`) does that for you:

    variant<int, double> v;
    v = 123;        // v holds an int
    int x = get<int>(v);
    v = 123.456;    // v holds a double
    w = get<double>(v);

##### Enforcement

???

### <a name="Ru-anonymous"></a>C.182: Use anonymous `union`s to implement tagged unions

##### Reason

A well-designed tagged union is type safe.
An *anonymous* union simplifies the definition of a class with a (tag, union) pair.

##### Example

This example is mostly borrowed from TC++PL4 pp216-218.
You can look there for an explanation.

The code is somewhat elaborate.
Handling a type with user-defined assignment and destructor is tricky.
Saving programmers from having to write such code is one reason for including `variant` in the standard.

    class Value { // two alternative representations represented as a union
    private:
        enum class Tag { number, text };
        Tag type; // discriminant

        union { // representation (note: anonymous union)
            int i;
            string s; // string has default constructor, copy operations, and destructor
        };
    public:
        struct Bad_entry { }; // used for exceptions

        ~Value();
        Value& operator=(const Value&);   // necessary because of the string variant
        Value(const Value&);
        // ...
        int number() const;
        string text() const;

        void set_number(int n);
        void set_text(const string&);
        // ...
    };

    int Value::number() const
    {
        if (type != Tag::number) throw Bad_entry{};
        return i;
    }

    string Value::text() const
    {
        if (type != Tag::text) throw Bad_entry{};
        return s;
    }

    void Value::set_number(int n)
    {
        if (type == Tag::text) {
            s.~string();      // explicitly destroy string
            type = Tag::number;
        }
        i = n;
    }

    void Value::set_text(const string& ss)
    {
        if (type == Tag::text)
            s = ss;
        else {
            new(&s) string{ss};   // placement new: explicitly construct string
            type = Tag::text;
        }
    }

    Value& Value::operator=(const Value& e)   // necessary because of the string variant
    {
        if (type == Tag::text && e.type == Tag::text) {
            s = e.s;    // usual string assignment
            return *this;
        }

        if (type == Tag::text) s.~string(); // explicit destroy

        switch (e.type) {
        case Tag::number:
            i = e.i;
            break;
        case Tag::text:
            new(&s)(e.s);   // placement new: explicit construct
            type = e.type;
        }

        return *this;
    }

    Value::~Value()
    {
        if (type == Tag::text) s.~string(); // explicit destroy
    }

##### Enforcement

???

### <a name="Ru-pun"></a>C.183: Don't use a `union` for type punning

##### Reason

It is undefined behavior to read a `union` member with a different type from the one with which it was written.
Such punning is invisible, or at least harder to spot than using a named cast.
Type punning using a `union` is a source of errors.

##### Example, bad

    union Pun {
        int x;
        unsigned char c[sizeof(int)];
    };

The idea of `Pun` is to be able to look at the character representation of an `int`.

    void bad(Pun& u)
    {
        u.x = 'x';
        cout << u.c[0] << '\n';     // undefined behavior
    }

If you wanted to see the bytes of an `int`, use a (named) cast:

    void if_you_must_pun(int& x)
    {
        auto p = reinterpret_cast<unsigned char*>(&x);
        cout << p[0] << '\n';     // undefined behavior
        // ...
    }

Accessing the result of an `reinterpret_cast` to a different type from the objects declared type is still undefined behavior,
but at least we can see that something tricky is going on.

##### Note

Unfortunately, `union`s are commonly used for type punning.
We don't consider "sometimes, it works as expected" a strong argument.

##### Enforcement

???



# <a name="S-enum"></a>Enum: Enumerations

Enumerations are used to define sets of integer values and for defining types for such sets of values.
There are two kind of enumerations, "plain" `enum`s and `class enum`s.

Enumeration rule summary:

* [Enum.1: Prefer enumerations over macros](#Renum-macro)
* [Enum.2: Use enumerations to represent sets of related named constants](#Renum-set)
* [Enum.3: Prefer `enum class`es over "plain" `enum`s](#Renum-class)
* [Enum.4: Define operations on enumerations for safe and simple use](#Renum-oper)
* [Enum.5: Don't use `ALL_CAPS` for enumerators](#Renum-caps)
* [Enum.6: Avoid unnamed enumerations](#Renum-unnamed)
* [Enum.7: Specify the underlying type of an enumeration only when necessary](#Renum-underlying)
* [Enum.8: Specify enumerator values only when necessary](#Renum-value)

### <a name="Renum-macro"></a>Enum.1: Prefer enumerations over macros

##### Reason

Macros do not obey scope and type rules. Also, macro names are removed during preprocessing and so usually don't appear in tools like debuggers.

##### Example

First some bad old code:

    // webcolors.h (third party header)
    #define RED   0xFF0000
    #define GREEN 0x00FF00
    #define BLUE  0x0000FF

    // productinfo.h
    // The following define product subtypes based on color
    #define RED    0
    #define PURPLE 1
    #define BLUE   2

    int webby = BLUE;   // webby == 2; probably not what was desired

Instead use an `enum`:

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum class Product_info { red = 0, purple = 1, blue = 2 };

    int webby = blue;   // error: be specific
    Web_color webby = Web_color::blue;

We used an `enum class` to avoid name clashes.

##### Enforcement

Flag macros that define integer values.


### <a name="Renum-set"></a>Enum.2: Use enumerations to represent sets of related named constants

##### Reason

An enumeration shows the enumerators to be related and can be a named type.



##### Example

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };


##### Note

Switching on an enumeration is common and the compiler can warn against unusual patterns of case labels. For example:

    enum class Product_info { red = 0, purple = 1, blue = 2 };

    void print(Product_info inf)
    {
        switch (inf) {
        case Product_info::red: cout << "red"; break;
        case Product_info::purple: cout << "purple"; break;
        }
    }

Such off-by-one switch`statements are often the results of an added enumerator and insufficient testing.

##### Enforcement

* Flag `switch`-statements where the `case`s cover most but not all enumerators of an enumeration.
* Flag `switch`-statements where the `case`s cover a few enumerators of an enumeration, but has no `default`.


### <a name="Renum-class"></a>Enum.3: Prefer class enums over "plain" enums

##### Reason

To minimize surprises: traditional enums convert to int too readily.

##### Example

    void Print_color(int color);

    enum Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum Product_info { Red = 0, Purple = 1, Blue = 2 };

    Web_color webby = Web_color::blue;

    // Clearly at least one of these calls is buggy.
    Print_color(webby);
    Print_color(Product_info::Blue);

Instead use an `enum class`:

    void Print_color(int color);

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum class Product_info { red = 0, purple = 1, blue = 2 };

    Web_color webby = Web_color::blue;
    Print_color(webby);  // Error: cannot convert Web_color to int.
    Print_color(Product_info::Red);  // Error: cannot convert Product_info to int.

##### Enforcement

(Simple) Warn on any non-class `enum` definition.

### <a name="Renum-oper"></a>Enum.4: Define operations on enumerations for safe and simple use

##### Reason

Convenience of use and avoidance of errors.

##### Example

    enum class Day { mon, tue, wed, thu, fri, sat, sun };

    Day operator++(Day& d)
    {
        return d == Day::sun ? Day::mon : Day{++d};
    }

    Day today = Day::sat;
    Day tomorrow = ++today;

##### Enforcement

Flag repeated expressions cast back into an enumeration.


### <a name="Renum-caps"></a>Enum.5: Don't use `ALL_CAPS` for enumerators

##### Reason

Avoid clashes with macros.

##### Example, bad

     // webcolors.h (third party header)
    #define RED   0xFF0000
    #define GREEN 0x00FF00
    #define BLUE  0x0000FF

    // productinfo.h
    // The following define product subtypes based on color

    enum class Product_info { RED, PURPLE, BLUE };   // syntax error

##### Enforcement

Flag ALL_CAPS enumerators.

### <a name="Renum-unnamed"></a>Enum.6: Avoid unnamed enumerations

##### Reason

If you can't name an enumeration, the values are not related

##### Example, bad

    enum { red = 0xFF0000, scale = 4, is_signed = 1 };

Such code is not uncommon in code written before there were convenient alternative ways of specifying integer constants.

##### Alternative

Use `constexpr` values instead. For example:

    constexpr int red = 0xFF0000;
    constexpr short scale = 4;
    constexpr bool is_signed = true;

##### Enforcement

Flag unnamed enumerations.


### <a name="Renum-underlying"></a>Enum.7: Specify the underlying type of an enumeration only when necessary

##### Reason

The default is the easiest to read and write.
`int` is the default integer type.
`int` is compatible with C `enum`s.

##### Example

    enum class Direction : char { n, s, e, w,
                                  ne, nw, se, sw };  // underlying type saves space

    enum class Web_color : int { red   = 0xFF0000,
                                 green = 0x00FF00,
                                 blue  = 0x0000FF };  // underlying type is redundant

##### Note

Specifying the underlying type is necessary in forward declarations of enumerations:

    enum Flags : char;

    void f(Flags);

    // ....

    enum flags : char { /* ... */ };


##### Enforcement

????


### <a name="Renum-value"></a>Enum.8: Specify enumerator values only when necessary

##### Reason

It's the simplest.
It avoids duplicate enumerator values.
The default gives a consecutive set of values that is good for `switch`-statement implementations.

##### Example

    enum class Col1 { red, yellow, blue };
    enum class Col2 { red = 1, yellow = 2, blue = 2 }; // typo
    enum class Month { jan = 1, feb, mar, apr, may, jun,
                       jul, august, sep, oct, nov, dec }; // starting with 1 is conventional
    enum class Base_flag { dec = 1, oct = dec << 1, hex = dec << 2 }; // set of bits

Specifying values is necessary to match conventional values (e.g., `Month`)
and where consecutive values are undesirable (e.g., to get separate bits as in `Base_flag`).

##### Enforcement

* Flag duplicate enumerator values
* Flag explicitly specified all-consecutive enumerator values


# <a name="S-resource"></a>R: Resource management

This section contains rules related to resources.
A resource is anything that must be acquired and (explicitly or implicitly) released, such as memory, file handles, sockets, and locks.
The reason it must be released is typically that it can be in short supply, so even delayed release may do harm.
The fundamental aim is to ensure that we don't leak any resources and that we don't hold a resource longer than we need to.
An entity that is responsible for releasing a resource is called an owner.

There are a few cases where leaks can be acceptable or even optimal:
If you are writing a program that simply produces an output based on an input and the amount of memory needed is proportional to the size of the input, the optimal strategy (for performance and ease of programming) is sometimes simply never to delete anything.
If you have enough memory to handle your largest input, leak away, but be sure to give a good error message if you are wrong.
Here, we ignore such cases.

* Resource management rule summary:

  * [R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)](#Rr-raii)
  * [R.2: In interfaces, use raw pointers to denote individual objects (only)](#Rr-use-ptr)
  * [R.3: A raw pointer (a `T*`) is non-owning](#Rr-ptr)
  * [R.4: A raw reference (a `T&`) is non-owning](#Rr-ref)
  * [R.5: Prefer scoped objects](#Rr-scoped)
  * [R.6: Avoid non-`const` global variables](#Rr-global)

* Allocation and deallocation rule summary:

  * [R.10: Avoid `malloc()` and `free()`](#Rr-mallocfree)
  * [R.11: Avoid calling `new` and `delete` explicitly](#Rr-newdelete)
  * [R.12: Immediately give the result of an explicit resource allocation to a manager object](#Rr-immediate-alloc)
  * [R.13: Perform at most one explicit resource allocation in a single expression statement](#Rr-single-alloc)
  * [R.14: ??? array vs. pointer parameter](#Rr-ap)
  * [R.15: Always overload matched allocation/deallocation pairs](#Rr-pair)

* <a name="Rr-summary-smartptrs"></a>Smart pointer rule summary:

  * [R.20: Use `unique_ptr` or `shared_ptr` to represent ownership](#Rr-owner)
  * [R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership](#Rr-unique)
  * [R.22: Use `make_shared()` to make `shared_ptr`s](#Rr-make_shared)
  * [R.23: Use `make_unique()` to make `unique_ptr`s](#Rr-make_unique)
  * [R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`s](#Rr-weak_ptr)
  * [R.30: Take smart pointers as parameters only to explicitly express lifetime semantics](#Rr-smartptrparam)
  * [R.31: If you have non-`std` smart pointers, follow the basic pattern from `std`](#Rr-smart)
  * [R.32: Take a `unique_ptr<widget>` parameter to express that a function assumes ownership of a `widget`](#Rr-uniqueptrparam)
  * [R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the `widget`](#Rr-reseat)
  * [R.34: Take a `shared_ptr<widget>` parameter to express that a function is part owner](#Rr-sharedptrparam-owner)
  * [R.35: Take a `shared_ptr<widget>&` parameter to express that a function might reseat the shared pointer](#Rr-sharedptrparam)
  * [R.36: Take a `const shared_ptr<widget>&` parameter to express that it might retain a reference count to the object ???](#Rr-sharedptrparam-const)
  * [R.37: Do not pass a pointer or reference obtained from an aliased smart pointer](#Rr-smartptrget)

### <a name="Rr-raii"></a>R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)

##### Reason

To avoid leaks and the complexity of manual resource management.
C++'s language-enforced constructor/destructor symmetry mirrors the symmetry inherent in resource acquire/release function pairs such as `fopen`/`fclose`, `lock`/`unlock`, and `new`/`delete`.
Whenever you deal with a resource that needs paired acquire/release function calls, encapsulate that resource in an object that enforces pairing for you -- acquire the resource in its constructor, and release it in its destructor.

##### Example, bad

Consider:

    void send(X* x, cstring_span destination)
    {
        auto port = open_port(destination);
        my_mutex.lock();
        // ...
        send(port, x);
        // ...
        my_mutex.unlock();
        close_port(port);
        delete x;
    }

In this code, you have to remember to `unlock`, `close_port`, and `delete` on all paths, and do each exactly once.
Further, if any of the code marked `...` throws an exception, then `x` is leaked and `my_mutex` remains locked.

##### Example

Consider:

    void send(unique_ptr<X> x, cstring_span destination)  // x owns the X
    {
        Port port{destination};            // port owns the PortHandle
        lock_guard<mutex> guard{my_mutex}; // guard owns the lock
        // ...
        send(port, x);
        // ...
    } // automatically unlocks my_mutex and deletes the pointer in x

Now all resource cleanup is automatic, performed once on all paths whether or not there is an exception. As a bonus, the function now advertises that it takes over ownership of the pointer.

What is `Port`? A handy wrapper that encapsulates the resource:

    class Port {
        PortHandle port;
    public:
        Port(cstring_span destination) : port{open_port(destination)} { }
        ~Port() { close_port(port); }
        operator PortHandle() { return port; }

        // port handles can't usually be cloned, so disable copying and assignment if necessary
        Port(const Port&) = delete;
        Port& operator=(const Port&) = delete;
    };

##### Note

Where a resource is "ill-behaved" in that it isn't represented as a class with a destructor, wrap it in a class or use [`finally`](#S-gsl)

**See also**: [RAII](#Rr-raii).

### <a name="Rr-use-ptr"></a>R.2: In interfaces, use raw pointers to denote individual objects (only)

##### Reason

Arrays are best represented by a container type (e.g., `vector` (owning)) or a `span` (non-owning).
Such containers and views hold sufficient information to do range checking.

##### Example, bad

    void f(int* p, int n)   // n is the number of elements in p[]
    {
        // ...
        p[2] = 7;   // bad: subscript raw pointer
        // ...
    }

The compiler does not read comments, and without reading other code you do not know whether `p` really points to `n` elements.
Use a `span` instead.

##### Example

    void g(int* p, int fmt)   // print *p using format #fmt
    {
        // ... uses *p and p[0] only ...
    }

##### Exception

C-style strings are passed as single pointers to a zero-terminated sequence of characters.
Use `zstring` rather than `char*` to indicate that you rely on that convention.

##### Note

Many current uses of pointers to a single element could be references.
However, where `nullptr` is a possible value, a reference may not be an reasonable alternative.

##### Enforcement

* Flag pointer arithmetic (including `++`) on a pointer that is not part of a container, view, or iterator.
  This rule would generate a huge number of false positives if applied to an older code base.
* Flag array names passed as simple pointers

### <a name="Rr-ptr"></a>R.3: A raw pointer (a `T*`) is non-owning

##### Reason

There is nothing (in the C++ standard or in most code) to say otherwise and most raw pointers are non-owning.
We want owning pointers identified so that we can reliably and efficiently delete the objects pointed to by owning pointers.

##### Example

    void f()
    {
        int* p1 = new int{7};           // bad: raw owning pointer
        auto p2 = make_unique<int>(7);  // OK: the int is owned by a unique pointer
        // ...
    }

The `unique_ptr` protects against leaks by guaranteeing the deletion of its object (even in the presence of exceptions). The `T*` does not.

##### Example

    template<typename T>
    class X {
        // ...
    public:
        T* p;   // bad: it is unclear whether p is owning or not
        T* q;   // bad: it is unclear whether q is owning or not
    };

We can fix that problem by making ownership explicit:

    template<typename T>
    class X2 {
        // ...
    public:
        owner<T*> p;  // OK: p is owning
        T* q;         // OK: q is not owning
    };

##### Exception

A major class of exception is legacy code, especially code that must remain compilable as C or interface with C and C-style C++ through ABIs.
The fact that there are billions of lines of code that violate this rule against owning `T*`s cannot be ignored.
We'd love to see program transformation tools turning 20-year-old "legacy" code into shiny modern code,
we encourage the development, deployment and use of such tools,
we hope the guidelines will help the development of such tools,
and we even contributed (and contribute) to the research and development in this area.
However, it will take time: "legacy code" is generated faster than we can renovate old code, and so it will be for a few years.

This code cannot all be rewritten (ever assuming good code transformation software), especially not soon.
This problem cannot be solved (at scale) by transforming all owning pointers to `unique_ptr`s and `shared_ptr`s,
partly because we need/use owning "raw pointers" as well as simple pointers in the implementation of our fundamental resource handles.
For example, common `vector` implementations have one owning pointer and two non-owning pointers.
Many ABIs (and essentially all interfaces to C code) use `T*`s, some of them owning.
Some interfaces cannot be simply annotated with `owner` because they need to remain compilable as C
(although this would be a rare good use for a macro, that expands to `owner` in C++ mode only).

##### Note

`owner<T*>` has no default semantics beyond `T*`. It can be used without changing any code using it and without affecting ABIs.
It is simply a indicator to programmers and analysis tools.
For example, if an `owner<T*>` is a member of a class, that class better have a destructor that `delete`s it.

##### Example, bad

Returning a (raw) pointer imposes a life-time management uncertainty on the caller; that is, who deletes the pointed-to object?

    Gadget* make_gadget(int n)
    {
        auto p = new Gadget{n};
        // ...
        return p;
    }

    void caller(int n)
    {
        auto p = make_gadget(n);   // remember to delete p
        // ...
        delete p;
    }

In addition to suffering from the problem from [leak](#???), this adds a spurious allocation and deallocation operation, and is needlessly verbose. If Gadget is cheap to move out of a function (i.e., is small or has an efficient move operation), just return it "by value" (see ["out" return values](#Rf-out)):

    Gadget make_gadget(int n)
    {
        Gadget g{n};
        // ...
        return g;
    }

##### Note

This rule applies to factory functions.

##### Note

If pointer semantics are required (e.g., because the return type needs to refer to a base class of a class hierarchy (an interface)), return a "smart pointer."

##### Enforcement

* (Simple) Warn on `delete` of a raw pointer that is not an `owner<T>`.
* (Moderate) Warn on failure to either `reset` or explicitly `delete` an `owner<T>` pointer on every code path.
* (Simple) Warn if the return value of `new` or a function call with return value of pointer type is assigned to a raw pointer.
* (Simple) Warn if a function returns an object that was allocated within the function but has a move constructor.
  Suggest considering returning it by value instead.

### <a name="Rr-ref"></a>R.4: A raw reference (a `T&`) is non-owning

##### Reason

There is nothing (in the C++ standard or in most code) to say otherwise and most raw references are non-owning.
We want owners identified so that we can reliably and efficiently delete the objects pointed to by owning pointers.

##### Example

    void f()
    {
        int& r = *new int{7};  // bad: raw owning reference
        // ...
        delete &r;             // bad: violated the rule against deleting raw pointers
    }

**See also**: [The raw pointer rule](#Rr-ptr)

##### Enforcement

See [the raw pointer rule](#Rr-ptr)

### <a name="Rr-scoped"></a>R.5: Don't heap-allocate unnecessarily

##### Reason

A scoped object is a local object, a global object, or a member.
This implies that there is no separate allocation and deallocation cost in excess of that already used for the containing scope or object.
The members of a scoped object are themselves scoped and the scoped object's constructor and destructor manage the members' lifetimes.

##### Example

The following example is inefficient (because it has unnecessary allocation and deallocation), vulnerable to exception throws and returns in the `...` part (leading to leaks), and verbose:

    void f(int n)
    {
        auto p = new Gadget{n};
        // ...
        delete p;
    }

Instead, use a local variable:

    void f(int n)
    {
        Gadget g{n};
        // ...
    }

##### Enforcement

* (Moderate) Warn if an object is allocated and then deallocated on all paths within a function. Suggest it should be a local `auto` stack object instead.
* (Simple) Warn if a local `Unique_ptr` or `Shared_ptr` is not moved, copied, reassigned or `reset` before its lifetime ends.

### <a name="Rr-global"></a>R.6: Avoid non-`const` global variables

##### Reason

Global variables can be accessed from everywhere so they can introduce surprising dependencies between apparently unrelated objects.
They are a notable source of errors.

**Warning**: The initialization of global objects is not totally ordered.
If you use a global object initialize it with a constant.
Note that it is possible to get undefined initialization order even for `const` objects.

##### Exception

A global object is often better than a singleton.

##### Exception

An immutable (`const`) global does not introduce the problems we try to avoid by banning global objects.

##### Enforcement

(??? NM: Obviously we can warn about non-`const` statics ... do we want to?)

## <a name="SS-alloc"></a>R.alloc: Allocation and deallocation

### <a name="Rr-mallocfree"></a>R.10: Avoid `malloc()` and `free()`

##### Reason

 `malloc()` and `free()` do not support construction and destruction, and do not mix well with `new` and `delete`.

##### Example

    class Record {
        int id;
        string name;
        // ...
    };

    void use()
    {
        // p1 may be nullptr
        // *p1 is not initialized; in particular,
        // that string isn't a string, but a string-sized bag of bits
        Record* p1 = static_cast<Record*>(malloc(sizeof(Record)));

        auto p2 = new Record;

        // unless an exception is thrown, *p2 is default initialized
        auto p3 = new(nothrow) Record;
        // p3 may be nullptr; if not, *p3 is default initialized

        // ...

        delete p1;    // error: cannot delete object allocated by malloc()
        free(p2);    // error: cannot free() object allocated by new
    }

In some implementations that `delete` and that `free()` might work, or maybe they will cause run-time errors.

##### Exception

There are applications and sections of code where exceptions are not acceptable.
Some of the best such examples are in life-critical hard real-time code.
Beware that many bans on exception use are based on superstition (bad)
or by concerns for older code bases with unsystematic resource management (unfortunately, but sometimes necessary).
In such cases, consider the `nothrow` versions of `new`.

##### Enforcement

Flag explicit use of `malloc` and `free`.

### <a name="Rr-newdelete"></a>R.11: Avoid calling `new` and `delete` explicitly

##### Reason

The pointer returned by `new` should belong to a resource handle (that can call `delete`).
If the pointer returned by `new` is assigned to a plain/naked pointer, the object can be leaked.

##### Note

In a large program, a naked `delete` (that is a `delete` in application code, rather than part of code devoted to resource management)
is a likely bug: if you have N `delete`s, how can you be certain that you don't need N+1 or N-1?
The bug may be latent: it may emerge only during maintenance.
If you have a naked `new`, you probably need a naked `delete` somewhere, so you probably have a bug.

##### Enforcement

(Simple) Warn on any explicit use of `new` and `delete`. Suggest using `make_unique` instead.

### <a name="Rr-immediate-alloc"></a>R.12: Immediately give the result of an explicit resource allocation to a manager object

##### Reason

If you don't, an exception or a return may lead to a leak.

##### Example, bad

    void f(const string& name)
    {
        FILE* f = fopen(name, "r");          // open the file
        vector<char> buf(1024);
        auto _ = finally([f] { fclose(f); })  // remember to close the file
        // ...
    }

The allocation of `buf` may fail and leak the file handle.

##### Example

    void f(const string& name)
    {
        ifstream f{name};   // open the file
        vector<char> buf(1024);
        // ...
    }

The use of the file handle (in `ifstream`) is simple, efficient, and safe.

##### Enforcement

* Flag explicit allocations used to initialize pointers (problem: how many direct resource allocations can we recognize?)

### <a name="Rr-single-alloc"></a>R.13: Perform at most one explicit resource allocation in a single expression statement

##### Reason

If you perform two explicit resource allocations in one statement, you could leak resources because the order of evaluation of many subexpressions, including function arguments, is unspecified.

##### Example

    void fun(shared_ptr<Widget> sp1, shared_ptr<Widget> sp2);

This `fun` can be called like this:

    // BAD: potential leak
    fun(shared_ptr<Widget>(new Widget(a, b)), shared_ptr<Widget>(new Widget(c, d)));

This is exception-unsafe because the compiler may reorder the two expressions building the function's two arguments.
In particular, the compiler can interleave execution of the two expressions:
Memory allocation (by calling `operator new`) could be done first for both objects, followed by attempts to call the two `Widget` constructors.
If one of the constructor calls throws an exception, then the other object's memory will never be released!

This subtle problem has a simple solution: Never perform more than one explicit resource allocation in a single expression statement.
For example:

    shared_ptr<Widget> sp1(new Widget(a, b)); // Better, but messy
    fun(sp1, new Widget(c, d));

The best solution is to avoid explicit allocation entirely use factory functions that return owning objects:

    fun(make_shared<Widget>(a, b), make_shared<Widget>(c, d)); // Best

Write your own factory wrapper if there is not one already.

##### Enforcement

* Flag expressions with multiple explicit resource allocations (problem: how many direct resource allocations can we recognize?)

### <a name="Rr-ap"></a>R.14: ??? array vs. pointer parameter

##### Reason

An array decays to a pointer, thereby losing its size, opening the opportunity for range errors.

##### Example

    ??? what do we recommend: f(int*[]) or f(int**) ???

**Alternative**: Use `span` to preserve size information.

##### Enforcement

Flag `[]` parameters.

### <a name="Rr-pair"></a>R.15: Always overload matched allocation/deallocation pairs

##### Reason

Otherwise you get mismatched operations and chaos.

##### Example

    class X {
        // ...
        void* operator new(size_t s);
        void operator delete(void*);
        // ...
    };

##### Note

If you want memory that cannot be deallocated, `=delete` the deallocation operation.
Don't leave it undeclared.

##### Enforcement

Flag incomplete pairs.

## <a name="SS-smart"></a>R.smart: Smart pointers

### <a name="Rr-owner"></a>R.20: Use `unique_ptr` or `shared_ptr` to represent ownership

##### Reason

They can prevent resource leaks.

##### Example

Consider:

    void f()
    {
        X x;
        X* p1 { new X };              // see also ???
        unique_ptr<T> p2 { new X };   // unique ownership; see also ???
        shared_ptr<T> p3 { new X };   // shared ownership; see also ???
    }

This will leak the object used to initialize `p1` (only).

##### Enforcement

(Simple) Warn if the return value of `new` or a function call with return value of pointer type is assigned to a raw pointer.

### <a name="Rr-unique"></a>R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership

##### Reason

A `unique_ptr` is conceptually simpler and more predictable (you know when destruction happens) and faster (you don't implicitly maintain a use count).

##### Example, bad

This needlessly adds and maintains a reference count.

    void f()
    {
        shared_ptr<Base> base = make_shared<Derived>();
        // use base locally, without copying it -- refcount never exceeds 1
    } // destroy base

##### Example

This is more efficient:

    void f()
    {
        unique_ptr<Base> base = make_unique<Derived>();
        // use base locally
    } // destroy base

##### Enforcement

(Simple) Warn if a function uses a `Shared_ptr` with an object allocated within the function, but never returns the `Shared_ptr` or passes it to a function requiring a `Shared_ptr&`. Suggest using `unique_ptr` instead.

### <a name="Rr-make_shared"></a>R.22: Use `make_shared()` to make `shared_ptr`s

##### Reason

If you first make an object and then give it to a `shared_ptr` constructor, you (most likely) do one more allocation (and later deallocation) than if you use `make_shared()` because the reference counts must be allocated separately from the object.

##### Example

Consider:

    shared_ptr<X> p1 { new X{2} }; // bad
    auto p = make_shared<X>(2);    // good

The `make_shared()` version mentions `X` only once, so it is usually shorter (as well as faster) than the version with the explicit `new`.

##### Enforcement

(Simple) Warn if a `shared_ptr` is constructed from the result of `new` rather than `make_shared`.

### <a name="Rr-make_unique"></a>R.23: Use `make_unique()` to make `unique_ptr`s

##### Reason

For convenience and consistency with `shared_ptr`.

##### Note

`make_unique()` is C++14, but widely available (as well as simple to write).

##### Enforcement

(Simple) Warn if a `unique_ptr` is constructed from the result of `new` rather than `make_unique`.

### <a name="Rr-weak_ptr"></a>R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`s

##### Reason

 `shared_ptr`'s rely on use counting and the use count for a cyclic structure never goes to zero, so we need a mechanism to
be able to destroy a cyclic structure.

##### Example

    ???

##### Note

 ??? (HS: A lot of people say "to break cycles", while I think "temporary shared ownership" is more to the point.)
???(BS: breaking cycles is what you must do; temporarily sharing ownership is how you do it.
You could "temporarily share ownership" simply by using another `shared_ptr`.)

##### Enforcement

??? probably impossible. If we could statically detect cycles, we wouldn't need `weak_ptr`

### <a name="Rr-smartptrparam"></a>R.30: Take smart pointers as parameters only to explicitly express lifetime semantics

##### Reason

Accepting a smart pointer to a `widget` is wrong if the function just needs the `widget` itself.
It should be able to accept any `widget` object, not just ones whose lifetimes are managed by a particular kind of smart pointer.
A function that does not manipulate lifetime should take raw pointers or references instead.

##### Example, bad

    // callee
    void f(shared_ptr<widget>& w)
    {
        // ...
        use(*w); // only use of w -- the lifetime is not used at all
        // ...
    };

    // caller
    shared_ptr<widget> my_widget = /* ... */;
    f(my_widget);

    widget stack_widget;
    f(stack_widget); // error

##### Example, good

    // callee
    void f(widget& w)
    {
        // ...
        use(w);
        // ...
    };

    // caller
    shared_ptr<widget> my_widget = /* ... */;
    f(*my_widget);

    widget stack_widget;
    f(stack_widget); // ok -- now this works

##### Enforcement

* (Simple) Warn if a function takes a parameter of a smart pointer type (that overloads `operator->` or `operator*`) `unique_ptr` or `shared_ptr` and the function only calls any of: `operator*`, `operator->` or `get()`.
* Flag a parameter of a smart pointer type (a type that overloads `operator->` or `operator*`) that is copyable but never copied/moved from in the function body or else movable but never moved from in the function body or by being a by-value parameter, and that is never modified, and that is not passed along to another function that could do so. That means the ownership semantics are not used.
  Suggest using a `T*` or `T&` instead.

### <a name="Rr-smart"></a>R.31: If you have non-`std` smart pointers, follow the basic pattern from `std`

##### Reason

The rules in the following section also work for other kinds of third-party and custom smart pointers and are very useful for diagnosing common smart pointer errors that cause performance and correctness problems.
You want the rules to work on all the smart pointers you use.

Any type (including primary template or specialization) that overloads unary `*` and `->` is considered a smart pointer:

* If it is copyable, it is recognized as a reference-counted `shared_ptr`.
* If it is not copyable, it is recognized as a unique `unique_ptr`.

##### Example

    // use Boost's intrusive_ptr
    #include<boost/intrusive_ptr.hpp>
    void f(boost::intrusive_ptr<widget> p)  // error under rule 'sharedptrparam'
    {
        p->foo();
    }

    // use Microsoft's CComPtr
    #include<atlbase.h>
    void f(CComPtr<widget> p)               // error under rule 'sharedptrparam'
    {
        p->foo();
    }

Both cases are an error under the [`sharedptrparam` guideline](#Rr-smartptrparam):
`p` is a `Shared_ptr`, but nothing about its sharedness is used here and passing it by value is a silent pessimization;
these functions should accept a smart pointer only if they need to participate in the widget's lifetime management. Otherwise they should accept a `widget*`, if it can be `nullptr`. Otherwise, and ideally, the function should accept a `widget&`.
These smart pointers match the `Shared_ptr` concept, so these guideline enforcement rules work on them out of the box and expose this common pessimization.

### <a name="Rr-uniqueptrparam"></a>R.32: Take a `unique_ptr<widget>` parameter to express that a function assumes ownership of a `widget`

##### Reason

Using `unique_ptr` in this way both documents and enforces the function call's ownership transfer.

##### Example

    void sink(unique_ptr<widget>); // consumes the widget

    void sink(widget*);            // just uses the widget

##### Example, bad

    void thinko(const unique_ptr<widget>&); // usually not what you want

##### Enforcement

* (Simple) Warn if a function takes a `Unique_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by reference to `const`. Suggest taking a `const T*` or `const T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by rvalue reference. Suggest using pass by value instead.

### <a name="Rr-reseat"></a>R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the`widget`

##### Reason

Using `unique_ptr` in this way both documents and enforces the function call's reseating semantics.

##### Note

"reseat" means "making a reference or a smart pointer refer to a different object."

##### Example

    void reseat(unique_ptr<widget>&); // "will" or "might" reseat pointer

##### Example, bad

    void thinko(const unique_ptr<widget>&); // usually not what you want

##### Enforcement

* (Simple) Warn if a function takes a `Unique_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by reference to `const`. Suggest taking a `const T*` or `const T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by rvalue reference. Suggest using pass by value instead.

### <a name="Rr-sharedptrparam-owner"></a>R.34: Take a `shared_ptr<widget>` parameter to express that a function is part owner

##### Reason

This makes the function's ownership sharing explicit.

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-sharedptrparam"></a>R.35: Take a `shared_ptr<widget>&` parameter to express that a function might reseat the shared pointer

##### Reason

This makes the function's reseating explicit.

##### Note

"reseat" means "making a reference or a smart pointer refer to a different object."

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-sharedptrparam-const"></a>R.36: Take a `const shared_ptr<widget>&` parameter to express that it might retain a reference count to the object ???

##### Reason

This makes the function's ??? explicit.

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-smartptrget"></a>R.37: Do not pass a pointer or reference obtained from an aliased smart pointer

##### Reason

Violating this rule is the number one cause of losing reference counts and finding yourself with a dangling pointer.
Functions should prefer to pass raw pointers and references down call chains.
At the top of the call tree where you obtain the raw pointer or reference from a smart pointer that keeps the object alive.
You need to be sure that the smart pointer cannot inadvertently be reset or reassigned from within the call tree below.

##### Note

To do this, sometimes you need to take a local copy of a smart pointer, which firmly keeps the object alive for the duration of the function and the call tree.

##### Example

Consider this code:

    // global (static or heap), or aliased local ...
    shared_ptr<widget> g_p = ...;

    void f(widget& w)
    {
        g();
        use(w);  // A
    }

    void g()
    {
        g_p = ...; // oops, if this was the last shared_ptr to that widget, destroys the widget
    }

The following should not pass code review:

    void my_code()
    {
        // BAD: passing pointer or reference obtained from a nonlocal smart pointer
        //      that could be inadvertently reset somewhere inside f or it callees
        f(*g_p);

        // BAD: same reason, just passing it as a "this" pointer
         g_p->func();
    }

The fix is simple -- take a local copy of the pointer to "keep a ref count" for your call tree:

    void my_code()
    {
        // cheap: 1 increment covers this entire function and all the call trees below us
        auto pin = g_p;

        // GOOD: passing pointer or reference obtained from a local unaliased smart pointer
        f(*pin);

        // GOOD: same reason
        pin->func();
    }

##### Enforcement

* (Simple) Warn if a pointer or reference obtained from a smart pointer variable (`Unique_ptr` or `Shared_ptr`) that is nonlocal, or that is local but potentially aliased, is used in a function call. If the smart pointer is a `Shared_ptr` then suggest taking a local copy of the smart pointer and obtain a pointer or reference from that instead.

# <a name="S-expr"></a>ES: Expressions and Statements

Expressions and statements are the lowest and most direct way of expressing actions and computation. Declarations in local scopes are statements.

For naming, commenting, and indentation rules, see [NL: Naming and layout](#S-naming).

General rules:

* [ES.1: Prefer the standard library to other libraries and to "handcrafted code"](#Res-lib)
* [ES.2: Prefer suitable abstractions to direct use of language features](#Res-abstr)

Declaration rules:

* [ES.5: Keep scopes small](#Res-scope)
* [ES.6: Declare names in for-statement initializers and conditions to limit scope](#Res-cond)
* [ES.7: Keep common and local names short, and keep uncommon and nonlocal names longer](#Res-name-length)
* [ES.8: Avoid similar-looking names](#Res-name-similar)
* [ES.9: Avoid `ALL_CAPS` names](#Res-not-CAPS)
* [ES.10: Declare one name (only) per declaration](#Res-name-one)
* [ES.11: Use `auto` to avoid redundant repetition of type names](#Res-auto)
* [ES.12: Do not reuse names in nested scopes](#Res-reuse)
* [ES.20: Always initialize an object](#Res-always)
* [ES.21: Don't introduce a variable (or constant) before you need to use it](#Res-introduce)
* [ES.22: Don't declare a variable until you have a value to initialize it with](#Res-init)
* [ES.23: Prefer the `{}`-initializer syntax](#Res-list)
* [ES.24: Use a `unique_ptr<T>` to hold pointers in code that may throw](#Res-unique)
* [ES.25: Declare an object `const` or `constexpr` unless you want to modify its value later on](#Res-const)
* [ES.26: Don't use a variable for two unrelated purposes](#Res-recycle)
* [ES.27: Use `std::array` or `stack_array` for arrays on the stack](#Res-stack)
* [ES.28: Use lambdas for complex initialization, especially of `const` variables](#Res-lambda-init)
* [ES.30: Don't use macros for program text manipulation](#Res-macros)
* [ES.31: Don't use macros for constants or "functions"](#Res-macros2)
* [ES.32: Use `ALL_CAPS` for all macro names](#Res-ALL_CAPS)
* [ES.33: If you must use macros, give them unique names](#Res-MACROS)
* [ES.34: Don't define a (C-style) variadic function](#Res-ellipses)

Expression rules:

* [ES.40: Avoid complicated expressions](#Res-complicated)
* [ES.41: If in doubt about operator precedence, parenthesize](#Res-parens)
* [ES.42: Keep use of pointers simple and straightforward](#Res-ptr)
* [ES.43: Avoid expressions with undefined order of evaluation](#Res-order)
* [ES.44: Don't depend on order of evaluation of function arguments](#Res-order-fct)
* [ES.45: Avoid narrowing conversions](#Res-narrowing)
* [ES.46: Avoid "magic constants"; use symbolic constants](#Res-magic)
* [ES.47: Use `nullptr` rather than `0` or `NULL`](#Res-nullptr)
* [ES.48: Avoid casts](#Res-casts)
* [ES.49: If you must use a cast, use a named cast](#Res-casts-named)
* [ES.50: Don't cast away `const`](#Res-casts-const)
* [ES.55: Avoid the need for range checking](#Res-range-checking)
* [ES.56: Avoid `std::move()` in application code](#Res-move)
* [ES.60: Avoid `new` and `delete` outside resource management functions](#Res-new)
* [ES.61: delete arrays using `delete[]` and non-arrays using `delete`](#Res-del)
* [ES.62: Don't compare pointers into different arrays](#Res-arr2)
* [ES.63: Don't slice](#Res-slice)

Statement rules:

* [ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice](#Res-switch-if)
* [ES.71: Prefer a range-`for`-statement to a `for`-statement when there is a choice](#Res-for-range)
* [ES.72: Prefer a `for`-statement to a `while`-statement when there is an obvious loop variable](#Res-for-while)
* [ES.73: Prefer a `while`-statement to a `for`-statement when there is no obvious loop variable](#Res-while-for)
* [ES.74: Prefer to declare a loop variable in the initializer part of as `for`-statement](#Res-for-init)
* [ES.75: Avoid `do`-statements](#Res-do)
* [ES.76: Avoid `goto`](#Res-goto)
* [ES.77: ??? `continue`](#Res-continue)
* [ES.78: Always end a non-empty `case` with a `break`](#Res-break)
* [ES.79: ??? `default`](#Res-default)
* [ES.85: Make empty statements visible](#Res-empty)
* [ES.86: Avoid modifying loop control variables inside the body of raw for-loops](#Res-loop-counter)

Arithmetic rules:

* [ES.100: Don't mix signed and unsigned arithmetic](#Res-mix)
* [ES.101: Use unsigned types for bit manipulation](#Res-unsigned)
* [ES.102: Use signed types for arithmetic](#Res-signed)
* [ES.103: Don't overflow](#Res-overflow)
* [ES.104: Don't underflow](#Res-underflow)
* [ES.105: Don't divide by zero](#Res-zero)

### <a name="Res-lib"></a>ES.1: Prefer the standard library to other libraries and to "handcrafted code"

##### Reason

Code using a library can be much easier to write than code working directly with language features, much shorter, tend to be of a higher level of abstraction, and the library code is presumably already tested.
The ISO C++ standard library is among the most widely known and best tested libraries.
It is available as part of all C++ Implementations.

##### Example

    auto sum = accumulate(begin(a), end(a), 0.0);   // good

a range version of `accumulate` would be even better:

    auto sum = accumulate(v, 0.0); // better

but don't hand-code a well-known algorithm:

    int max = v.size();   // bad: verbose, purpose unstated
    double sum = 0.0;
    for (int i = 0; i < max; ++i)
        sum = sum + v[i];

##### Exception

Large parts of the standard library rely on dynamic allocation (free store). These parts, notably the containers but not the algorithms, are unsuitable for some hard-real time and embedded applications. In such cases, consider providing/using similar facilities, e.g.,  a standard-library-style container implemented using a pool allocator.

##### Enforcement

Not easy. ??? Look for messy loops, nested loops, long functions, absence of function calls, lack of use of non-built-in types. Cyclomatic complexity?

### <a name="Res-abstr"></a>ES.2: Prefer suitable abstractions to direct use of language features

##### Reason

A "suitable abstraction" (e.g., library or class) is closer to the application concepts than the bare language, leads to shorter and clearer code, and is likely to be better tested.

##### Example

    vector<string> read1(istream& is)   // good
    {
        vector<string> res;
        for (string s; is >> s;)
            res.push_back(s);
        return res;
    }

The more traditional and lower-level near-equivalent is longer, messier, harder to get right, and most likely slower:

    char** read2(istream& is, int maxelem, int maxstring, int* nread)   // bad: verbose and incomplete
    {
        auto res = new char*[maxelem];
        int elemcount = 0;
        while (is && elemcount < maxelem) {
            auto s = new char[maxstring];
            is.read(s, maxstring);
            res[elemcount++] = s;
        }
        nread = &elemcount;
        return res;
    }

Once the checking for overflow and error handling has been added that code gets quite messy, and there is the problem remembering to `delete` the returned pointer and the C-style strings that array contains.

##### Enforcement

Not easy. ??? Look for messy loops, nested loops, long functions, absence of function calls, lack of use of non-built-in types. Cyclomatic complexity?

## ES.dcl: Declarations

A declaration is a statement. a declaration introduces a name into a scope and may cause the construction of a named object.

### <a name="Res-scope"></a>ES.5: Keep scopes small

##### Reason

Readability. Minimize resource retention. Avoid accidental misuse of value.

**Alternative formulation**: Don't declare a name in an unnecessarily large scope.

##### Example

    void use()
    {
        int i;    // bad: i is needlessly accessible after loop
        for (i = 0; i < 20; ++i) { /* ... */ }
        // no intended use of i here
        for (int i = 0; i < 20; ++i) { /* ... */ }  // good: i is local to for-loop

        if (auto pc = dynamic_cast<Circle*>(ps)) {  // good: pc is local to if-statement
            // ... deal with Circle ...
        }
        else {
            // ... handle error ...
        }
    }

##### Example, bad

    void use(const string& name)
    {
        string fn = name + ".txt";
        ifstream is {fn};
        Record r;
        is >> r;
        // ... 200 lines of code without intended use of fn or is ...
    }

This function is by most measure too long anyway, but the point is that the resources used by `fn` and the file handle held by `is`
are retained for much longer than needed and that unanticipated use of `is` and `fn` could happen later in the function.
In this case, it might be a good idea to factor out the read:

    Record load_record(const string& name)
    {
        string fn = name + ".txt";
        ifstream is {fn};
        Record r;
        is >> r;
        return r;
    }

    void use(const string& name)
    {
        Record r = load_record(name);
        // ... 200 lines of code ...
    }

##### Enforcement

* Flag loop variable declared outside a loop and not used after the loop
* Flag when expensive resources, such as file handles and locks are not used for N-lines (for some suitable N)

### <a name="Res-cond"></a>ES.6: Declare names in for-statement initializers and conditions to limit scope

##### Reason

Readability. Minimize resource retention.

##### Example

    void use()
    {
        for (string s; cin >> s;)
            v.push_back(s);

        for (int i = 0; i < 20; ++i) {   // good: i is local to for-loop
            // ...
        }

        if (auto pc = dynamic_cast<Circle*>(ps)) {   // good: pc is local to if-statement
            // ... deal with Circle ...
        }
        else {
            // ... handle error ...
        }
    }

##### Enforcement

* Flag loop variables declared before the loop and not used after the loop
* (hard) Flag loop variables declared before the loop and used after the loop for an unrelated purpose.

### <a name="Res-name-length"></a>ES.7: Keep common and local names short, and keep uncommon and nonlocal names longer

##### Reason

Readability. Lowering the chance of clashes between unrelated non-local names.

##### Example

Conventional short, local names increase readability:

    template<typename T>    // good
    void print(ostream& os, const vector<T>& v)
    {
        for (int i = 0; i < v.size(); ++i)
            os << v[i] << '\n';
    }

An index is conventionally called `i` and there is no hint about the meaning of the vector in this generic function, so `v` is as good name as any. Compare

    template<typename Element_type>   // bad: verbose, hard to read
    void print(ostream& target_stream, const vector<Element_type>& current_vector)
    {
        for (int current_element_index = 0;
                current_element_index < current_vector.size();
                ++current_element_index
        )
        target_stream << current_vector[current_element_index] << '\n';
    }

Yes, it is a caricature, but we have seen worse.

##### Example

Unconventional and short non-local names obscure code:

    void use1(const string& s)
    {
        // ...
        tt(s);   // bad: what is tt()?
        // ...
    }

Better, give non-local entities readable names:

    void use1(const string& s)
    {
        // ...
        trim_tail(s);   // better
        // ...
    }

Here, there is a chance that the reader knows what `trim_tail` means and that the reader can remember it after looking it up.

##### Example, bad

Argument names of large functions are de facto non-local and should be meaningful:

    void complicated_algorithm(vector<Record>& vr, const vector<int>& vi, map<string, int>& out)
    // read from events in vr (marking used Records) for the indices in
    // vi placing (name, index) pairs into out
    {
        // ... 500 lines of code using vr, vi, and out ...
    }

We recommend keeping functions short, but that rule isn't universally adhered to and naming should reflect that.

##### Enforcement

Check length of local and non-local names. Also take function length into account.

### <a name="Res-name-similar"></a>ES.8: Avoid similar-looking names

##### Reason

Code clarity and readability. Too-similar names slow down comprehension and increase the likelihood of error.

##### Example; bad

    if (readable(i1 + l1 + ol + o1 + o0 + ol + o1 + I0 + l0)) surprise();

##### Example; bad

Do not declare a non-type with the same name as a type in the same scope. This removes the need to disambiguate with a keyword such as `struct` or `enum`. It also removes a source of errors, as `struct X` can implicitly declare `X` if lookup fails.

    struct foo { int n; };
    struct foo foo();       // BAD, foo is a type already in scope
    struct foo x = foo();   // requires disambiguation

##### Exception

Antique header files might declare non-types and types with the same name in the same scope.

##### Enforcement

* Check names against a list of known confusing letter and digit combinations.
* Flag a declaration of a variable, function, or enumerator that hides a class or enumeration declared in the same scope.

### <a name="Res-not-CAPS"></a>ES.9: Avoid `ALL_CAPS` names

##### Reason

Such names are commonly used for macros. Thus, `ALL_CAPS` name are vulnerable to unintended macro substitution.

##### Example

    // somewhere in some header:
    #define NE !=

    // somewhere else in some other header:
    enum Coord { N, NE, NW, S, SE, SW, E, W };

    // somewhere third in some poor programmer's .cpp:
    switch (direction) {
    case N:
        // ...
    case NE:
        // ...
    // ...
    }

##### Note

Do not use `ALL_CAPS` for constants just because constants used to be macros.

##### Enforcement

Flag all uses of ALL CAPS. For older code, accept ALL CAPS for macro names and flag all non-ALL-CAPS macro names.

### <a name="Res-name-one"></a>ES.10: Declare one name (only) per declaration

##### Reason

One-declaration-per line increases readability and avoids mistakes related to
the C/C++ grammar. It also leaves room for a more descriptive end-of-line
comment.

##### Example, bad

    char *p, c, a[7], *pp[7], **aa[10];   // yuck!

##### Exception

A function declaration can contain several function argument declarations.

##### Example

    template <class InputIterator, class Predicate>
    bool any_of(InputIterator first, InputIterator last, Predicate pred);

or better using concepts:

    bool any_of(InputIterator first, InputIterator last, Predicate pred);

##### Example

    double scalbn(double x, int n);   // OK: x * pow(FLT_RADIX, n); FLT_RADIX is usually 2

or:

    double scalbn(    // better: x * pow(FLT_RADIX, n); FLT_RADIX is usually 2
        double x,     // base value
        int n         // exponent
    );

or:

    // better: base * pow(FLT_RADIX, exponent); FLT_RADIX is usually 2
    double scalbn(double base, int exponent);

##### Enforcement

Flag non-function arguments with multiple declarators involving declarator operators (e.g., `int* p, q;`)

### <a name="Res-auto"></a>ES.11: Use `auto` to avoid redundant repetition of type names

##### Reason

* Simple repetition is tedious and error prone.
* When you use `auto`, the name of the declared entity is in a fixed position in the declaration, increasing readability.
* In a template function declaration the return type can be a member type.

##### Example

Consider:

    auto p = v.begin();   // vector<int>::iterator
    auto s = v.size();
    auto h = t.future();
    auto q = make_unique<int[]>(s);
    auto f = [](int x){ return x + 10; };

In each case, we save writing a longish, hard-to-remember type that the compiler already knows but a programmer could get wrong.

##### Example

    template<class T>
    auto Container<T>::first() -> Iterator;   // Container<T>::Iterator

##### Exception

Avoid `auto` for initializer lists and in cases where you know exactly which type you want and where an initializer might require conversion.

##### Example

    auto lst = { 1, 2, 3 };   // lst is an initializer list
    auto x{1};   // x is an int (after correction of the C++14 standard; initializer_list in C++11)

##### Note

When concepts become available, we can (and should) be more specific about the type we are deducing:

    // ...
    ForwardIterator p = algo(x, y, z);

##### Enforcement

Flag redundant repetition of type names in a declaration.

### <a name="Res-reuse"></a>ES.12: Do not reuse names in nested scopes

##### Reason

It is easy to get confused about which variable is used.
Can cause maintenance problems.

##### Example, bad

    int d = 0;
    // ...
    if (cond) {
        // ...
        d = 9;
        // ...
    }
    else {
        // ...
        int d = 7;
        // ...
        d = value_to_be_returned;
        // ...
    }

    return d;

If this is a large `if`-statement, it is easy to overlook that a new `d` has been introduced in the inner scope.
This is a known source of bugs.
Sometimes such reuse of a name in an inner scope is called "shadowing".

##### Note

Shadowing is primarily a problem when functions are too large and too complex.

##### Example

Shadowing of function arguments in the outermost block is disallowed by the language:

    void f(int x)
    {
        int x = 4;  // error: reuse of function argument name

        if (x) {
            int x = 7;  // allowed, but bad
            // ...
        }
    }

##### Example, bad

Reuse of a member name as a local variable can also be a problem:

    struct S {
        int m;
        void f(int x);
    };

    void S::f(int x)
    {
        m = 7;    // assign to member
        if (x) {
            int m = 9;
            // ...
            m = 99; // assign to member
            // ...
        }
    }

##### Exception

We often reuse function names from a base class in a derived class:

    struct B {
        void f(int);
    };

    struct D : B {
        void f(double);
        using B::f;
    };

This is error-prone.
For example, had we forgotten the using declaration, a call `d.f(1)` would not have found the `int` version of `f`.

??? Do we need a specific rule about shadowing/hiding in class hierarchies?

##### Enforcement

* Flag reuse of a name in nested local scopes
* Flag reuse of a member name as a local variable in a member function
* Flag reuse of a global name as a local variable or a member name
* Flag reuse of a base class member name in a derived class (except for function names)

### <a name="Res-always"></a>ES.20: Always initialize an object

##### Reason

Avoid used-before-set errors and their associated undefined behavior.
Avoid problems with comprehension of complex initialization.
Simplify refactoring.

##### Example

    void use(int arg)
    {
        int i;   // bad: uninitialized variable
        // ...
        i = 7;   // initialize i
    }

No, `i = 7` does not initialize `i`; it assigns to it. Also, `i` can be read in the `...` part. Better:

    void use(int arg)   // OK
    {
        int i = 7;   // OK: initialized
        string s;    // OK: default initialized
        // ...
    }

##### Note

The *always initialize* rule is deliberately stronger than the *an object must be set before used* language rule.
The latter, more relaxed rule, catches the technical bugs, but:

* It leads to less readable code
* It encourages people to declare names in greater than necessary scopes
* It leads to harder to read code
* It leads to logic bugs by encouraging complex code
* It hampers refactoring

The *always initialize* rule is a style rule aimed to improve maintainability as well as a rule protecting against used-before-set errors.

##### Example

Here is an example that is often considered to demonstrate the need for a more relaxed rule for initialization

    widget i;    // "widget" a type that's expensive to initialize, possibly a large POD
    widget j;

    if (cond) {  // bad: i and j are initialized "late"
        i = f1();
        j = f2();
    }
    else {
        i = f3();
        j = f4();
    }

This cannot trivially be rewritten to initialize `i` and `j` with initializers.
Note that for types with a default constructor, attempting to postpone initialization simply leads to a default initialization followed by an assignment.
A popular reason for such examples is "efficiency", but a compiler that can detect whether we made a used-before-set error can also eliminate any redundant double initialization.

At the cost of repeating `cond` we could write:

    widget i = (cond) ? f1() : f3();
    widget j = (cond) ? f2() : f4();

Assuming that there is a logical connection between `i` and `j`, that connection should probably be expressed in code:

    pair<widget, widget> make_related_widgets(bool x)
    {
        return (x) ? {f1(), f2()} : {f3(), f4() };
    }

    auto init = make_related_widgets(cond);
    widget i = init.first;
    widget j = init.second;

Obviously, what we really would like is a construct that initialized n variables from a `tuple`. For example:

    auto {i, j} = make_related_widgets(cond);    // Not C++14

Today, we might approximate that using `tie()`:

    widget i;       // bad: uninitialized variable
    widget j;
    tie(i, j) = make_related_widgets(cond);

This may be seen as an example of the *immediately initialize from input* exception below.

Creating optimal and equivalent code from all of these examples should be well within the capabilities of modern C++ compilers
(but don't make performance claims without measuring; a compiler may very well not generate optimal code for every example and
there may be language rules preventing some optimization that you would have liked in a particular case).

##### Note

Complex initialization has been popular with clever programmers for decades.
It has also been a major source of errors and complexity.
Many such errors are introduced during maintenance years after the initial implementation.

##### Exception

It you are declaring an object that is just about to be initialized from input, initializing it would cause a double initialization.
However, beware that this may leave uninitialized data beyond the input -- and that has been a fertile source of errors and security breaches:

    constexpr int max = 8 * 1024;
    int buf[max];         // OK, but suspicious: uninitialized
    f.read(buf, max);

The cost of initializing that array could be significant in some situations.
However, such examples do tend to leave uninitialized variables accessible, so they should be treated with suspicion.

    constexpr int max = 8 * 1024;
    int buf[max] = {};   // zero all elements; better in some situations
    f.read(buf, max);

When feasible use a library function that is known not to overflow. For example:

    string s;   // s is default initialized to ""
    cin >> s;   // s expands to hold the string

Don't consider simple variables that are targets for input operations exceptions to this rule:

    int i;   // bad
    // ...
    cin >> i;

In the not uncommon case where the input target and the input operation get separated (as they should not) the possibility of used-before-set opens up.

    int i2 = 0;   // better
    // ...
    cin >> i;

A good optimizer should know about input operations and eliminate the redundant operation.

##### Example

Using an `uninitialized` or sentinel value is a symptom of a problem and not a
solution:

    widget i = uninit;  // bad
    widget j = uninit;

    // ...
    use(i);         // possibly used before set
    // ...

    if (cond) {     // bad: i and j are initialized "late"
        i = f1();
        j = f2();
    }
    else {
        i = f3();
        j = f4();
    }

Now the compiler cannot even simply detect a used-before-set. Further, we've introduced complexity in the state space for widget: which operations are valid on an `uninit` widget and which are not?

##### Note

Sometimes, a lambda can be used as an initializer to avoid an uninitialized variable:

    error_code ec;
    Value v = [&] {
        auto p = get_value();   // get_value() returns a pair<error_code, Value>
        ec = p.first;
        return p.second;
    }();

or maybe:

    Value v = [] {
        auto p = get_value();   // get_value() returns a pair<error_code, Value>
        if (p.first) throw Bad_value{p.first};
        return p.second;
    }();

**See also**: [ES.28](#Res-lambda-init)

##### Enforcement

* Flag every uninitialized variable.
  Don't flag variables of user-defined types with default constructors.
* Check that an uninitialized buffer is written into *immediately* after declaration.
  Passing an uninitialized variable as a reference to non-`const` argument can be assumed to be a write into the variable.

### <a name="Res-introduce"></a>ES.21: Don't introduce a variable (or constant) before you need to use it

##### Reason

Readability. To limit the scope in which the variable can be used.

##### Example

    int x = 7;
    // ... no use of x here ...
    ++x;

##### Enforcement

Flag declarations that are distant from their first use.

### <a name="Res-init"></a>ES.22: Don't declare a variable until you have a value to initialize it with

##### Reason

Readability. Limit the scope in which a variable can be used. Don't risk used-before-set. Initialization is often more efficient than assignment.

##### Example, bad

    string s;
    // ... no use of s here ...
    s = "what a waste";

##### Example, bad

    SomeLargeType var;   // ugly CaMeLcAsEvArIaBlE

    if (cond)   // some non-trivial condition
        Set(&var);
    else if (cond2 || !cond3) {
        var = Set2(3.14);
    }
    else {
        var = 0;
        for (auto& e : something)
            var += e;
    }

    // use var; that this isn't done too early can be enforced statically with only control flow

This would be fine if there was a default initialization for `SomeLargeType` that wasn't too expensive.
Otherwise, a programmer might very well wonder if every possible path through the maze of conditions has been covered.
If not, we have a "use before set" bug. This is a maintenance trap.

For initializers of moderate complexity, including for `const` variables, consider using a lambda to express the initializer; see [ES.28](#Res-lambda-init).

##### Enforcement

* Flag declarations with default initialization that are assigned to before they are first read.
* Flag any complicated computation after an uninitialized variable and before its use.

### <a name="Res-list"></a>ES.23: Prefer the `{}` initializer syntax

##### Reason

The rules for `{}` initialization are simpler, more general, less ambiguous, and safer than for other forms of initialization.

##### Example

    int x {f(99)};
    vector<int> v = {1, 2, 3, 4, 5, 6};

##### Exception

For containers, there is a tradition for using `{...}` for a list of elements and `(...)` for sizes:

    vector<int> v1(10);    // vector of 10 elements with the default value 0
    vector<int> v2 {10};   // vector of 1 element with the value 10

##### Note

`{}`-initializers do not allow narrowing conversions.

##### Example

    int x {7.9};   // error: narrowing
    int y = 7.9;   // OK: y becomes 7. Hope for a compiler warning

##### Note

`{}` initialization can be used for all initialization; other forms of initialization can't:

    auto p = new vector<int> {1, 2, 3, 4, 5};   // initialized vector
    D::D(int a, int b) :m{a, b} {   // member initializer (e.g., m might be a pair)
        // ...
    };
    X var {};   // initialize var to be empty
    struct S {
        int m {7};   // default initializer for a member
        // ...
    };

##### Note

Initialization of a variable declared using `auto` with a single value, e.g., `{v}`, had surprising results until recently:

    auto x1 {7};        // x1 is an int with the value 7
    // x2 is an initializer_list<int> with an element 7
    // (this will will change to "element 7" in C++17)
    auto x2 = {7};

    auto x11 {7, 8};    // error: two initializers
    auto x22 = {7, 8};  // x2 is an initializer_list<int> with elements 7 and 8

##### Exception

Use `={...}` if you really want an `initializer_list<T>`

    auto fib10 = {0, 1, 2, 3, 5, 8, 13, 25, 38, 63};   // fib10 is a list

##### Note

Old habits die hard, so this rule is hard to apply consistently, especially as there are so many cases where `=` is innocent.

##### Example

    template<typename T>
    void f()
    {
        T x1(1);    // T initialized with 1
        T x0();     // bad: function declaration (often a mistake)

        T y1 {1};   // T initialized with 1
        T y0 {};    // default initialized T
        // ...
    }

**See also**: [Discussion](#???)

##### Enforcement

Tricky.

* Don't flag uses of `=` for simple initializers.
* Look for `=` after `auto` has been seen.

### <a name="Res-unique"></a>ES.24: Use a `unique_ptr<T>` to hold pointers

##### Reason

Using `std::unique_ptr` is the simplest way to avoid leaks. It is reliable, it
makes the type system do much of the work to validate ownership safety, it
increases readability, and it has zero or near zero runtime cost.

##### Example

    void use(bool leak)
    {
        auto p1 = make_unique<int>(7);   // OK
        int* p2 = new int{7};            // bad: might leak
        // ...
        if (leak) return;
        // ...
    }

If `leak == true` the object pointed to by `p2` is leaked and the object pointed to by `p1` is not.

##### Enforcement

Look for raw pointers that are targets of `new`, `malloc()`, or functions that may return such pointers.

### <a name="Res-const"></a>ES.25: Declare objects `const` or `constexpr` unless you want to modify its value later on

##### Reason

That way you can't change the value by mistake. That way may offer the compiler optimization opportunities.

##### Example

    void f(int n)
    {
        const int bufmax = 2 * n + 2;  // good: we can't change bufmax by accident
        int xmax = n;                  // suspicious: is xmax intended to change?
        // ...
    }

##### Enforcement

Look to see if a variable is actually mutated, and flag it if
not. Unfortunately, it may be impossible to detect when a non-`const` was not
*intended* to vary (vs when it merely did not vary).

### <a name="Res-recycle"></a>ES.26: Don't use a variable for two unrelated purposes

##### Reason

Readability.

##### Example, bad

    void use()
    {
        int i;
        for (i = 0; i < 20; ++i) { /* ... */ }
        for (i = 0; i < 200; ++i) { /* ... */ } // bad: i recycled
    }

##### Enforcement

Flag recycled variables.

### <a name="Res-stack"></a>ES.27: Use `std::array` or `stack_array` for arrays on the stack

##### Reason

They are readable and don't implicitly convert to pointers.
They are not confused with non-standard extensions of built-in arrays.

##### Example, bad

    const int n = 7;
    int m = 9;

    void f()
    {
        int a1[n];
        int a2[m];   // error: not ISO C++
        // ...
    }

##### Note

The definition of `a1` is legal C++ and has always been.
There is a lot of such code.
It is error-prone, though, especially when the bound is non-local.
Also, it is a "popular" source of errors (buffer overflow, pointers from array decay, etc.).
The definition of `a2` is C but not C++ and is considered a security risk

##### Example

    const int n = 7;
    int m = 9;

    void f()
    {
        array<int, n> a1;
        stack_array<int> a2(m);
        // ...
    }

##### Enforcement

* Flag arrays with non-constant bounds (C-style VLAs)
* Flag arrays with non-local constant bounds

### <a name="Res-lambda-init"></a>ES.28: Use lambdas for complex initialization, especially of `const` variables

##### Reason

It nicely encapsulates local initialization, including cleaning up scratch variables needed only for the initialization, without needing to create a needless nonlocal yet nonreusable function. It also works for variables that should be `const` but only after some initialization work.

##### Example, bad

    widget x;   // should be const, but:
    for (auto i = 2; i <= N; ++i) {             // this could be some
        x += some_obj.do_something_with(i);  // arbitrarily long code
    }                                        // needed to initialize x
    // from here, x should be const, but we can't say so in code in this style

##### Example, good

    const widget x = [&]{
        widget val;                                // assume that widget has a default constructor
        for (auto i = 2; i <= N; ++i) {            // this could be some
            val += some_obj.do_something_with(i);  // arbitrarily long code
        }                                          // needed to initialize x
        return val;
    }();

##### Example

    string var = [&]{
        if (!in) return "";   // default
        string s;
        for (char c : in >> c)
            s += toupper(c);
        return s;
    }(); // note ()

If at all possible, reduce the conditions to a simple set of alternatives (e.g., an `enum`) and don't mix up selection and initialization.

##### Example

    owner<istream&> in = [&]{
        switch (source) {
        case default:       owned = false; return cin;
        case command_line:  owned = true;  return *new istringstream{argv[2]};
        case file:          owned = true;  return *new ifstream{argv[2]};
    }();

##### Enforcement

Hard. At best a heuristic. Look for an uninitialized variable followed by a loop assigning to it.

### <a name="Res-macros"></a>ES.30: Don't use macros for program text manipulation

##### Reason

Macros are a major source of bugs.
Macros don't obey the usual scope and type rules.
Macros ensure that the human reader see something different from what the compiler sees.
Macros complicates tool building.

##### Example, bad

    #define Case break; case   /* BAD */

This innocuous-looking macro makes a single lower case `c` instead of a `C` into a bad flow-control bug.

##### Note

This rule does not ban the use of macros for "configuration control" use in `#ifdef`s, etc.

##### Enforcement

Scream when you see a macro that isn't just use for source control (e.g., `#ifdef`)

### <a name="Res-macros2"></a>ES.31: Don't use macros for constants or "functions"

##### Reason

Macros are a major source of bugs.
Macros don't obey the usual scope and type rules.
Macros don't obey the usual rules for argument passing.
Macros ensure that the human reader sees something different from what the compiler sees.
Macros complicate tool building.

##### Example, bad

    #define PI 3.14
    #define SQUARE(a, b) (a * b)

Even if we hadn't left a well-known bug in `SQUARE` there are much better behaved alternatives; for example:

    constexpr double pi = 3.14;
    template<typename T> T square(T a, T b) { return a * b; }

##### Enforcement

Scream when you see a macro that isn't just used for source control (e.g., `#ifdef`)

### <a name="Res-ALL_CAPS"></a>ES.32: Use `ALL_CAPS` for all macro names

##### Reason

Convention. Readability. Distinguishing macros.

##### Example

    #define forever for (;;)   /* very BAD */

    #define FOREVER for (;;)   /* Still evil, but at least visible to humans */

##### Enforcement

Scream when you see a lower case macro.

### <a name="Res-MACROS"></a>ES.33: If you must use macros, give them unique names

##### Reason

Macros do not obey scope rules.

##### Example

    #define MYCHAR        /* BAD, will eventually clash with someone else's MYCHAR*/

    #define ZCORP_CHAR    /* Still evil, but less likely to clash */

##### Note

Avoid macros if you can: [ES.30](#Res-macros), [ES.31](#Res-macros2), and [ES.32](#Res-ALL_CAPS).
However, there are billions of lines of code littered with macros and a long tradition for using and overusing macros.
If you are forced to use macros, use long names and supposedly unique prefixes (e.g., your organization's name) to lower the likelihood of a clash.

##### Enforcement

Warn against short macro names.

### <a name="Res-ellipses"></a> ES.34: Don't define a (C-style) variadic function

##### Reason

Not type safe.
Requires messy cast-and-macro-laden code to get working right.

##### Example

    #include<cstdarg>

    // "severity" followed by a zero-terminated list of char*s; write the C-style strings to cerr
    void error(int severity ...)
    {
        va_list ap;             // a magic type for holding arguments
        va_start(ap, severity); // arg startup: "severity" is the first argument of error()

        for (;;) {
            // treat the next var as a char*; no checking: a cast in disguise
            char* p = va_arg(ap, char*);
            if (p == nullptr) break;
            cerr << p << ' ';
        }

        va_end(ap);             // arg cleanup (don't forget this)

        cerr << '\n';
        if (severity) exit(severity);
    }

    void use()
    {
        error(7, "this", "is", "an", "error", nullptr);
        error(7); // crash
        error(7, "this", "is", "an", "error");  // crash
        const char* is = "is";
        string an = "an";
        error(7, "this", "is", an, "error"); // crash
    }

**Alternative**: Overloading. Templates. Variadic templates.

##### Note

This is basically the way `printf` is implemented.

##### Enforcement

* Flag definitions of C-style variadic functions.
* Flag `#include<cstdarg>` and `#include<stdarg.h>`

## ES.stmt: Statements

Statements control the flow of control (except for function calls and exception throws, which are expressions).

### <a name="Res-switch-if"></a>ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice

##### Reason

* Readability.
* Efficiency: A `switch` compares against constants and is usually better optimized than a series of tests in an `if`-`then`-`else` chain.
* a `switch` is enables some heuristic consistency checking. For example, have all values of an `enum` been covered? If not, is there a `default`?

##### Example

    void use(int n)
    {
        switch (n) {   // good
        case 0:   // ...
        case 7:   // ...
        }
    }

rather than:

    void use2(int n)
    {
        if (n == 0)   // bad: if-then-else chain comparing against a set of constants
            // ...
        else if (n == 7)
            // ...
    }

##### Enforcement

Flag `if`-`then`-`else` chains that check against constants (only).

### <a name="Res-for-range"></a>ES.71: Prefer a range-`for`-statement to a `for`-statement when there is a choice

##### Reason

Readability. Error prevention. Efficiency.

##### Example

    for (int i = 0; i < v.size(); ++i)   // bad
            cout << v[i] << '\n';

    for (auto p = v.begin(); p != v.end(); ++p)   // bad
        cout << *p << '\n';

    for (auto& x : v)    // OK
        cout << x << '\n';

    for (int i = 1; i < v.size(); ++i) // touches two elements: can't be a range-for
        cout << v[i] + v[i - 1] << '\n';

    for (int i = 0; i < v.size(); ++i) // possible side-effect: can't be a range-for
        cout << f(v, &v[i]) << '\n';

    for (int i = 0; i < v.size(); ++i) { // body messes with loop variable: can't be a range-for
        if (i % 2)
            ++i;   // skip even elements
        else
            cout << v[i] << '\n';
    }

A human or a good static analyzer may determine that there really isn't a side effect on `v` in `f(v, &v[i])` so that the loop can be rewritten.

"Messing with the loop variable" in the body of a loop is typically best avoided.

##### Note

Don't use expensive copies of the loop variable of a range-`for` loop:

    for (string s : vs) // ...

This will copy each elements of `vs` into `s`. Better:

    for (string& s : vs) // ...

Better still, if the loop variable isn't modified or copied:

    for (const string& s : vs) // ...

##### Enforcement

Look at loops, if a traditional loop just looks at each element of a sequence, and there are no side-effects on what it does with the elements, rewrite the loop to a ranged-`for` loop.

### <a name="Res-for-while"></a>ES.72: Prefer a `for`-statement to a `while`-statement when there is an obvious loop variable

##### Reason

Readability: the complete logic of the loop is visible "up front". The scope of the loop variable can be limited.

##### Example

    for (int i = 0; i < vec.size(); i++) {
        // do work
    }

##### Example, bad

    int i = 0;
    while (i < vec.size()) {
        // do work
        i++;
    }

##### Enforcement

???

### <a name="Res-while-for"></a>ES.73: Prefer a `while`-statement to a `for`-statement when there is no obvious loop variable

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Res-for-init"></a>ES.74: Prefer to declare a loop variable in the initializer part of as `for`-statement

##### Reason

Limit the loop variable visibility to the scope of the loop.
Avoid using the loop variable for other purposes after the loop.

##### Example

    for (int i = 0; i < 100; ++i) {   // GOOD: i var is visible only inside the loop
        // ...
    }

##### Example, don't

    int j;                            // BAD: j is visible outside the loop
    for (j = 0; j < 100; ++j) {
        // ...
    }
    // j is still visible here and isn't needed

**See also**: [Don't use a variable for two unrelated purposes](#Res-recycle)

##### Enforcement

Warn when a variable modified inside the `for`-statement is declared outside the loop and not being used outside the loop.

**Discussion**: Scoping the loop variable to the loop body also helps code optimizers greatly. Recognizing that the induction variable
is only accessible in the loop body unblocks optimizations such as hoisting, strength reduction, loop-invariant code motion, etc.

### <a name="Res-do"></a>ES.75: Avoid `do`-statements

##### Reason

Readability, avoidance of errors.
The termination condition is at the end (where it can be overlooked) and the condition is not checked the first time through. ???

##### Example

    int x;
    do {
        cin >> x;
        // ...
    } while (x < 0);

##### Enforcement

???

### <a name="Res-goto"></a>ES.76: Avoid `goto`

##### Reason

Readability, avoidance of errors. There are better control structures for humans; `goto` is for machine generated code.

##### Exception

Breaking out of a nested loop. In that case, always jump forwards.

##### Example

    ???

##### Example

There is a fair amount of use of the C goto-exit idiom:

    void f()
    {
        // ...
            goto exit;
        // ...
            goto exit;
        // ...
    exit:
        ... common cleanup code ...
    }

This is an ad-hoc simulation of destructors. Declare your resources with handles with destructors that clean up.

##### Enforcement

* Flag `goto`. Better still flag all `goto`s that do not jump from a nested loop to the statement immediately after a nest of loops.

### <a name="Res-continue"></a>ES.77: ??? `continue`

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Res-break"></a>ES.78: Always end a non-empty `case` with a `break`

##### Reason

 Accidentally leaving out a `break` is a fairly common bug.
 A deliberate fallthrough is a maintenance hazard.

##### Example

    switch (eventType)
    {
    case Information:
        update_status_bar();
        break;
    case Warning:
        write_event_log();
    case Error:
        display_error_window(); // Bad
        break;
    }

It is easy to overlook the fallthrough. Be explicit:

    switch (eventType)
    {
    case Information:
        update_status_bar();
        break;
    case Warning:
        write_event_log();
        // fallthrough
    case Error:
        display_error_window(); // Bad
        break;
    }

There is a proposal for a `[[fallthrough]]` annotation.

##### Note

Multiple case labels of a single statement is OK:

    switch (x) {
    case 'a':
    case 'b':
    case 'f':
        do_something(x);
        break;
    }

##### Enforcement

Flag all fallthroughs from non-empty `case`s.

### <a name="Res-default"></a>ES.79: ??? `default`

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Res-empty"></a>ES.85: Make empty statements visible

##### Reason

Readability.

##### Example

    for (i = 0; i < max; ++i);   // BAD: the empty statement is easily overlooked
    v[i] = f(v[i]);

    for (auto x : v) {           // better
        // nothing
    }
    v[i] = f(v[i]);

##### Enforcement

Flag empty statements that are not blocks and don't contain comments.

### <a name="Res-loop-counter"></a>ES.86: Avoid modifying loop control variables inside the body of raw for-loops

##### Reason

The loop control up front should enable correct reasoning about what is happening inside the loop. Modifying loop counters in both the iteration-expression and inside the body of the loop is a perennial source of surprises and bugs.

##### Example

    for (int i = 0; i < 10; ++i) {
        // no updates to i -- ok
    }

    for (int i = 0; i < 10; ++i) {
        //
        if (/* something */) ++i; // BAD
        //
    }

    bool skip = false;
    for (int i = 0; i < 10; ++i) {
        if (skip) { skip = false; continue; }
        //
        if (/* something */) skip = true;  // Better: using two variable for two concepts.
        //
    }

##### Enforcement

Flag variables that are potentially updated (have a non-const use) in both the loop control iteration-expression and the loop body.

## ES.expr: Expressions

Expressions manipulate values.

### <a name="Res-complicated"></a>ES.40: Avoid complicated expressions

##### Reason

Complicated expressions are error-prone.

##### Example

    // bad: assignment hidden in subexpression
    while ((c = getc()) != -1)

    // bad: two non-local variables assigned in a sub-expressions
    while ((cin >> c1, cin >> c2), c1 == c2)

    // better, but possibly still too complicated
    for (char c1, c2; cin >> c1 >> c2 && c1 == c2;)

    // OK: iff i and j are not aliased
    int x = ++i + ++j;

    // OK: iff i != j and i != k
    v[i] = v[j] + v[k];

    // bad: multiple assignments "hidden" in subexpressions
    x = a + (b = f()) + (c = g()) * 7;

    // bad: relies on commonly misunderstood precedence rules
    x = a & b + c * d && e ^ f == 7;

    // bad: undefined behavior
    x = x++ + x++ + ++x;

Some of these expressions are unconditionally bad (e.g., they rely on undefined behavior). Others are simply so complicated and/or unusual that even good programmers could misunderstand them or overlook a problem when in a hurry.

##### Note

A programmer should know and use the basic rules for expressions.

##### Example

    x = k * y + z;             // OK

    auto t1 = k * y;           // bad: unnecessarily verbose
    x = t1 + z;

    if (0 <= x && x < max)   // OK

    auto t1 = 0 <= x;        // bad: unnecessarily verbose
    auto t2 = x < max;
    if (t1 && t2)            // ...

##### Enforcement

Tricky. How complicated must an expression be to be considered complicated? Writing computations as statements with one operation each is also confusing. Things to consider:

* side effects: side effects on multiple non-local variables (for some definition of non-local) can be suspect, especially if the side effects are in separate subexpressions
* writes to aliased variables
* more than N operators (and what should N be?)
* reliance of subtle precedence rules
* uses undefined behavior (can we catch all undefined behavior?)
* implementation defined behavior?
* ???

### <a name="Res-parens"></a>ES.41: If in doubt about operator precedence, parenthesize

##### Reason

Avoid errors. Readability. Not everyone has the operator table memorized.

##### Example

    const unsigned int flag = 2;
    unsigned int a = flag;

    if (a & flag != 0)  // bad: means a&(flag != 0)

Note: We recommend that programmers know their precedence table for the arithmetic operations, the logical operations, but consider mixing bitwise logical operations with other operators in need of parentheses.

    if ((a & flag) != 0)  // OK: works as intended

##### Note

You should know enough not to need parentheses for:

    if (a < 0 || a <= max) {
        // ...
    }

##### Enforcement

* Flag combinations of bitwise-logical operators and other operators.
* Flag assignment operators not as the leftmost operator.
* ???

### <a name="Res-ptr"></a>ES.42: Keep use of pointers simple and straightforward

##### Reason

Complicated pointer manipulation is a major source of errors.

* Do all pointer arithmetic on a `span` (exception ++p in simple loop???)
* Avoid pointers to pointers
* ???

##### Example

    ???

##### Enforcement

We need a heuristic limiting the complexity of pointer arithmetic statement.

### <a name="Res-order"></a>ES.43: Avoid expressions with undefined order of evaluation

##### Reason

You have no idea what such code does. Portability.
Even if it does something sensible for you, it may do something different on another compiler (e.g., the next release of your compiler) or with a different optimizer setting.

##### Example

    v[i] = ++i;   //  the result is undefined

A good rule of thumb is that you should not read a value twice in an expression where you write to it.

##### Example

    ???

##### Note

What is safe?

##### Enforcement

Can be detected by a good analyzer.

### <a name="Res-order-fct"></a>ES.44: Don't depend on order of evaluation of function arguments

##### Reason

Because that order is unspecified.

##### Example

    int i = 0;
    f(++i, ++i);

The call will most likely be `f(0, 1)` or `f(1, 0)`, but you don't know which. Technically, the behavior is undefined.

##### Example

??? overloaded operators can lead to order of evaluation problems (shouldn't :-()

    f1()->m(f2());   // m(f1(), f2())
    cout << f1() << f2();   // operator<<(operator<<(cout, f1()), f2())

##### Enforcement

Can be detected by a good analyzer.

### <a name="Res-magic"></a>ES.45: Avoid "magic constants"; use symbolic constants

##### Reason

Unnamed constants embedded in expressions are easily overlooked and often hard to understand:

##### Example

    for (int m = 1; m <= 12; ++m)   // don't: magic constant 12
        cout << month[m] << '\n';

No, we don't all know that there are 12 months, numbered 1..12, in a year. Better:

    constexpr int month_count = 12;   // months are numbered 1..12

    for (int m = first_month; m <= month_count; ++m)   // better
        cout << month[m] << '\n';

Better still, don't expose constants:

    for (auto m : month)
        cout << m << '\n';

##### Enforcement

Flag literals in code. Give a pass to `0`, `1`, `nullptr`, `\n`, `""`, and others on a positive list.

### <a name="Res-narrowing"></a>ES.46: Avoid lossy (narrowing, truncating) arithmetic conversions

##### Reason

A narrowing conversion destroys information, often unexpectedly so.

##### Example, bad

A key example is basic narrowing:

    double d = 7.9;
    int i = d;    // bad: narrowing: i becomes 7
    i = (int) d;  // bad: we're going to claim this is still not explicit enough

    void f(int x, long y, double d)
    {
        char c1 = x;   // bad: narrowing
        char c2 = y;   // bad: narrowing
        char c3 = d;   // bad: narrowing
    }

##### Note

The guideline support library offers a `narrow` operation for specifying that narrowing is acceptable and a `narrow` ("narrow if") that throws an exception if a narrowing would throw away information:

    i = narrow_cast<int>(d);   // OK (you asked for it): narrowing: i becomes 7
    i = narrow<int>(d);        // OK: throws narrowing_error

We also include lossy arithmetic casts, such as from a negative floating point type to an unsigned integral type:

    double d = -7.9;
    unsigned u = 0;

    u = d;                          // BAD
    u = narrow_cast<unsigned>(d);   // OK (you asked for it): u becomes 0
    u = narrow<unsigned>(d);        // OK: throws narrowing_error

##### Enforcement

A good analyzer can detect all narrowing conversions. However, flagging all narrowing conversions will lead to a lot of false positives. Suggestions:

* flag all floating-point to integer conversions (maybe only `float`->`char` and `double`->`int`. Here be dragons! we need data)
* flag all `long`->`char` (I suspect `int`->`char` is very common. Here be dragons! we need data)
* consider narrowing conversions for function arguments especially suspect

### <a name="Res-nullptr"></a>ES.47: Use `nullptr` rather than `0` or `NULL`

##### Reason

Readability. Minimize surprises: `nullptr` cannot be confused with an
`int`. `nullptr` also has a well-specified (very restrictive) type, and thus
works in more scenarios where type deduction might do the wrong thing on `NULL`
or `0`.

##### Example

Consider:

    void f(int);
    void f(char*);
    f(0);         // call f(int)
    f(nullptr);   // call f(char*)

##### Enforcement

Flag uses of `0` and `NULL` for pointers. The transformation may be helped by simple program transformation.

### <a name="Res-casts"></a>ES.48: Avoid casts

##### Reason

Casts are a well-known source of errors. Makes some optimizations unreliable.

##### Example

    ???

##### Note

Programmer who write casts typically assumes that they know what they are doing.
In fact, they often disable the general rules for using values.
Overload resolution and template instantiation usually pick the right function if there is a right function to pick.
If there is not, maybe there ought to be, rather than applying a local fix (cast).

##### Note

Casts are necessary in a systems programming language.  For example, how else
would we get the address of a device register into a pointer?  However, casts
are seriously overused as well as a major source of errors.

##### Note

If you feel the need for a lot of casts, there may be a fundamental design problem.

##### Enforcement

* Force the elimination of C-style casts
* Warn against named casts
* Warn if there are many functional style casts (there is an obvious problem in quantifying 'many').

### <a name="Res-casts-named"></a>ES.49: If you must use a cast, use a named cast

##### Reason

Readability. Error avoidance.
Named casts are more specific than a C-style or functional cast, allowing the compiler to catch some errors.

The named casts are:

* `static_cast`
* `const_cast`
* `reinterpret_cast`
* `dynamic_cast`
* `std::move`         // `move(x)` is an rvalue reference to `x`
* `std::forward`      // `forward(x)` is an rvalue reference to `x`
* `gsl::narrow_cast`  // `narrow_cast<T>(x)` is `static_cast<T>(x)`
* `gsl::narrow`       // `narrow<T>(x)` is `static_cast<T>(x)` if `static_cast<T>(x) == x` or it throws `narrowing_error`

##### Example

    ???

##### Note

When converting between types with no information loss (e.g. from `float` to
`double` or `int64` from `int32`), brace initialization may be used instead.

    double d{some_float};
    int64_t i{some_int32};

This makes it clear that the type conversion was intended and also prevents
conversions between types that might result in loss of precision. (It is a
compilation error to try to initialize a `float` from a `double` in this fashion,
for example.)

##### Enforcement

Flag C-style and functional casts.

### <a name="Res-casts-const"></a>ES.50: Don't cast away `const`

##### Reason

It makes a lie out of `const`.

##### Note

Usually the reason to "cast away `const`" is to allow the updating of some transient information of an otherwise immutable object.
Examples are caching, memoization, and precomputation.
Such examples are often handled as well or better using `mutable` or an indirection than with a `const_cast`.

##### Example

Consider keeping previously computed results around for a costly operation:

    int compute(int x); // compute a value for x; assume this to be costly

    class Cache {   // some type implementing a cache for an int->int operation
    public:
        pair<bool, int> find(int x) const;   // is there a value for x?
        void set(int x, int v);             // make y the value for x
        // ...
    private:
        // ...
    };

    class X {
    public:
        int get_val(int x)
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache.set(x, val); // insert value for x
            return val;
        }
        // ...
    private:
        Cache cache;
    };

Here, `get_val()` is logically constant, so we would like to make it a `const` member.
To do this we still need to mutate `cache`, so people sometimes resort to a `const_cast`:

    class X {   // Suspicious solution based on casting
    public:
        int get_val(int x) const
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            const_cast<Cache&>(cache).set(x, val);   // ugly
            return val;
        }
        // ...
    private:
        Cache cache;
    };

Fortunately, there is a better solution:
State that `cache` is mutable even for a `const` object:

    class X {   // better solution
    public:
        int get_val(int x) const
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache.set(x, val);
            return val;
        }
        // ...
    private:
        mutable Cache cache;
    };

An alternative solution would to store a pointer to the `cache`:

    class X {   // OK, but slightly messier solution
    public:
        int get_val(int x) const
        {
            auto p = cache->find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache->set(x, val);
            return val;
        }
        // ...
    private:
        unique_ptr<Cache> cache;
    };

That solution is the most flexible, but requires explicit construction and destruction of `*cache`
(most likely in the constructor and destructor of `X`).

In any variant, we must guard against data races on the `cache` in multithreaded code, possibly using a `std::mutex`.

##### Enforcement

Flag `const_cast`s.

### <a name="Res-range-checking"></a>ES.55: Avoid the need for range checking

##### Reason

Constructs that cannot overflow do not overflow (and usually run faster):

##### Example

    for (auto& x : v)      // print all elements of v
        cout << x << '\n';

    auto p = find(v, x);   // find x in v

##### Enforcement

Look for explicit range checks and heuristically suggest alternatives.

### <a name="Res-move"></a>ES.56: Write `std::move()` only when you need to explicitly move an object to another scope

##### Reason

We move, rather than copy, to avoid duplication and for improved performance.

A move typically leaves behind an empty object ([C.64](#Rc-move-semantic)), which can be surprising or even dangerous, so we try to avoid moving from lvalues (they might be accessed later).

##### Notes

Moving is done implicitly when the source is an rvalue (e.g., value in a `return` treatment or a function result), so don't pointlessly complicate code in those cases by writing `move` explicitly. Instead, write short functions that return values, and both the function's return and the caller's accepting of the return will be optimized naturally.

In general, following the guidelines in this document (including not making variables' scopes needlessly large, writing short functions that return values, returning local variables) help eliminate most need for explicit `std::move`.

Explicit `move` is needed to explicitly move an object to another scope, notably to pass it to a "sink" function and in the implementations of the move operations themselves (move constructor, move assignment operator) and swap operations.

##### Example, bad

    void sink(X&& x);   // sink takes ownership of x

    void user()
    {
        X x;
        // error: cannot bind an lvalue to a rvalue reference
        sink(x);
        // OK: sink takes the contents of x, x must now be assumed to be empty
        sink(std::move(x));

        // ...

        // probably a mistake
        use(x);
    }

Usually, a `std::move()` is used as an argument to a `&&` parameter.
And after you do that, assume the object has been moved from (see [C.64](#Rc-move-semantic)) and don't read its state again until you first set it to a new value.

    void f() {
        string s1 = "supercalifragilisticexpialidocious";

        string s2 = s1;             // ok, takes a copy
        assert(s1 == "supercalifragilisticexpialidocious");  // ok

        // bad, if you want to keep using s1's value
        string s3 = move(s1);

        // bad, assert will likely fail, s1 likely changed
        assert(s1 == "supercalifragilisticexpialidocious");
    }

##### Example

    void sink(unique_ptr<widget> p);  // pass ownership of p to sink()

    void f() {
        auto w = make_unique<widget>();
        // ...
        sink(std::move(w));               // ok, give to sink()
        // ...
        sink(w);    // Error: unique_ptr is carefully designed so that you cannot copy it
    }

##### Notes

`std::move()` is a cast to `&&` in disguise; it doesn't itself move anything, but marks a named object as a candidate that can be moved from.
The language already knows the common cases where objects can be moved from, especially when returning values from functions, so don't complicate code with redundant `std::move()`'s.

Never write `std::move()` just because you've heard "it's more efficient."
In general, don't believe claims of "efficiency" without data (???).
In general, don't complicate your code without reason (??)

##### Example, bad

    vector<int> make_vector() {
        vector<int> result;
        // ... load result with data
        return std::move(result);       // bad; just write "return result;"
    }

Never write `return move(local_variable);`, because the language already knows the variable is a move candidate.
Writing `move` in this code won't help, and can actually be detrimental because on some compilers it interferes with RVO (the return value optimization) by creating an additional reference alias to the local variable.


##### Example, bad

    vector<int> v = std::move(make_vector());   // bad; the std::move is entirely redundant

Never write `move` on a returned value such as `x = move(f());` where `f` returns by value.
The language already knows that a returned value is a temporary object that can be moved from.

##### Example

    void mover(X&& x) {
        call_something(std::move(x));         // ok
        call_something(std::forward<X>(x));   // bad, don't std::forward an rvalue reference
        call_something(x);                    // suspicious, why not std::move?
    }

    template<class T>
    void forwarder(T&& t) {
        call_something(std::move(t));         // bad, don't std::move a forwarding reference
        call_something(std::forward<T>(t));   // ok
        call_something(t);                    // suspicious, why not std::forward?
    }

##### Enforcement

* Flag use of `std::move(x)` where `x` is an rvalue or the language will already treat it as an rvalue, including `return std::move(local_variable);` and `std::move(f())` on a function that returns by value.
* Flag functions taking an `S&&` parameter if there is no `const S&` overload to take care of lvalues.
* Flag a `std::move`s argument passed to a parameter, except when the parameter type is one of the following: an `X&&` rvalue reference; a `T&&` forwarding reference where `T` is a template parameter type; or by value and the type is move-only.
* Flag when `std::move` is applied to a forwarding reference (`T&&` where `T` is a template parameter type). Use `std::forward` instead.
* Flag when `std::move` is applied to other than an rvalue reference. (More general case of the previous rule to cover the non-forwarding cases.)
* Flag when `std::forward` is applied to an rvalue reference (`X&&` where `X` is a concrete type). Use `std::move` instead.
* Flag when `std::forward` is applied to other than a forwarding reference. (More general case of the previous rule to cover the non-moving cases.)
* Flag when an object is potentially moved from and the next operation is a `const` operation; there should first be an intervening non-`const` operation, ideally assignment, to first reset the object's value.

### <a name="Res-new"></a>ES.60: Avoid `new` and `delete` outside resource management functions

##### Reason

Direct resource management in application code is error-prone and tedious.

##### Note

also known as "No naked `new`!"

##### Example, bad

    void f(int n)
    {
        auto p = new X[n];   // n default constructed Xs
        // ...
        delete[] p;
    }

There can be code in the `...` part that causes the `delete` never to happen.

**See also**: [R: Resource management](#S-resource).

##### Enforcement

Flag naked `new`s and naked `delete`s.

### <a name="Res-del"></a>ES.61: delete arrays using `delete[]` and non-arrays using `delete`

##### Reason

That's what the language requires and mistakes can lead to resource release errors and/or memory corruption.

##### Example, bad

    void f(int n)
    {
        auto p = new X[n];   // n default constructed Xs
        // ...
        delete p;   // error: just delete the object p, rather than delete the array p[]
    }

##### Note

This example not only violates the [no naked `new` rule](#Res-new) as in the previous example, it has many more problems.

##### Enforcement

* if the `new` and the `delete` is in the same scope, mistakes can be flagged.
* if the `new` and the `delete` are in a constructor/destructor pair, mistakes can be flagged.

### <a name="Res-arr2"></a>ES.62: Don't compare pointers into different arrays

##### Reason

The result of doing so is undefined.

##### Example, bad

    void f(int n)
    {
        int a1[7];
        int a2[9];
        if (&a1[5] < &a2[7]) {}       // bad: undefined
        if (0 < &a1[5] - &a2[7]) {}   // bad: undefined
    }

##### Note

This example has many more problems.

##### Enforcement

???

### <a name="Res-slice"></a>ES.63: Don't slice

##### Reason

Slicing -- that is, copying only part of an object using assignment or initialization -- most often leads to errors because
the object was meant to be considered as a whole.
In the rare cases where the slicing was deliberate the code can be surprising.

##### Example

    class Shape { /* ... */ };
    class Circle : public Shape { /* ... */ Point c; int r; };

    Circle c {{ "{{" }}0, 0}, 42};
    Shape s {c};    // copy Shape part of Circle

The result will be meaningless because the center and radius will not be copied from `c` into `s`.
The first defense against this is to [define the base class `Shape` not to allow this](#Rc-copy-virtual).

##### Alternative

If you mean to slice, define an explicit operations to do so.
This saves readers from confusion.
For example:

    class Smiley : public Circle {
        public:
        Circle copy_circle();
        // ...
    };

    Smiley sm { /* ... */ };
    Circle c1 {sm};  // ideally prevented by the definition of Circle
    Circle c2 {sm.copy_circle()};

##### Enforcement

Warn against slicing.

## <a name="SS-numbers"></a>Arithmetic

### <a name="Res-mix"></a>ES.100: Don't mix signed and unsigned arithmetic

##### Reason

Avoid wrong results.

##### Example

    int x = -3;
    unsigned int y = 7;

    cout << x - y << '\n';  // unsigned result, possibly 4294967286
    cout << x + y << '\n';  // unsigned result: 4
    cout << x * y << '\n';  // unsigned result, possibly 4294967275

It is harder to spot the problem in more realistic examples.

##### Note

Unfortunately, C++ uses signed integers for array subscripts and the standard library uses unsigned integers for container subscripts.
This precludes consistency.

##### Enforcement

Compilers already know and sometimes warn.

### <a name="Res-unsigned"></a>ES.101: Use unsigned types for bit manipulation

##### Reason

Unsigned types support bit manipulation without surprises from sign bits.

##### Example

    unsigned char x = 0b1010'1010;
    unsigned char y = ~x;   // y == 0b0101'0101;

##### Note

Unsigned types can also be useful for modulo arithmetic.
However, if you want modulo arithmetic add
comments as necessary noting the reliance on wraparound behavior, as such code
can be surprising for many programmers.

##### Enforcement

* Just about impossible in general because of the use of unsigned subscripts in the standard library
* ???

### <a name="Res-signed"></a>ES.102: Use signed types for arithmetic

##### Reason

Because most arithmetic is assumed to be signed;
`x-y` yields a negative number when `y>x` except in the rare cases where you really want modulo arithmetic.

##### Example

Unsigned arithmetic can yield surprising results if you are not expecting it.
This is even more true for mixed signed and unsigned arithmetic.

    template<typename T, typename T2>
    T subtract(T x, T2 y)
    {
        return x-y;
    }

    void test()
    {
        int s = 5;
        unsigned int us = 5;
        cout << subtract(s, 7) << '\n';     // -2
        cout << subtract(us, 7u) << '\n';   // 4294967294
        cout << subtract(s, 7u) << '\n';    // -2
        cout << subtract(us, 7) << '\n';    // 4294967294
        cout << subtract(s, us+2) << '\n';  // -2
        cout << subtract(us, s+2) << '\n';  // 4294967294
    }

Here we have been very explicit about what's happening,
but if you had see `us-(s+2)` or `s+=2; ... us-s` would you reliably have suspected that the result would print as `4294967294`?

##### Exception

Use unsigned types if you really want modulo arithmetic - add
comments as necessary noting the reliance on overflow behavior, as such code
is going to be surprising for many programmers.

##### Example

The standard library uses unsigned types for subscripts.
The build-in array uses signed types for subscripts.
This makes surprises (and bugs) inevitable.

    int a[10];
    for (int i=0; i < 10; ++i) a[i]=i;
    vector<int> v(10);
    // compares signed to unsigned; some compilers warn
    for (int i=0; v.size() < 10; ++i) v[i]=i;

    int a2[-2];         // error: negative size

    // OK, but the number of ints (4294967294) is so large that we should get an exception
    vector<int> v2(-2);

##### Enforcement

* Flag mixed signed and unsigned arithmetic
* Flag results of unsigned arithmetic assigned to or printed as signed.
* Flag unsigned literals (e.g. `-2`) used as container subscripts.

### <a name="Res-overflow"></a>ES.103: Don't overflow

##### Reason

Overflow usually makes your numeric algorithm meaningless.
Incrementing a value beyond a maximum value can lead to memory corruption and undefined behavior.

##### Example, bad

    int a[10];
    a[10] = 7;   // bad

    int n = 0;
    while (n++ < 10)
        a[n - 1] = 9; // bad (twice)

##### Example, bad

    int n = numeric_limits<int>::max();
    int m = n + 1;   // bad

##### Example, bad

    int area(int h, int w) { return h * w; }

    auto a = area(10'000'000, 100'000'000);   // bad

##### Exception

Use unsigned types if you really want modulo arithmetic.

**Alternative**: For critical applications that can afford some overhead, use a range-checked integer and/or floating-point type.

##### Enforcement

???

### <a name="Res-underflow"></a>ES.104: Don't underflow

##### Reason

Decrementing a value beyond a minimum value can lead to memory corruption and undefined behavior.

##### Example, bad

    int a[10];
    a[-2] = 7;   // bad

    int n = 101;
    while (n--)
        a[n - 1] = 9;   // bad (twice)

##### Exception

Use unsigned types if you really want modulo arithmetic.

##### Enforcement

???

### <a name="Res-zero"></a>ES.105: Don't divide by zero

##### Reason

The result is undefined and probably a crash.

##### Note

This also applies to `%`.

##### Example; bad

    double divide(int a, int b) {
        // BAD, should be checked (e.g., in a precondition)
        return a / b;
    }

##### Example; good

    double divide(int a, int b) {
        // good, address via precondition (and replace with contracts once C++ gets them)
        Expects(b != 0);
        return a / b;
    }

    double divide(int a, int b) {
        // good, address via check
        return b ? a / b : quiet_NaN<double>();
    }

**Alternative**: For critical applications that can afford some overhead, use a range-checked integer and/or floating-point type.

##### Enforcement

* Flag division by an integral value that could be zero

# <a name="S-performance"></a>Per: Performance

??? should this section be in the main guide???

This section contains rules for people who need high performance or low-latency.
That is, these are rules that relate to how to use as little time and as few resources as possible to achieve a task in a predictably short time.
The rules in this section are more restrictive and intrusive than what is needed for many (most) applications.
Do not blindly try to follow them in general code: achieving the goals of low latency requires extra work.

Performance rule summary:

* [Per.1: Don't optimize without reason](#Rper-reason)
* [Per.2: Don't optimize prematurely](#Rper-Knuth)
* [Per.3: Don't optimize something that's not performance critical](#Rper-critical)
* [Per.4: Don't assume that complicated code is necessarily faster than simple code](#Rper-simple)
* [Per.5: Don't assume that low-level code is necessarily faster than high-level code](#Rper-low)
* [Per.6: Don't make claims about performance without measurements](#Rper-measure)
* [Per.7: Design to enable optimization](#Rper-efficiency)
* [Per.10: Rely on the static type system](#Rper-type)
* [Per.11: Move computation from run time to compile time](#Rper-Comp)
* [Per.12: Eliminate redundant aliases](#Rper-alias)
* [Per.13: Eliminate redundant indirections](#Rper-indirect)
* [Per.14: Minimize the number of allocations and deallocations](#Rper-alloc)
* [Per.15: Do not allocate on a critical branch](#Rper-alloc0)
* [Per.16: Use compact data structures](#Rper-compact)
* [Per.17: Declare the most used member of a time-critical struct first](#Rper-struct)
* [Per.18: Space is time](#Rper-space)
* [Per.19: Access memory predictably](#Rper-access)
* [Per.30: Avoid context switches on the critical path](#Rper-context)

### <a name="Rper-reason"></a>Per.1: Don't optimize without reason

##### Reason

If there is no need for optimization, the main result of the effort will be more errors and higher maintenance costs.

##### Note

Some people optimize out of habit or because it's fun.

???

### <a name="Rper-Knuth"></a>Per.2: Don't optimize prematurely

##### Reason

Elaborately optimized code is usually larger and harder to change than unoptimized code.

???

### <a name="Rper-critical"></a>Per.3: Don't optimize something that's not performance critical

##### Reason

Optimizing a non-performance-critical part of a program has no effect on system performance.

##### Note

If your program spends most of its time waiting for the web or for a human, optimization of in-memory computation is probably useless.

Put another way: If your program spends 4% of its processing time doing
computation A and 40% of its time doing computation B, a 50% improvement on A is
only as impactful as a 5% improvement on B. (If you don't even know how much
time is spent on A or B, see <a href="#Rper-reason">Per.1</a> and <a
href="#Rper-Knuth">Per.2</a>.)

### <a name="Rper-simple"></a>Per.4: Don't assume that complicated code is necessarily faster than simple code

##### Reason

Simple code can be very fast. Optimizers sometimes do marvels with simple code

##### Example, good

    // clear expression of intent, fast execution

    vector<uint8_t> v(100000);

    for (auto& c : v)
        c = ~c;

##### Example, bad

    // intended to be faster, but is actually slower

    vector<uint8_t> v(100000);

    for (size_t i = 0; i < v.size(); i += sizeof(uint64_t))
    {
        uint64_t& quad_word = *reinterpret_cast<uint64_t*>(&v[i]);
        quad_word = ~quad_word;
    }

##### Note

???

???

### <a name="Rper-low"></a>Per.5: Don't assume that low-level code is necessarily faster than high-level code

##### Reason

Low-level code sometimes inhibits optimizations. Optimizers sometimes do marvels with high-level code.

##### Note

???

???

### <a name="Rper-measure"></a>Per.6: Don't make claims about performance without measurements

##### Reason

The field of performance is littered with myth and bogus folklore.
Modern hardware and optimizers defy naive assumptions; even experts are regularly surprised.

##### Note

Getting good performance measurements can be hard and require specialized tools.

##### Note

A few simple microbenchmarks using Unix `time` or the standard library `<chrono>` can help dispel the most obvious myths.
If you can't measure your complete system accurately, at least try to measure a few of your key operations and algorithms.
A profiler can help tell you which parts of your system are performance critical.
Often, you will be surprised.

???

### <a name="Rper-efficiency"></a>Per.7: Design to enable optimization

##### Reason

Because we often need to optimize the initial design.
Because a design that ignore the possibility of later improvement is hard to change.

##### Example

From the C (and C++) standard:

    void qsort (void* base, size_t num, size_t size, int (*compar)(const void*, const void*));

When did you even want to sort memory?
Really, we sort sequences of elements, typically stored in containers.
A call to `qsort` throws away much useful information (e.g., the element type), forces the user to repeat information
already known (e.g., the element size), and forces the user to write extra code (e.g., a function to compare `double`s).
This implies added work for the programmer, is error prone, and deprives the compiler of information needed for optimization.

    double data[100];
    // ... fill a ...

    // 100 chunks of memory of sizeof(double) starting at
    // address data using the order defined by compare_doubles
    qsort(data, 100, sizeof(double), compare_doubles);

From the point of view of interface design is that `qsort` throws away useful information.

We can do better (in C++98)

    template<typename Iter>
        void sort(Iter b, Iter e);  // sort [b:e)

    sort(data, data + 100);

Here, we use the compiler's knowledge about the size of the array, the type of elements, and how to compare `double`s.

With C++11 plus [concepts](#???), we can do better still

    // Sortable specifies that c must be a
    // random-access sequence of elements comparable with <
    void sort(Sortable& c);

    sort(c);

The key is to pass sufficient information for a good implementation to be chosen.
In this, the `sort` interfaces shown here still have a weakness:
They implicitly rely on the element type having less-than (`<`) defined.
To complete the interface, we need a second version that accepts a comparison criteria:

    // compare elements of c using p
    void sort(Sortable& c, Predicate<Value_type<Sortable>> p);

The standard-library specification of `sort` offers those two versions,
but the semantics is expressed in English rather than code using concepts.

##### Note

Premature optimization is said to be [the root of all evil](#Rper-Knuth), but that's not a reason to despise performance.
It is never premature to consider what makes a design amenable to improvement, and improved performance is a commonly desired improvement.
Aim to build a set of habits that by default results in efficient, maintainable, and optimizable code.
In particular, when you write a function that is not a one-off implementation detail, consider

* Information passing:
Prefer clean [interfaces](#S-interfaces) carrying sufficient information for later improvement of implementation.
Note that information flows into and out of an implementation through the interfaces we provide.
* Compact data: By default, [use compact data](#Rper-compact), such as `std::vector` and [access it in a systematic fashion](#Rper-access).
If you think you need a linked structure, try to craft the interface so that this structure isn't seen by users.
* Function argument passing and return:
Distinguish between mutable and non-mutable data.
Don't impose a resource management burden on your users.
Don't impose spurious run-time indirections on your users.
Use [conventional ways](#Rf-conventional) of passing information through an interface;
unconventional and/or "optimized" ways of passing data can seriously complicate later reimplementation.
* Abstraction:
Don't overgeneralize; a design that tries to cater for every possible use (and misuse) and defers every design decision for later
(using compile-time or run-time indirections) is usually a complicated, bloated, hard-to-understand mess.
Generalize from concrete examples, preserving performance as we generalize.
Do not generalize based on mere speculation about future needs.
The ideal is zero-overhead generalization.
* Libraries:
Use libraries with good interfaces.
If no library is available build one yourself and imitate the interface style from a good library.
The [standard library](#S-stdlib) is a good first place to look for inspiration.
* Isolation:
Isolate your code from messy and/or old style code by providing an interface of your choosing to it.
This is sometimes called "providing a wrapper" for the useful/necessary but messy code.
Don't let bad designs "bleed into" your code.

##### Example

Consider:

    template <class ForwardIterator, class T>
    bool binary_search(ForwardIterator first, ForwardIterator last, const T& val);

`binary_search(begin(c), end(c), 7)` will tell you whether `7` is in `c` or not.
However, it will not tell you where that `7` is or whether there are more than one `7`.

Sometimes, just passing the minimal amount of information back (here, `true` or `false`) is sufficient, but a good interface passes
needed information back to the caller. Therefore, the standard library also offers

    template <class ForwardIterator, class T>
    ForwardIterator lower_bound(ForwardIterator first, ForwardIterator last, const T& val);

`lower_bound` returns an iterator to the first match if any, otherwise `last`.

However, `lower_bound` still doesn't return enough information for all uses, so the standard library also offers

    template <class ForwardIterator, class T>
    pair<ForwardIterator, ForwardIterator>
    equal_range(ForwardIterator first, ForwardIterator last, const T& val);

`equal_range` returns a `pair` of iterators specifying the first and one beyond last match.

    auto r = equal_range(begin(c), end(c), 7);
    for (auto p = r.first(); p != r.second(), ++p)
        cout << *p << '\n';

Obviously, these three interfaces are implemented by the same basic code.
They are simply three ways of presenting the basic binary search algorithm to users,
ranging from the simplest ("make simple things simple!")
to returning complete, but not always needed, information ("don't hide useful information").
Naturally, crafting such a set of interfaces requires experience and domain knowledge.

##### Note

Do not simply craft the interface to match the first implementation and the first use case you think of.
Once your first initial implementation is complete, review it; once you deploy it, mistakes will be hard to remedy.

##### Note

A need for efficiency does not imply a need for [low-level code](#Rper-low).
High-level code does not imply slow or bloated.

##### Note

Things have costs.
Don't be paranoid about costs (modern computers really are very fast),
but have a rough idea of the order of magnitude of cost of what you use.
For example, have a rough idea of the cost of
a memory access,
a function call,
a string comparison,
a system call,
a disk access,
and a message through a network.

##### Note

If you can only think of one implementation, you probably don't have something for which you can devise a stable interface.
Maybe, it is just an implementation detail - not every piece of code needs a stable interface - but pause and consider.
One question that can be useful is
"what interface would be needed if this operation should be implemented using multiple threads? be vectorized?"

##### Note

This rule does not contradict the [Don't optimize prematurely](#Rper-Knuth) rule.
It complements it encouraging developers enable later - appropriate and non-premature - optimization, if and where needed.

##### Enforcement

Tricky.
Maybe looking for `void*` function arguments will find examples of interfaces that hinder later optimization.

### <a name="Rper-type"></a>Per.10: Rely on the static type system

##### Reason

Type violations, weak types (e.g. `void*`s), and low level code (e.g., manipulation of sequences as individual bytes) make the job of the optimizer much harder. Simple code often optimizes better than hand-crafted complex code.

???

### <a name="Rper-Comp"></a>Per.11: Move computation from run time to compile time

???

### <a name="Rper-alias"></a>Per.12: Eliminate redundant aliases

???

### <a name="Rper-indirect"></a>Per.13: Eliminate redundant indirections

???

### <a name="Rper-alloc"></a>Per.14: Minimize the number of allocations and deallocations

???

### <a name="Rper-alloc0"></a>Per.15: Do not allocate on a critical branch

???

### <a name="Rper-compact"></a>Per.16: Use compact data structures

##### Reason

Performance is typically dominated by memory access times.

???

### <a name="Rper-struct"></a>Per.17: Declare the most used member of a time-critical struct first

???

### <a name="Rper-space"></a>Per.18: Space is time

##### Reason

Performance is typically dominated by memory access times.

???

### <a name="Rper-access"></a>Per.19: Access memory predictably

##### Reason

Performance is very sensitive to cache performance and cache algorithms favor simple (usually linear) access to adjacent data.

##### Example

    int matrix[rows][cols];

    // bad
    for (int c = 0; c < cols; ++c)
        for (int r = 0; r < rows; ++r)
            sum += matrix[r][c];

    // good
    for (int r = 0; r < rows; ++r)
        for (int c = 0; c < cols; ++c)
            sum += matrix[r][c];

### <a name="Rper-context"></a>Per.30: Avoid context switches on the critical path

???

# <a name="S-concurrency"></a>CP: Concurrency and Parallelism

We often want our computers to do many tasks at the same time (or at least make them appear to do them at the same time).
The reasons for doing so varies (e.g., wanting to wait for many events using only a single processor, processing many data streams simultaneously, or utilizing many hardware facilities)
and so does the basic facilities for expressing concurrency and parallelism.
Here, we articulate a few general principles and rules for using the ISO standard C++ facilities for expressing basic concurrency and parallelism.

The core machine support for concurrent and parallel programming is the thread.
Threads allow you to run multiple instances of your program independently, while sharing
the same memory. Concurrent programming is tricky for many reasons, most
importantly that it is undefined behavior to read data in one thread after it
was written by another thread, if there is no proper synchronization between
those threads. Making existing single-threaded code execute concurrently can be
as trivial as adding `std::async` or `std::thread` strategically, or it can
necessitate a full rewrite, depending on whether the original code was written
in a thread-friendly way.

The concurrency/parallelism rules in this document are designed with three goals
in mind:

* To help you write code that is amenable to being used in a threaded
  environment
* To show clean, safe ways to use the threading primitives offered by the
  standard library
* To offer guidance on what to do when concurrency and parallelism aren't giving
  you the performance gains you need

It is also important to note that concurrency in C++ is an unfinished
story. C++11 introduced many core concurrency primitives, C++14 improved on
them, and it seems that there is much interest in making the writing of
concurrent programs in C++ even easier. We expect some of the library-related
guidance here to change significantly over time.

This section needs a lot of work (obviously).
Please note that we start with rules for relative non-experts.
Real experts must wait a bit;
contributions are welcome,
but please think about the majority of programmers who are struggling to get their concurrent programs correct and performant.

Concurrency and parallelism rule summary:

* [CP.1: Assume that your code will run as part of a multi-threaded program](#Rconc-multi)
* [CP.2: Avoid data races](#Rconc-races)
* [CP.3: Minimize explicit sharing of writable data](#Rconc-data)
* [CP.4: Think in terms of tasks, rather than threads](#Rconc-task)
* [CP.8 Don't try to use `volatile` for synchronization](#Rconc-volatile)

See also:

* [CP.con: Concurrency](#SScp-con)
* [CP.par: Parallelism](#SScp-par)
* [CP.mess: Message passing](#SScp-mess)
* [CP.vec: Vectorization](#SScp-vec)
* [CP.free: Lock-free programming](#SScp-free)
* [CP.etc: Etc. concurrency rules](#SScp-etc)

### <a name="Rconc-multi"></a>CP.1: Assume that your code will run as part of a multi-threaded program

##### Reason

It is hard to be certain that concurrency isn't used now or will be sometime in the future.
Code gets re-used.
Libraries using threads may be used from some other part of the program.
Note that this applies most urgently to library code and least urgently to stand-alone applications.
However, thanks to the magic of cut-and-paste, code fragments can turn up in unexpected places.

##### Example

    double cached_computation(double x)
    {
        static double cached_x = 0.0;
        static double cached_result = COMPUTATION_OF_ZERO;
        double result;

        if (cached_x == x)
            return cached_result;
        result = computation(x);
        cached_x = x;
        cached_result = result;
        return result;
    }

Although `cached_computation` works perfectly in a single-threaded environment, in a multi-threaded environment the two `static` variables result in data races and thus undefined behavior.

There are several ways that this example could be made safe for a multi-threaded environment:

* Delegate concurrency concerns upwards to the caller.
* Mark the `static` variables as `thread_local` (which might make caching less effective).
* Implement concurrency control, for example, protecting the two `static` variables with a `static` lock (which might reduce performance).
* Have the caller provide the memory to be used for the cache, thereby delegating both memory allocation and concurrency concerns upwards to the caller.
* Refuse to build and/or run in a multi-threaded environment.
* Provide two implementations, one which is used in single-threaded environments and another which is used in multi-threaded environments.

##### Exception

Code that is never run in a multi-threaded environment.

Be careful: there are many examples where code that was "known" to never run in a multi-threaded program
was run as part of a multi-threaded program. Often years later.
Typically, such programs lead to a painful effort to remove data races.
Therefore, code that is never intended to run in a multi-threaded environment should be clearly labeled as such and ideally come with compile or run-time enforcement mechanisms to catch those usage bugs early.

### <a name="Rconc-races"></a>CP.2: Avoid data races

##### Reason

Unless you do, nothing is guaranteed to work and subtle errors will persist.

##### Note

In a nutshell, if two threads can access the same object concurrently (without synchronization), and at least one is a writer (performing a non-`const` operation), you have a data race.
For further information of how to use synchronization well to eliminate data races, please consult a good book about concurrency.

##### Example, bad

There are many examples of data races that exist, some of which are running in
production software at this very moment. One very simple example:

    int get_id() {
      static int id = 1;
      return id++;
    }

The increment here is an example of a data race. This can go wrong in many ways,
including:

* Thread A loads the value of `id`, the OS context switches A out for some
  period, during which other threads create hundreds of IDs. Thread A is then
  allowed to run again, and `id` is written back to that location as A's read of
  `id` plus one.
* Thread A and B load `id` and increment it simultaneously.  They both get the
  same ID.

Local static variables are a common source of data races.

##### Example, bad:

    void f(fstream&  fs, regex pat)
    {
        array<double, max> buf;
        int sz = read_vec(fs, buf, max);            // read from fs into buf
        gsl::span<double> s {buf};
        // ...
        auto h1 = async([&]{ sort(par, s); });     // spawn a task to sort
        // ...
        auto h2 = async([&]{ return find_all(buf, sz, pat); });   // span a task to find matches
        // ...
    }

Here, we have a (nasty) data race on the elements of `buf` (`sort` will both read and write).
All data races are nasty.
Here, we managed to get a data race on data on the stack.
Not all data races are as easy to spot as this one.

##### Example, bad:

    // code not controlled by a lock

    unsigned val;

    if (val < 5) {
        // ... other thread can change val here ...
        switch (val) {
        case 0: // ...
        case 1: // ...
        case 2: // ...
        case 3: // ...
        case 4: // ...
        }
    }

Now, a compiler that does not know that `val` can change will  most likely implement that `switch` using a jump table with five entries.
Then, a `val` outside the `[0..4]` range will cause a jump to an address that could be anywhere in the program, and execution would proceed there.
Really, "all bets are off" if you get a data race.
Actually, it can be worse still: by looking at the generated code you may be able to determine where the stray jump will go for a given value;
this can be a security risk.

##### Enforcement

Some is possible, do at least something.
There are commercial and open-source tools that try to address this problem, but static tools often have many false positives and run-time tools often have a significant cost.
We hope for better tools.

Help the tools:

* less global data
* fewer `static` variables
* more use of stack memory (and don't pass pointers around too much)
* more immutable data (literals, `constexpr`, and `const`)

### <a name="Rconc-data"></a>CP.3: Minimize explicit sharing of writable data

##### Reason

If you don't share writable data, you can't have a data race.
The less sharing you do, the less chance you have to forget to synchronize access (and get data races).
The less sharing you do, the less chance you have to wait on a lock (so performance can improve).

##### Example

    bool validate(const vector<Reading>&);
    Graph<Temp_node> temperature_gradiants(const vector<Reading>&);
    Image altitude_map(const vector<Reading>&);
    // ...

    void process_readings(istream& socket1)
    {
        vector<Reading> surface_readings;
        socket1 >> surface_readings;
        if (!socket1) throw Bad_input{};

        auto h1 = async([&] { if (!validate(surface_readings) throw Invalide_data{}; });
        auto h2 = async([&] { return temperature_gradiants(surface_readings); });
        auto h3 = async([&] { return altitude_map(surface_readings); });
        // ...
        auto v1 = h1.get();
        auto v2 = h2.get();
        auto v3 = h3.get();
        // ...
    }

Without those `const`s, we would have to review every asynchronously invoked function for potential data races on `surface_readings`.

##### Note

Immutable data can be safely and efficiently shared.
No locking is needed: You can't have a data race on a constant.

##### Enforcement

???


### <a name="Rconc-task"></a>CP.4: Think in terms of tasks, rather than threads

##### Reason

A `thread` is an implementation concept, a way of thinking about the machine.
A task is an application notion, something you'd like to do, preferably concurrently with other tasks.
Application concepts are easier to reason about.

##### Example

    ???

##### Note

With the exception of `async()`, the standard-library facilities are low-level, machine-oriented, threads-and-lock level.
This is a necessary foundation, but we have to try to raise the level of abstraction: for productivity, for reliability, and for performance.
This is a potent argument for using higher level, more applications-oriented libraries (if possibly, built on top of standard-library facilities).

##### Enforcement

???

### <a name="Rconc-volatile"></a>CP.8 Don't try to use `volatile` for synchronization

##### Reason

In C++, unlike some other languages, `volatile` does not provide atomicity, does not synchronize between threads,
and does not prevent instruction reordering (neither compiler nor hardware).
It simply has nothing to do with concurrency.

##### Example, bad:

    int free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

Here we have a problem:
This is perfectly good code in a single-threaded program, but have two treads execute this and
there is a race condition on `free_slots` so that two threads might get the same value and `free_slots`.
That's (obviously) a bad data race, so people trained in other languages may try to fix it like this:

    volatile int free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

This has no effect on synchronization: The data race is still there!

The C++ mechanism for this is `atomic` types:

    atomic<int> free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

Now the `--` operation is atomic,
rather than a read-increment-write sequence where another thread might get in-between the individual operations.

##### Alternative

Use `atomic` types where you might have used `volatile` in some other language.
Use a `mutex` for more complicated examples.

##### See also

[(rare) proper uses of `volatile`](#Rconc-volatile2)

## <a name="SScp-con"></a>CP.con: Concurrency

This section focuses on relatively ad-hoc uses of multiple threads communicating through shared data.

* For parallel algorithms, see [parallelism](#SScp-par)
* For inter-task communication without explicit sharing, see [messaging](#SScp-mess)
* For vector parallel code, see [vectorization](#SScp-vec)
* For lock-free programming, see [lock free](#SScp-free)

Concurrency rule summary:

* [CP.20: Use RAII, never plain `lock()`/`unlock()`](#Rconc-raii)
* [CP.21: Use `std::lock()` to acquire multiple `mutex`es](#Rconc-lock)
* [CP.22: Never call unknown code while holding a lock (e.g., a callback)](#Rconc-unknown)
* [CP.23: Think of a joining `thread` as a scoped container](#Rconc-join)
* [CP.24: Think of a detached `thread` as a global container](#Rconc-detach)
* [CP.25: Prefer `gsl::raii_thread` over `std::thread` unless you plan to `detach()`](#Rconc-raii_thread)
* [CP.26: Prefer `gsl::detached_thread` over `std::thread` if you plan to `detach()`](#Rconc-detached_thread)
* [CP.27: Use plain `std::thread` for `thread`s that detach based on a run-time condition (only)](#Rconc-thread)
* [CP.28: Remember to join scoped `thread`s that are not `detach()`ed](#Rconc-join-undetached)
* [CP.30: Do not pass pointers to local variables to non-`raii_thread's](#Rconc-pass)
* [CP.31: Pass small amounts of data between threads by value, rather than by reference or pointer](#Rconc-data-by-value)
* [CP.32: To share ownership between unrelated `thread`s use `shared_ptr`](#Rconc-shared)
* [CP.40: Minimize context switching](#Rconc-switch)
* [CP.41: Minimize thread creation and destruction](#Rconc-create)
* [CP.42: Don't `wait` without a condition](#Rconc-wait)
* [CP.43: Minimize time spent in a critical section](#Rconc-time)
* [CP.44: Remember to name your `lock_guard`s and `unique_lock`s](#Rconc-name)
* [CP.50: Define a `mutex` together with the data it protects](#Rconc-mutex)
* ??? when to use a spinlock
* ??? when to use `try_lock()`
* ??? when to prefer `lock_guard` over `unique_lock`
* ??? Time multiplexing
* ??? when/how to use `new thread`

### <a name="Rconc-raii"></a>CP.20: Use RAII, never plain `lock()`/`unlock()`

##### Reason

Avoids nasty errors from unreleased locks.

##### Example, bad

    mutex mtx;

    void do_stuff()
    {
        mtx.lock();
        // ... do stuff ...
        mtx.unlock();
    }

Sooner or later, someone will forget the `mtx.unlock()`, place a `return` in the `... do stuff ...`, throw an exception, or something.

    mutex mtx;

    void do_stuff()
    {
        unique_lock<mutex> lck {mtx};
        // ... do stuff ...
    }

##### Enforcement

Flag calls of member `lock()` and `unlock()`.  ???


### <a name="Rconc-lock"></a>CP.21: Use `std::lock()` to acquire multiple `mutex`es

##### Reason

To avoid deadlocks on multiple `mutex`s

##### Example

This is asking for deadlock:

    // thread 1
    lock_guard<mutex> lck1(m1);
    lock_guard<mutex> lck2(m2);

    // thread 2
    lock_guard<mutex> lck2(m2);
    lock_guard<mutex> lck1(m1);

Instead, use `lock()`:

    // thread 1
    lock_guard<mutex> lck1(m1, defer_lock);
    lock_guard<mutex> lck2(m2, defer_lock);
    lock(lck1, lck2);

    // thread 2
    lock_guard<mutex> lck2(m2, defer_lock);
    lock_guard<mutex> lck1(m1, defer_lock);
    lock(lck2, lck1);

Here, the writers of `thread1` and `thread2` are still not agreeing on the order of the `mutex`es, but order no longer matters.

##### Note

In real code, `mutex`es are rarely named to conveniently remind the programmer of an intended relation and intended order of acquisition.
In real code, `mutex`es are not always conveniently acquired on consecutive lines.

I'm really looking forward to be able to write plain

    lock_guard lck1(m1, defer_lock);

and have the `mutex` type deduced.

##### Enforcement

Detect the acquisition of multiple `mutex`es.
This is undecidable in general, but catching common simple examples (like the one above) is easy.


### <a name="Rconc-unknown"></a>CP.22: Never call unknown code while holding a lock (e.g., a callback)

##### Reason

If you don't know what a piece of code does, you are risking deadlock.

##### Example

    void do_this(Foo* p)
    {
        lock_guard<mutex> lck {my_mutex};
        // ... do something ...
        p->act(my_data);
        // ...
    }

If you don't know what `Foo::act` does (maybe it is a virtual function invoking a derived class member of a class not yet written),
it may call `do_this` (recursively) and cause a deadlock on `my_mutex`.
Maybe it will lock on a different mutex and not return in a reasonable time, causing delays to any code calling `do_this`.

##### Example

A common example of the "calling unknown code" problem is a call to a function that tries to gain locked access to the same object.
Such problem can often be solved by using a `recursive_mutex`. For example:

    recursive_mutex my_mutex;

    template<typename Action>
    void do_something(Action f)
    {
        unique_lock<recursive_mutex> lck {my_mutex};
        // ... do something ...
        f(this);    // f will do something to *this
        // ...
    }

If, as it is likely, `f()` invokes operations on `*this`, we must make sure that the object's invariant holds before the call.

##### Enforcement

* Flag calling a virtual function with a non-recursive `mutex` held
* Flag calling a callback with a non-recursive `mutex` held


### <a name="Rconc-join"></a>CP.23: Think of a joining `thread` as a scoped container

##### Reason

To maintain pointer safety and avoid leaks, we need to consider what pointers are used by a `thread`.
If a `thread` joins, we can safely pass pointers to objects in the scope of the `thread` and its enclosing scopes.

##### Example

    void f(int * p)
    {
        // ...
        *p = 99;
        // ...
    }
    int glob = 33;

    void some_fct(int* p)
    {
        int x = 77;
        raii_thread t0(f, &x);           // OK
        raii_thread t1(f, p);            // OK
        raii_thread t2(f, &glob);        // OK
        auto q = make_unique<int>(99);
        raii_thread t3(f, q.get());      // OK
        // ...
    }

An `raii_thread` is a `std::thread` with a destructor that joined and cannot be `detached()`.
By "OK" we mean that the object will be in scope ("live") for as long as a `thread` can use the pointer to it.
The fact that `thread`s run concurrently doesn't affect the lifetime or ownership issues here;
these `thread`s can be seen as just a function object called from `some_fct`.

##### Enforcement

Ensure that `raii_thread`s don't `detach()`.
After that, the usual lifetime and ownership (for local objects) enforcement applies.


### <a name="Rconc-detach"></a>CP.24: Think of a detached `thread` as a global container

##### Reason

To maintain pointer safety and avoid leaks, we need to consider what pointers are used by a `thread`.
If a `thread` is detached, we can safely pass pointers to static and free store objects (only).

##### Example

    void f(int * p)
    {
        // ...
        *p = 99;
        // ...
    }

    int glob = 33;

    void some_fct(int* p)
    {
        int x = 77;
        std::thread t0(f, &x);           // bad
        std::thread t1(f, p);            // bad
        std::thread t2(f, &glob);        // OK
        auto q = make_unique<int>(99);
        std::thread t3(f, q.get());      // bad
        // ...
        t0.detach();
        t1.detach();
        t2.detach();
        t3.detach();
        // ...
    }

By "OK" we mean that the object will be in scope ("live") for as long as a `thread` can use the pointers to it.
By "bad" we mean that a `thread` may use a pointer after the pointed-to object is destroyed.
The fact that `thread`s run concurrently doesn't affect the lifetime or ownership issues here;
these `thread`s can be seen as just a function object called from `some_fct`.

##### Enforcement

In general, it is undecidable whether a `detach()` is executed for a `thread`, but simple common cases are easily detected.
If we cannot prove that a `thread` does not `detach()`, we must assume that it does and that it outlives the scope in which it was constructed;
After that, the usual lifetime and ownership (for global objects) enforcement applies.


### <a name="Rconc-raii_thread"></a>CP.25: Prefer `gsl::raii_thread` over `std::thread` unless you plan to `detach()`

##### Reason

An `raii_thread` is a thread that joins at the end of its scope.

Detached threads are hard to monitor.

??? Place all "immortal threads" on the free store rather than `detach()`?

##### Example

    ???

##### Enforcement

???

### <a name="Rconc-detached_thread"></a>CP.26: Prefer `gsl::detached_thread` over `std::thread` if you plan to `detach()`

##### Reason

Often, the need to `detach` is inherent in the `thread`s task.
Documenting that aids comprehension and helps static analysis.

##### Example

    void heartbeat();

    void use()
    {
        gsl::detached_thread t1(heartbeat);    // obviously need not be joined
        std::thread t2(heartbeat);             // do we need to join? (read the code for heartbeat())
        // ...
    }

Flag unconditional `detach` on a plain `thread`


### <a name="Rconc-thread"></a>CP.27: Use plain `std::thread` for `thread`s that detach based on a run-time condition (only)

##### Reason

`thread`s that are supposed to unconditionally `join` or unconditionally `detach` can be clearly identified as such.
The plain `thread`s should be assumed to use the full generality of `std::thread`.

##### Example

    void tricky(thread* t, int n)
    {
        // ...
        if (is_odd(n))
            t->detach();
        // ...
    }

    void use(int n)
    {
        thread t { tricky, this, n };
        // ...
        // ... should I join here? ...
    }

##### Enforcement

???



### <a name="Rconc-join-undetached"></a>CP.28: Remember to join scoped `thread`s that are not `detach()`ed

##### Reason

A `thread` that has not been `detach()`ed when it is destroyed terminates the program.

##### Example, bad

    void f() { std::cout << "Hello "; }

    struct F {
        void operator()() { std::cout << "parallel world "; }
    };

    int main()
    {
        std::thread t1{f};      // f() executes in separate thread
        std::thread t2{F()};    // F()() executes in separate thread
    }  // spot the bugs

##### Example

    void f() { std::cout << "Hello "; }

    struct F {
        void operator()() { std::cout << "parallel world "; }
    };

    int main()
    {
        std::thread t1{f};      // f() executes in separate thread
        std::thread t2{F()};    // F()() executes in separate thread

        t1.join();
        t2.join();
    }  // one bad bug left

??? Is `cout` synchronized?

##### Enforcement

* Flag `join`s for `raii_thread`s ???
* Flag `detach`s for `detached_thread`s


### <a name="RRconc-pass"></a>CP.30: Do not pass pointers to local variables to non-`raii_thread's

##### Reason

In general, you cannot know whether a non-`raii_thread` will outlive the scope of the variables, so that those pointers will become invalid.

##### Example, bad

    void use()
    {
        int x = 7;
        thread t0 { f, ref(x) };
        // ...
        t0.detach();
    }

The `detach` may not be so easy to spot.
Use a `raii_thread` or don't pass the pointer.

##### Example, bad

    ??? put pointer to a local on a queue that is read by a longer-lived thread ???

##### Enforcement

Flag pointers to locals passed in the constructor of a plain `thread`.


### <a name="Rconc-data-by-value"></a>CP.31: Pass small amounts of data between threads by value, rather than by reference or pointer

##### Reason

Copying a small amount of data is cheaper to copy and access than to share it using some locking mechanism.
Copying naturally gives unique ownership (simplifies code) and eliminates the possibility of data races.

##### Note

Defining "small amount" precisely is impossible.

##### Example

    string modify1(string);
    void modify2(shared_ptr<string>);

    void fct(string& s)
    {
        auto res = async(modify1, s);
        async(modify2, &s);
    }

The call of `modify1` involves copying two `string` values; the call of `modify2` does not.
On the other hand, the implementation of `modify1` is exactly as we would have written it for single-threaded code,
whereas the implementation of `modify2` will need some form of locking to avoid data races.
If the string is short (say 10 characters), the call of `modify1` can be surprisingly fast;
essentially all the cost is in the `thread` switch. If the string is long (say 1,000,000 characters), copying it twice
is probably not a good idea.

Note that this argument has nothing to do with `sync` as such. It applies equally to considerations about whether to use
message passing or shared memory.

##### Enforcement

???


### <a name="Rconc-shared"></a>[CP.32: To share ownership between unrelated `thread`s use `shared_ptr`

##### Reason

If threads are unrelated (that is, not known to be in the same scope or one within the lifetime of the other)
and they need to share free store memory that needs to be deleted, a `shared_ptr` (or equivalent) is the only
safe way to ensure proper deletion.

##### Example

    ???

##### Note

* A static object (e.g. a global) can be shared because it is not owned in the sense that some thread is responsible for it's deletion.
* An object on free store that is never to be deleted can be shared.
* An object owned by one thread can be safely shared with another as long as that second thread doesn't outlive the owner.

##### Enforcement

???


### <a name="Rconc-switch"></a>CP.40: Minimize context switching

##### Reason

Context switches are expensive.

##### Example

    ???

##### Enforcement

???


### <a name="Rconc-create"></a>CP.41: Minimize thread creation and destruction

##### Reason

Thread creation is expensive.

##### Example

    void worker(Message m)
    {
        // process
    }

    void master(istream& is)
    {
        for (Message m; is >> m; )
            run_list.push_back(new thread(worker, m));
    }

This spawns a `thread` per message, and the `run_list` is presumably managed to destroy those tasks once they are finished.

Instead, we could have a set of pre-created worker threads processing the messages

    Sync_queue<Message> work;

    void master(istream& is)
    {
        for (Message m; is >> m; )
            work.put(n);
    }

    void worker()
    {
        for (Message m; m = work.get(); ) {
            // process
        }
    }

    void workers()  // set up worker threads (specifically 4 worker threads)
    {
        raii_thread w1 {worker};
        raii_thread w2 {worker};
        raii_thread w3 {worker};
        raii_thread w4 {worker};
    }

##### Note

If your system has a good thread pool, use it.
If your system has a good message queue, use it.

##### Enforcement

???


### <a name="Rconc-wait"></a>CP.42: Don't `wait` without a condition

##### Reason

A `wait` without a condition can miss a wakeup or wake up simply to find that there is no work to do.

##### Example, bad

    std::condition_variable cv;
    std::mutex mx;

    void thread1()
    {
        while (true) {
            // do some work ...
            std::unique_lock<std::mutex> lock(mx);
            cv.notify_one();    // wake other thread
        }
    }

    void thread2()
    {
        while (true) {
            std::unique_lock<std::mutex> lock(mx);
            cv.wait(lock);    // might block forever
            // do work ...
        }
    }

Here, if some other `thread` consumes `thread1`'s notification, `thread2` can wait forever.

##### Example

    template<typename T>
    class Sync_queue {
    public:
        void put(const T& val);
        void put(T&& val);
        void get(T& val);
    private:
        mutex mtx;
        condition_variable cond;    // this controls access
        list<T> q;
    };

    template<typename T>
    void Sync_queue<T>::put(const T& val)
    {
        lock_guard<mutex> lck(mtx);
        q.push_back(val);
        cond.notify_one();
    }

    template<typename T>
    void Sync_queue<T>::get(T& val)
    {
        unique_lock<mutex> lck(mtx);
        cond.wait(lck, [this]{ return !q.empty(); });    // prevent spurious wakeup
        val = q.front();
        q.pop_front();
    }

Now if the queue is empty when a thread executing `get()` wakes up (e.g., because another thread has gotten to `get()` before it),
it will immediately go back to sleep, waiting.

##### Enforcement

Flag all `wait`s without conditions.


### <a name="Rconc-time"></a>CP.43: Minimize time spent in a critical section

##### Reason

The less time is spent with a `mutex` taken, the less chance that another `thread` has to wait,
and `thread` suspension and resumption are expensive.

##### Example

    void do_something() // bad
    {
        unique_lock<mutex> lck(my_lock);
        do0();  // preparation: does not need lock
        do1();  // transaction: needs locking
        do2();  // cleanup: does not need locking
    }

Here, we are holding the lock for longer than necessary:
We should not have taken the lock before we needed it and should have released it again before starting the cleanup.
We could rewrite this to

    void do_something() // bad
    {
        do0();  // preparation: does not need lock
        my_lock.lock();
        do1();  // transaction: needs locking
        my_lock.unlock();
        do2();  // cleanup: does not need locking
    }

But that compromises safety and violates the [use RAII](#Rconc-raii) rule.
Instead, add a block for the critical section:

    void do_something() // OK
    {
        do0();  // preparation: does not need lock
        {
            unique_lock<mutex> lck(my_lock);
            do1();  // transaction: needs locking
        }
        do2();  // cleanup: does not need locking
    }

##### Enforcement

Impossible in general.
Flag "naked" `lock()` and `unlock()`.


### <a name="Rconc-name"></a>CP.44: Remember to name your `lock_guard`s and `unique_lock`s

##### Reason

An unnamed local objects is a temporary that immediately goes out of scope.

##### Example

    unique_lock<mutex>(m1);
    lock_guard<mutex> {m2};
    lock(m1, m2);

This looks innocent enough, but it isn't.

##### Enforcement

Flag all unnamed `lock_guard`s and `unique_lock`s.



### <a name="Rconc-mutex"></a>P.50: Define a `mutex` together with the data it guards

##### Reason

It should be obvious to a reader that the data is to be guarded and how.

##### Example

    struct Record {
        std::mutex m;   // take this mutex before accessing other members
        // ...
    };

##### Enforcement

??? Possible?


## <a name="SScp-par"></a>CP.par: Parallelism

By "parallelism" we refer to performing a task (more or less) simultaneously ("in parallel with") on many data items.

Parallelism rule summary:

* ???
* ???
* Where appropriate, prefer the standard-library parallel algorithms
* Use algorithms that are designed for parallelism, not algorithms with unnecessary dependency on linear evaluation



## <a name="SScp-mess"></a>CP.mess: Message passing

The standard-library facilities are quite low level, focused on the needs of close-to the hardware critical programming using `thread`s, `mutex`es, `atomic` types, etc.
Most people shouldn't work at this level: it's error-prone and development is slow.
If possible, use a higher level facility: messaging libraries, parallel algorithms, and vectorization.
This section looks at passing messages so that a programmer doesn't have to do explicit synchronization.

Message passing rules summary:

* [CP.60: Use a `future` to return a value from a concurrent task](#Rconc-future)
* [CP.61: Use a `async()` to spawn a concurrent task](#Rconc-async)
* message queues
* messaging libraries

???? should there be a "use X rather than `std::async`" where X is something that would use a better specified thread pool?

??? Is `std::async` worth using in light of future (and even existing, as libraries) parallelism facilities? What should the guidelines recommend if someone wants to parallelize, e.g., `std::accumulate` (with the additional precondition of commutativity), or merge sort?


### <a name="Rconc-future"></a>CP.60: Use a `future` to return a value from a concurrent task

##### Reason

A `future` preserves the usual function call return semantics for asynchronous tasks.
The is no explicit locking and both correct (value) return and error (exception) return are handled simply.

##### Example

    ???

##### Note

???

##### Enforcement

???

### <a name="Rconc-async"></a>CP.61: Use a `async()` to spawn a concurrent task

##### Reason

A `future` preserves the usual function call return semantics for asynchronous tasks.
The is no explicit locking and both correct (value) return and error (exception) return are handled simply.

##### Example

    ???

##### Note

Unfortunately, `async()` is not perfect.
For example, there is no guarantee that a thread pool is used to minimize thread construction.
In fact, most current `async()` implementations don't.
However, `async()` is simple and logically correct so until something better comes along
and unless you really need to optimize for many asynchronous tasks, stick with `async()`.

##### Enforcement

???


## <a name="SScp-vec"></a>CP.vec: Vectorization

Vectorization is a technique for executing a number of tasks concurrently without introducing explicit synchronization.
An operation is simply applied to elements of a data structure (a vector, an array, etc.) in parallel.
Vectorization has the interesting property of often requiring no non-local changes to a program.
However, vectorization works best with simple data structures and with algorithms specifically crafted to enable it.

Vectorization rule summary:

* ???
* ???

## <a name="SScp-free"></a>CP.free: Lock-free programming

Synchronization using `mutex`es and `condition_variable`s can be relatively expensive.
Furthermore, it can lead to deadlock.
For performance and to eliminate the possibility of deadlock, we sometimes have to use the tricky low-level "lock-free" facilities
that rely on briefly gaining exclusive ("atomic") access to memory.
Lock free programming is also used to implement higher-level concurrency mechanisms, such as `thread`s and `mutex`es.

Lock-free programming rule summary:

* [CP.100: Don't use lock-free programming unless you absolutely have to](#Rconc-lockfree)
* [CP.101: Distrust your hardware/compiler combination](#Rconc-distrust)
* [CP.102: Carefully study the literature](#Rconc-literature)
* how/when to use atomics
* avoid starvation
* use a lock free data structure rather than hand-crafting specific lock-free access
* [CP.110: Do not write your own double-checked locking for initialization](#Rconc-double)
* [CP.111: Use a conventional pattern if you really need double-checked locking](#Rconc-double-pattern)
* how/when to compare and swap


### <a name="Rconc-lockfree"></a>CP.100: Don't use lock-free programming unless you absolutely have to

##### Reason

It's error-prone and requires expert level knowledge of language features, machine architecture, and data structures.

##### Example, bad

    extern atomic<Link*> head;        // the shared head of a linked list

    Link* nh = new Link(data, nullptr);    // make a link ready for insertion
    Link* h = head.load();                 // read the shared head of the list

    do {
        if (h->data <= data) break;        // if so, insert elsewhere
        nh->next = h;                      // next element is the previous head
    } while (!head.compare_exchange_weak(h, nh));    // write nh to head or to h

Spot the bug.
It would be really hard to find through testing.
Read up on the ABA problem.

##### Exception

[Atomic variables](#???) can be used simply and safely.

##### Note

Higher-level concurrency mechanisms, such as `thread`s and `mutex`es are implemented using lock-free programming.

**Alternative**: Use lock-free data structures implemented by others as part of some library.


### <a name="Rconc-distrust"></a>CP.101: Distrust your hardware/compiler combination

##### Reason

The low-level hardware interfaces used by lock-free programming are among the hardest to implement well and among
the areas where the most subtle portability problems occur.
If you are doing lock-free programming for performance, you need to check for regressions.

##### Note

Instruction reordering (static and dynamic) makes it hard for us to think effectively at this level (especially if you use relaxed memory models).
Experience, (semi)formal models and model checking can be useful.
Testing - often to an extreme extent - is essential.
"Don't fly too close to the wind."

##### Enforcement

Have strong rules for re-testing in place that covers any change in hardware, operating system, compiler, and libraries.


### <a name="Rconc-literature"></a>CP.102: Carefully study the literature

##### Reason

With the exception of atomics and a few use standard patterns, lock-free programming is really an expert-only topic.
Become an expert before shipping lock-free code for others to use.

##### References

* Anthony Williams: C++ concurrency in action. Manning Publications.
* Boehm, Adve, You Don't Know Jack About Shared Variables or Memory Models , Communications of the ACM, Feb 2012.
* Boehm, "Threads Basics", HPL TR 2009-259.
* Adve, Boehm, "Memory Models: A Case for Rethinking Parallel Languages and Hardware", Communications of the ACM, August 2010.
* Boehm, Adve, "Foundations of the C++ Concurrency Memory Model", PLDI 08.
* Mark Batty, Scott Owens, Susmit Sarkar, Peter Sewell, and Tjark Weber, "Mathematizing C++ Concurrency", POPL 2011.
* Damian Dechev, Peter Pirkelbauer, and Bjarne Stroustrup: Understanding and Effectively Preventing the ABA Problem in Descriptor-based Lock-free Designs. 13th IEEE Computer Society ISORC 2010 Symposium. May 2010.
* Damian Dechev and Bjarne Stroustrup: Scalable Non-blocking Concurrent Objects for Mission Critical Code. ACM OOPSLA'09. October 2009
* Damian Dechev, Peter Pirkelbauer, Nicolas Rouquette, and Bjarne Stroustrup: Semantically Enhanced Containers for Concurrent Real-Time Systems. Proc. 16th Annual IEEE International Conference and Workshop on the Engineering of Computer Based Systems (IEEE ECBS). April 2009.


### <a name="Rconc-double"></a>CP.110: Do not write your own double-checked locking for initialization

##### Reason

Since C++11, static local variables are now initialized in a thread-safe way. When combined with the RAII pattern, static local variables can replace the need for writing your own double-checked locking for initialization. std::call_once can also achieve the same purpose. Use either static local variables of C++11 or std::call_once instead of writing your own double-checked locking for initialization.

##### Example

Example with std::call_once.

    void f()
    {
        static std::once_flag my_once_flag;
        std::call_once(my_once_flag, []()
        {
            // do this only once
        });
        // ...
    }

Example with thread-safe static local variables of C++11.

    void f()
    {
        // Assuming the compiler is compliant with C++11
        static My_class my_object; // Constructor called only once
        // ...
    }
    
    class My_class
    {
    public:
        My_class()
        {
            // ...
        }
    };

##### Enforcement

??? Is it possible to detect the idiom?


### <a name="Rconc-double-pattern"></a>CP.111: Use a conventional pattern if you really need double-checked locking

##### Reason

Double-checked locking is easy to mess up. If you really need to write your own double-checked locking, in spite of the rules [CP.110: Do not write your own double-checked locking for initialization](#Rconc-double) and [CP.100: Don't use lock-free programming unless you absolutely have to](#Rconc-lockfree), then do it in a conventional pattern.

##### Example, bad

Even if the following example works correctly on most hardware platforms, it is not guaranteed to work by the C++ standard. The x_init.load(memory_order_relaxed) call may see a value from outside of the lock guard. 

    atomic<bool> x_init;

    if (!x_init.load(memory_order_acquire)) {
        lock_guard<mutex> lck(x_mutex);
        if (!x_init.load(memory_order_relaxed)) {
            // ... initialize x ...
            x_init.store(true, memory_order_release);
        }
    }

##### Example, good

One of the conventional patterns is below.

    std::atomic<int> state;
    
    // If state == SOME_ACTION_NEEDED maybe an action is needed, maybe not, we need to
    // check again in a lock. However, if state != SOME_ACTION_NEEDED, then we can be
    // sure that an action is not needed. This is the basic assumption of double-checked
    // locking.
    
    if (state == SOME_ACTION_NEEDED)
    {
        std::lock_guard<std::mutex> lock(mutex);
        if (state == SOME_ACTION_NEEDED)
        {
            // do something
            state = NO_ACTION_NEEDED;
        }
    }

In the example above (state == SOME_ACTION_NEEDED) could be any condition. It doesn't necessarily needs to be equality comparison. For example, it could as well be (size > MIN_SIZE_TO_TAKE_ACTION).

##### Enforcement

??? Is it possible to detect the idiom?


## <a name="SScp-etc"></a>CP.etc: Etc. concurrency rules

These rules defy simple categorization:

* [CP.200: Use `volatile` only to talk to non-C++ memory](#Rconc-volatile2)
* [CP.201: ??? Signals](#Rconc-signal)

### <a name="Rconc-volatile2"></a>CP.200: Use `volatile` only to talk to non-C++ memory

##### Reason

`volatile` is used to refer to objects that are shared with "non-C++" code or hardware that does not follow the C++ memory model.

##### Example

    const volatile long clock;

This describes a register constantly updated by a clock circuit.
`clock` is `volatile` because its value will change without any action from the C++ program that uses it.
For example, reading `clock` twice will often yield two different values, so the optimizer had better not optimize away the second read in this code:

    long t1 = clock;
    // ... no use of clock here ...
    long t2 = clock;

`clock` is `const` because the program should not try to write to `clock`.

##### Note

Unless you are writing the lowest level code manipulating hardware directly, consider `volatile` an esoteric feature that is best avoided.

##### Example

Usually C++ code receives `volatile` memory that is owned Elsewhere (hardware or another language):

    int volatile* vi = get_hardware_memory_location();
        // note: we get a pointer to someone else's memory here
        // volatile says "treat this with extra respect"

Sometimes C++ code allocates the `volatile` memory and shares it with "elsewhere" (hardware or another language) by deliberately escaping a pointer:

    static volatile long vl;
    please_use_this(&vl);   // escape a reference to this to "elsewhere" (not C++)

##### Example; bad

`volatile` local variables are nearly always wrong -- how can they be shared with other languages or hardware if they're ephemeral?
The same applies almost as strongly to member variables, for the same reason.

    void f() {
        volatile int i = 0; // bad, volatile local variable
        // etc.
    }

    class My_type {
        volatile int i = 0; // suspicious, volatile member variable
        // etc.
    };

##### Note

In C++, unlike in some other languages, `volatile` has [nothing to do with synchronization](#Rconc-volatile).

##### Enforcement

* Flag `volatile T` local and member variables; almost certainly you intended to use `atomic<T>` instead.
* ???

### <a name="Rconc-signal"></a>CP.201: ??? Signals

???UNIX signal handling???. May be worth reminding how little is async-signal-safe, and how to communicate with a signal handler (best is probably "not at all")


# <a name="S-errors"></a>E: Error handling

Error handling involves:

* Detecting an error
* Transmitting information about an error to some handler code
* Preserve the state of a program in a valid state
* Avoid resource leaks

It is not possible to recover from all errors. If recovery from an error is not possible, it is important to quickly "get out" in a well-defined way. A strategy for error handling must be simple, or it becomes a source of even worse errors.  Untested and rarely executed error-handling code is itself the source of many bugs.

The rules are designed to help avoid several kinds of errors:

* Type violations (e.g., misuse of `union`s and casts)
* Resource leaks (including memory leaks)
* Bounds errors
* Lifetime errors (e.g., accessing an object after is has been `delete`d)
* Complexity errors (logical errors make likely by overly complex expression of ideas)
* Interface errors (e.g., an unexpected value is passed through an interface)

Error-handling rule summary:

* [E.1: Develop an error-handling strategy early in a design](#Re-design)
* [E.2: Throw an exception to signal that a function can't perform its assigned task](#Re-throw)
* [E.3: Use exceptions for error handling only](#Re-errors)
* [E.4: Design your error-handling strategy around invariants](#Re-design-invariants)
* [E.5: Let a constructor establish an invariant, and throw if it cannot](#Re-invariant)
* [E.6: Use RAII to prevent leaks](#Re-raii)
* [E.7: State your preconditions](#Re-precondition)
* [E.8: State your postconditions](#Re-postcondition)

* [E.12: Use `noexcept` when exiting a function because of a `throw` is impossible or unacceptable](#Re-noexcept)
* [E.13: Never throw while being the direct owner of an object](#Re-never-throw)
* [E.14: Use purpose-designed user-defined types as exceptions (not built-in types)](#Re-exception-types)
* [E.15: Catch exceptions from a hierarchy by reference](#Re-exception-ref)
* [E.16: Destructors, deallocation, and `swap` must never fail](#Re-never-fail)
* [E.17: Don't try to catch every exception in every function](#Re-not-always)
* [E.18: Minimize the use of explicit `try`/`catch`](#Re-catch)
* [E.19: Use a `final_action` object to express cleanup if no suitable resource handle is available](#Re-finally)

* [E.25: If you can't throw exceptions, simulate RAII for resource management](#Re-no-throw-raii)
* [E.26: If you can't throw exceptions, consider failing fast](#Re-no-throw-crash)
* [E.27: If you can't throw exceptions, use error codes systematically](#Re-no-throw-codes)
* [E.28: Avoid error handling based on global state (e.g. `errno`)](#Re-no-throw)

### <a name="Re-design"></a>E.1: Develop an error-handling strategy early in a design

##### Reason

A consistent and complete strategy for handling errors and resource leaks is hard to retrofit into a system.

### <a name="Re-throw"></a>E.2: Throw an exception to signal that a function can't perform its assigned task

##### Reason

To make error handling systematic, robust, and non-repetitive.

##### Example

    struct Foo {
        vector<Thing> v;
        File_handle f;
        string s;
    };

    void use()
    {
        Foo bar {{ "{{" }}Thing{1}, Thing{2}, Thing{monkey}}, {"my_file", "r"}, "Here we go!"};
        // ...
    }

Here, `vector` and `string`s constructors may not be able to allocate sufficient memory for their elements, `vector`s constructor may not be able copy the `Thing`s in its initializer list, and `File_handle` may not be able to open the required file.
In each case, they throw an exception for `use()`'s caller to handle.
If `use()` could handle the failure to construct `bar` it can take control using `try`/`catch`.
In either case, `Foo`'s constructor correctly destroys constructed members before passing control to whatever tried to create a `Foo`.
Note that there is no return value that could contain an error code.

The `File_handle` constructor might defined like this:

    File_handle::File_handle(const string& name, const string& mode)
        :f{fopen(name.c_str(), mode.c_str())}
    {
        if (!f)
            throw runtime_error{"File_handle: could not open " + name + " as " + mode};
    }

##### Note

It is often said that exceptions are meant to signal exceptional events and failures.
However, that's a bit circular because "what is exceptional?"
Examples:

* A precondition that cannot be met
* A constructor that cannot construct an object (failure to establish its class's [invariant](#Rc-struct))
* An out-of-range error (e.g., `v[v.size()] = 7`)
* Inability to acquire a resource (e.g., the network is down)

In contrast, termination of an ordinary loop is not exceptional.
Unless the loop was meant to be infinite, termination is normal and expected.

##### Note

Don't use a `throw` as simply an alternative way of returning a value from a function.

##### Exception

Some systems, such as hard-real time systems require a guarantee that an action is taken in a (typically short) constant maximum time known before execution starts. Such systems can use exceptions only if there is tool support for accurately predicting the maximum time to recover from a `throw`.

**See also**: [RAII](#Re-raii)

**See also**: [discussion](#Sd-noexcept)

##### Note

Before deciding that you cannot afford or don't like exception-based error handling, have a look at the [alternatives](#Re-no-throw-raii);
they have their own complexities and problems.
Also, as far as possible, measure before making claims about efficiency.

### <a name="Re-errors"></a>E.3: Use exceptions for error handling only

##### Reason

To keep error handling separated from "ordinary code."
C++ implementations tend to be optimized based on the assumption that exceptions are rare.

##### Example, don't

    // don't: exception not used for error handling
    int find_index(vector<string>& vec, const string& x)
    {
        try {
            for (int i = 0; i < vec.size(); ++i)
                if (vec[i] == x) throw i;  // found x
        } catch (int i) {
            return i;
        }
        return -1;   // not found
    }

This is more complicated and most likely runs much slower than the obvious alternative.
There is nothing exceptional about finding a value in a `vector`.

##### Enforcement

Would need to be heuristic.
Look for exception values "leaked" out of `catch` clauses.

### <a name="Re-design-invariants"></a>E.4: Design your error-handling strategy around invariants

##### Reason

To use an object it must be in a valid state (defined formally or informally by an invariant) and to recover from an error every object not destroyed must be in a valid state.

##### Note

An [invariant](#Rc-struct) is logical condition for the members of an object that a constructor must establish for the public member functions to assume.

##### Enforcement

???

### <a name="Re-invariant"></a>E.5: Let a constructor establish an invariant, and throw if it cannot

##### Reason

Leaving an object without its invariant established is asking for trouble.
Not all member functions can be called.

##### Example

    class Vector {  // very simplified vector of doubles
        // if elem != nullptr then elem points to sz doubles
    public:
        Vector() : elem{nullptr}, sz{0}{}
        Vector(int s) : elem{new double}, sz{s} { /* initialize elements */ }
        ~Vector() { delete elem; }
        double& operator[](int s) { return elem[s]; }
        // ...
    private:
        owner<double*> elem;
        int sz;
    };

The class invariant - here stated as a comment - is established by the constructors.
`new` throws if it cannot allocate the required memory.
The operators, notably the subscript operator, relies on the invariant.

**See also**: [If a constructor cannot construct a valid object, throw an exception](#Rc-throw)

##### Enforcement

Flag classes with `private` state without a constructor (public, protected, or private).

### <a name="Re-raii"></a>E.6: Use RAII to prevent leaks

##### Reason

Leaks are typically unacceptable. RAII ("Resource Acquisition Is Initialization") is the simplest, most systematic way of preventing leaks.

##### Example

    void f1(int i)   // Bad: possibly leak
    {
        int* p = new int[12];
        // ...
        if (i < 17) throw Bad {"in f()", i};
        // ...
    }

We could carefully release the resource before the throw:

    void f2(int i)   // Clumsy: explicit release
    {
        int* p = new int[12];
        // ...
        if (i < 17) {
            delete[] p;
            throw Bad {"in f()", i};
        }
        // ...
    }

This is verbose. In larger code with multiple possible `throw`s explicit releases become repetitive and error-prone.

    void f3(int i)   // OK: resource management done by a handle
    {
        auto p = make_unique<int[]>(12);
        // ...
        if (i < 17) throw Bad {"in f()", i};
        // ...
    }

Note that this works even when the `throw` is implicit because it happened in a called function:

    void f4(int i)   // OK: resource management done by a handle
    {
        auto p = make_unique<int[]>(12);
        // ...
        helper(i);   // may throw
        // ...
    }

Unless you really need pointer semantics, use a local resource object:

    void f5(int i)   // OK: resource management done by local object
    {
        vector<int> v(12);
        // ...
        helper(i);   // may throw
        // ...
    }

##### Note

If there is no obvious resource handle, cleanup actions can be represented by a [`final_action` object](#Re-finally)

##### Note

But what do we do if we are writing a program where exceptions cannot be used?
First challenge that assumption; there are many anti-exceptions myths around.
We know of only a few good reasons:

* We are on a system so small that the exception support would eat up most of our 2K memory.
* We are in a hard-real-time system and we don't have tools that guarantee us that an exception is handled within the required time.
* We are in a system with tons of legacy code using lots of pointers in difficult-to-understand ways
  (in particular without a recognizable ownership strategy) so that exceptions could cause leaks.
* Our implementation of the C++ exception mechanisms is unreasonably poor
(slow, memory consuming, failing to work correctly for dynamically linked libraries, etc.).
Complain to your implementation purveyor; if no user complains, no improvement will happen.
* We get fired if we challenge our manager's ancient wisdom.

Only the first of these reasons is fundamental, so whenever possible, use exceptions to implement RAII, or design your RAII objects to never fail.
When exceptions cannot be used, simulate RAII.
That is, systematically check that objects are valid after construction and still release all resources in the destructor.
One strategy is to add a `valid()` operation to every resource handle:

    void f()
    {
        vector<string> vs(100);   // not std::vector: valid() added
        if (!vs.valid()) {
            // handle error or exit
        }

        ifstream fs("foo");   // not std::ifstream: valid() added
        if (!fs.valid()) {
            // handle error or exit
        }

        // ...
    } // destructors clean up as usual

Obviously, this increases the size of the code, doesn't allow for implicit propagation of "exceptions" (`valid()` checks), and `valid()` checks can be forgotten.
Prefer to use exceptions.

**See also**: [Use of `noexcept`](#Se-noexcept).

##### Enforcement

???

### <a name="Re-precondition"></a>E.7: State your preconditions

##### Reason

To avoid interface errors.

**See also**: [precondition rule](#Ri-pre).

### <a name="Re-postcondition"></a>E.8: State your postconditions

##### Reason

To avoid interface errors.

**See also**: [postcondition rule](#Ri-post).

### <a name="Re-noexcept"></a>E.12: Use `noexcept` when exiting a function because of a `throw` is impossible or unacceptable

##### Reason

To make error handling systematic, robust, and efficient.

##### Example

    double compute(double d) noexcept
    {
        return log(sqrt(d <= 0 ? 1 : d));
    }

Here, we know that `compute` will not throw because it is composed out of operations that don't throw.
By declaring `compute` to be `noexcept`, we give the compiler and human readers information that can make it easier for them to understand and manipulate `compute`.

##### Note

Many standard library functions are `noexcept` including all the standard library functions "inherited" from the C standard library.

##### Example

    vector<double> munge(const vector<double>& v) noexcept
    {
        vector<double> v2(v.size());
        // ... do something ...
    }

The `noexcept` here states that I am not willing or able to handle the situation where I cannot construct the local `vector`. That is, I consider memory exhaustion a serious design error (on par with hardware failures) so that I'm willing to crash the program if it happens.

**See also**: [discussion](#Sd-noexcept).

### <a name="Re-never-throw"></a>E.13: Never throw while being the direct owner of an object

##### Reason

That would be a leak.

##### Example

    void leak(int x)   // don't: may leak
    {
        auto p = new int{7};
        if (x < 0) throw Get_me_out_of_here{};  // may leak *p
        // ...
        delete p;   // we may never get here
    }

One way of avoiding such problems is to use resource handles consistently:

    void no_leak(int x)
    {
        auto p = make_unique<int>(7);
        if (x < 0) throw Get_me_out_of_here{};  // will delete *p if necessary
        // ...
        // no need for delete p
    }

Another solution (often better) would be to use a local variable to eliminate explicit use of pointers:

    void no_leak_simplified(int x)
    {
        vector<int> v(7);
        // ...
    }

**See also**: ???resource rule ???

### <a name="Re-exception-types"></a>E.14: Use purpose-designed user-defined types as exceptions (not built-in types)

##### Reason

A user-defined type is unlikely to clash with other people's exceptions.

##### Example

    void my_code()
    {
        // ...
        throw Moonphase_error{};
        // ...
    }

    void your_code()
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(Bufferpool_exhausted) {
            // ...
        }
    }

##### Example, don't

    void my_code()     // Don't
    {
        // ...
        throw 7;       // 7 means "moon in the 4th quarter"
        // ...
    }

    void your_code()   // Don't
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(int i) {  // i == 7 means "input buffer too small"
            // ...
        }
    }

##### Note

The standard-library classes derived from `exception` should be used only as base classes or for exceptions that require only "generic" handling. Like built-in types, their use could clash with other people's use of them.

##### Example, don't

    void my_code()   // Don't
    {
        // ...
        throw runtime_error{"moon in the 4th quarter"};
        // ...
    }

    void your_code()   // Don't
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(runtime_error) {   // runtime_error means "input buffer too small"
            // ...
        }
    }

**See also**: [Discussion](#Sd-???)

##### Enforcement

Catch `throw` and `catch` of a built-in type. Maybe warn about `throw` and `catch` using an standard-library `exception` type. Obviously, exceptions derived from the `std::exception` hierarchy is fine.

### <a name="Re-exception-ref"></a>E.15: Catch exceptions from a hierarchy by reference

##### Reason

To prevent slicing.

##### Example

    void f()
    try {
        // ...
    }
    catch (exception e) {   // don't: may slice
        // ...
    }

Instead, use:

    catch (exception& e) { /* ... */ }

##### Enforcement

Flag by-value exceptions if their types are part of a hierarchy (could require whole-program analysis to be perfect).

### <a name="Re-never-fail"></a>E.16: Destructors, deallocation, and `swap` must never fail

##### Reason

We don't know how to write reliable programs if a destructor, a swap, or a memory deallocation fails; that is, if it exits by an exception or simply doesn't perform its required action.

##### Example, don't

    class Connection {
        // ...
    public:
        ~Connection()   // Don't: very bad destructor
        {
            if (cannot_disconnect()) throw I_give_up{information};
            // ...
        }
    };

##### Note

Many have tried to write reliable code violating this rule for examples, such as a network connection that "refuses to close".
To the best of our knowledge nobody has found a general way of doing this.
Occasionally, for very specific examples, you can get away with setting some state for future cleanup.
For example, we might put a socket that does not want to close on a "bad socket" list,
to be examined by a regular sweep of the system state.
Every example we have seen of this is error-prone, specialized, and often buggy.

##### Note

The standard library assumes that destructors, deallocation functions (e.g., `operator delete`), and `swap` do not throw. If they do, basic standard library invariants are broken.

##### Note

Deallocation functions, including `operator delete`, must be `noexcept`. `swap` functions must be `noexcept`.
Most destructors are implicitly `noexcept` by default.
Also, [make move operations `noexcept`](##Rc-move-noexcept).

##### Enforcement

Catch destructors, deallocation operations, and `swap`s that `throw`.
Catch such operations that are not `noexcept`.

**See also**: [discussion](#Sd-never-fail)

### <a name="Re-not-always"></a>E.17: Don't try to catch every exception in every function

##### Reason

Catching an exception in a function that cannot take a meaningful recovery action leads to complexity and waste.
Let an exception propagate until it reaches a function that can handle it.
Let cleanup actions on the unwinding path be handled by [RAII](#Re-raii).

##### Example, don't

    void f()   // bad
    {
        try {
            // ...
        }
        catch (...) {
            // no action
            throw;   // propagate exception
        }
    }

##### Enforcement

* Flag nested try-blocks.
* Flag source code files with a too high ratio of try-blocks to functions. (??? Problem: define "too high")

### <a name="Re-catch"></a>E.18: Minimize the use of explicit `try`/`catch`

##### Reason

 `try`/`catch` is verbose and non-trivial uses error-prone.
 `try`/`catch` can be a sign of unsystematic and/or low-level resource management or error handling.

##### Example, Bad

    void f(zstring s)
    {
        Gadget* p;
        try {
            p = new Gadget(s);
            // ...
            delete p;
        }
        catch (Gadget_construction_failure) {
            delete p;
            throw;
        }
    }

This code is messy.
There could be a leak from the naked pointer in the `try` block.
Not all exceptions are handled.
`deleting` an object that failed to construct is almost certainly a mistake.
Better:

    void f2(zstring s)
    {
        Gadget g {s};
    }

##### Alternatives

* proper resource handles and [RAII](#Re-raii)
* [`finally`](#Re-finally)

##### Enforcement

??? hard, needs a heuristic

### <a name="Re-finally"></a>E.19: Use a `final_action` object to express cleanup if no suitable resource handle is available

##### Reason

`finally` is less verbose and harder to get wrong than `try`/`catch`.

##### Example

    void f(int n)
    {
        void* p = malloc(1, n);
        auto _ = finally([p] { free(p); });
        // ...
    }

##### Note

`finally` is not as messy as `try`/`catch`, but it is still ad-hoc.
Prefer [proper resource management objects](#Re-raii).

##### Note

Use of `finally` is a systematic and reasonably clean alternative to the old [`goto exit;` technique](##Re-no-throw-codes)
for dealing with cleanup where resource management is not systematic.

##### Enforcement

Heuristic: Detect `goto exit;`

### <a name="Re-no-throw-raii"></a>E.25: If you can't throw exceptions, simulate RAII for resource management

##### Reason

Even without exceptions, [RAII](#Re-raii) is usually the best and most systematic way of dealing with resources.

##### Note

Error handling using exceptions is the only complete and systematic way of handling non-local errors in C++.
In particular, non-intrusively signaling failure to construct an object requires an exception.
Signaling errors in a way that cannot be ignored requires exceptions.
If you can't use exceptions, simulate their use as best you can.

A lot of fear of exceptions is misguided.
When used for exceptional circumstances in code that is not littered with pointers and complicated control structures,
exception handling is almost always affordable (in time and space) and almost always leads to better code.
This, of course, assumes a good implementation of the exception handling mechanisms, which is not available on all systems.
There are also cases where the problems above do not apply, but exceptions cannot be used for other reasons.
Some hard real-time systems are an example: An operation has to be completed within a fixed time with an error or a correct answer.
In the absence of appropriate time estimation tools, this is hard to guarantee for exceptions.
Such systems (e.g. flight control software) typically also ban the use of dynamic (heap) memory.

So, the primary guideline for error handling is "use exceptions and [RAII](#Re-raii)."
This section deals with the cases where you either do not have an efficient implementation or exceptions
or have such a rat's nest of old-style code
(e.g., lots of pointers, ill-defined ownership, and lots of unsystematic error handling based on tests of errors codes)
that it is infeasible to introduce simple and systematic exception handling.

Before condemning exceptions or complaining too much about their cost, consider examples of the use of [error codes](#Re-no-throw-codes).
Consider the cost and complexity of the use of error codes.
If performance is your worry, measure.

##### Example

Assume you wanted to write

    void func(zstring arg)
    {
        Gadget g {arg};
        // ...
    }

If the `gadget` isn't correctly constructed, `func` exits with an exception.
If we cannot throw an exception, we can simulate this RAII style of resource handling by adding a `valid()` member function to `Gadget`:

    error_indicator func(zstring arg)
    {
        Gadget g {arg};
        if (!g.valid()) return gadget_construction_error;
        // ...
        return 0;   // zero indicates "good"
    }

The problem is of course that the caller now have to remember to test the return value.

**See also**: [Discussion](#Sd-???).

##### Enforcement

Possible (only) for specific versions of this idea: e.g., test for systematic test of `valid()` after resource handle construction

### <a name="Re-no-throw-crash"></a>E.26: If you can't throw exceptions, consider failing fast

##### Reason

If you can't do a good job at recovering, at least you can get out before too much consequential damage is done.

See also [Simulating RAII](#Re-no-throw-raii).

##### Note

If you cannot be systematic about error handling, consider "crashing" as a response to any error that cannot be handled locally.
That is, if you cannot recover from an error in the context of the function that detected it, call `abort()`, `quick_exit()`,
or a similar function that will trigger some sort of system restart.

In systems where you have lots of processes and/or lots of computers, you need to expect and handle fatal crashes anyway,
say from hardware failures.
In such cases, "crashing" is simply leaving error handling to the next level of the system.

##### Example

    void f(int n)
    {
        // ...
        p = static_cast<X*>(malloc(n, X));
        if (p == nullptr) abort();     // abort if memory is exhausted
        // ...
    }

Most programs cannot handle memory exhaustion gracefully anyway. This is roughly equivalent to

    void f(int n)
    {
        // ...
        p = new X[n];    // throw if memory is exhausted (by default, terminate)
        // ...
    }

Typically, it is a good idea to log the reason for the "crash" before exiting.

##### Enforcement

Awkward

### <a name="Re-no-throw-codes"></a>E.27: If you can't throw exceptions, use error codes systematically

##### Reason

Systematic use of any error-handling strategy minimizes the chance of forgetting to handle an error.

See also [Simulating RAII](#Re-no-throw-raii).

##### Note

There are several issues to be addressed:

* how do you transmit an error indicator from out of a function?
* how do you release all resources from a function before doing an error exit?
* What do you use as an error indicator?

In general, returning an error indicator implies returning two values: The result and an error indicator.
The error indicator can be part of the object, e.g. an object can have a `valid()` indicator
or a pair of values can be returned.

##### Example

    Gadget make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        Gadget g = make_gadget(17);
        if (!g.valid()) {
                // error handling
        }
        // ...
    }

This approach fits with [simulated RAII resource management](#Re-no-throw-raii).
The `valid()` function could return an `error_indicator` (e.g. a member of an `error_indicator` enumeration).

##### Example

What if we cannot or do not want to modify the `Gadget` type?
In that case, we must return a pair of values.
For example:

    std::pair<Gadget, error_indicator> make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        auto r = make_gadget(17);
        if (!r.second) {
                // error handling
        }
        Gadget& g = r.first;
        // ...
    }

As shown, `std::pair` is a possible return type.
Some people prefer a specific type.
For example:

    Gval make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        auto r = make_gadget(17);
        if (!r.err) {
                // error handling
        }
        Gadget& g = r.val;
        // ...
    }

One reason to prefer a specific return type is to have names for its members, rather than the somewhat cryptic `first` and `second`
and to avoid confusion with other uses of `std::pair`.

##### Example

In general, you must clean up before an error exit.
This can be messy:

    std::pair<int, error_indicator> user()
    {
        Gadget g1 = make_gadget(17);
        if (!g1.valid()) {
                return {0, g1_error};
        }

        Gadget g2 = make_gadget(17);
        if (!g2.valid()) {
                cleanup(g1);
                return {0, g2_error};
        }

        // ...

        if (all_foobar(g1, g2)) {
            cleanup(g1);
            cleanup(g2);
            return {0, foobar_error};
        // ...

        cleanup(g1);
        cleanup(g2);
        return {res, 0};
    }

Simulating RAII can be non-trivial, especially in functions with multiple resources and multiple possible errors.
A not uncommon technique is to gather cleanup at the end of the function to avoid repetition:

    std::pair<int, error_indicator> user()
    {
        error_indicator err = 0;

        Gadget g1 = make_gadget(17);
        if (!g1.valid()) {
                err = g1_error;
                goto exit;
        }

        Gadget g2 = make_gadget(17);
        if (!g2.valid()) {
                err = g2_error;
                goto exit;
        }

        if (all_foobar(g1, g2)) {
            err = foobar_error;
            goto exit;
        }
        // ...

    exit:
      if (g1.valid()) cleanup(g1);
      if (g2.valid()) cleanup(g2);
      return {res, err};
    }

The larger the function, the more tempting this technique becomes.
`finally` can [ease the pain a bit](#Re-finally).
Also, the larger the program becomes the harder it is to apply an error-indicator-based error handling strategy systematically.

We [prefer exception-based error handling](#Re-throw) and recommend [keeping functions short](#Rf-single).

**See also**: [Discussion](#Sd-???).

**See also**: [Returning multiple values](#Rf-out-multi).

##### Enforcement

Awkward.

### <a name="Re-no-throw"></a>E.28: Avoid error handling based on global state (e.g. `errno`)

##### Reason

Global state is hard to manage and it is easy to forget to check it.
When did you last test the return value of `printf()`?

See also [Simulating RAII](#Re-no-throw-raii).

##### Example, bad

    ???

##### Note

C-style error handling is based on the global variable `errno`, so it is essentially impossible to avoid this style completely.

##### Enforcement

Awkward.

# <a name="S-const"></a>Con: Constants and Immutability

You can't have a race condition on a constant.
It is easier to reason about a program when many of the objects cannot change their values.
Interfaces that promises "no change" of objects passed as arguments greatly increase readability.

Constant rule summary:

* [Con.1: By default, make objects immutable](#Rconst-immutable)
* [Con.2: By default, make member functions `const`](#Rconst-fct)
* [Con.3: By default, pass pointers and references to `const`s](#Rconst-ref)
* [Con.4: Use `const` to define objects with values that do not change after construction](#Rconst-const)
* [Con.5: Use `constexpr` for values that can be computed at compile time](#Rconst-constexpr)

### <a name="Rconst-immutable"></a>Con.1: By default, make objects immutable

##### Reason

Immutable objects are easier to reason about, so make object non-`const` only when there is a need to change their value.
Prevents accidental or hard-to-notice change of value.

##### Example

    for (const string& s : c) cout << s << '\n';    // just reading: const

    for (string& s : c) cout << s << '\n';    // BAD: just reading

    for (string& s : c) cin >> s;  // needs to write: non-const

##### Exception

Function arguments are rarely mutated, but also rarely declared const.
To avoid confusion and lots of false positives, don't enforce this rule for function arguments.

    void f(const char* const p); // pedantic
    void g(const int i);        // pedantic

Note that function parameter is a local variable so changes to it are local.

##### Enforcement

* Flag non-const variables that are not modified (except for parameters to avoid many false positives)

### <a name="Rconst-fct"></a>Con.2: By default, make member functions `const`

##### Reason

A member function should be marked `const` unless it changes the object's observable state.
This gives a more precise statement of design intent, better readability, more errors caught by the compiler, and sometimes more optimization opportunities.

##### Example; bad

    class Point {
        int x, y;
    public:
        int getx() { return x; }    // BAD, should be const as it doesn't modify the object's state
        // ...
    };

    void f(const Point& pt) {
        int x = pt.getx();          // ERROR, doesn't compile because getx was not marked const
    }

##### Note

[Do not cast away `const`](#Res-casts-const).

##### Enforcement

* Flag a member function that is not marked `const`, but that does not perform a non-`const` operation on any member variable.

### <a name="Rconst-ref"></a>Con.3: By default, pass pointers and references to `const`s

##### Reason

 To avoid a called function unexpectedly changing the value.
 It's far easier to reason about programs when called functions don't modify state.

##### Example

    void f(char* p);        // does f modify *p? (assume it does)
    void g(const char* p);  // g does not modify *p

##### Note

It is not inherently bad to pass a pointer or reference to non-const,
but that should be done only when the called function is supposed to modify the object.

##### Note

[Do not cast away `const`](#Res-casts-const).

##### Enforcement

* Flag function that does not modify an object passed by  pointer or reference to non-`const`
* Flag a function that (using a cast) modifies an object passed by pointer or reference to `const`

### <a name="Rconst-const"></a>Con.4: Use `const` to define objects with values that do not change after construction

##### Reason

 Prevent surprises from unexpectedly changed object values.

##### Example

    void f()
    {
        int x = 7;
        const int y = 9;

        for (;;) {
            // ...
        }
        // ...
    }

As `x` is not `const`, we must assume that it is modified somewhere in the loop.

##### Enforcement

* Flag unmodified non-`const` variables.

### <a name="Rconst-constexpr"></a>Con.5: Use `constexpr` for values that can be computed at compile time

##### Reason

Better performance, better compile-time checking, guaranteed compile-time evaluation, no possibility of race conditions.

##### Example

    double x = f(2);            // possible run-time evaluation
    const double y = f(2);      // possible run-time evaluation
    constexpr double z = f(2);  // error unless f(2) can be evaluated at compile time

##### Note

See F.4.

##### Enforcement

* Flag `const` definitions with constant expression initializers.

# <a name="S-templates"></a>T: Templates and generic programming

Generic programming is programming using types and algorithms parameterized by types, values, and algorithms.
In C++, generic programming is supported by the `template` language mechanisms.

Arguments to generic functions are characterized by sets of requirements on the argument types and values involved.
In C++, these requirements are expressed by compile-time predicates called concepts.

Templates can also be used for meta-programming; that is, programs that compose code at compile time.

A central notion in generic programming is "concepts"; that is, requirements on template arguments presented as compile-time predicates.
"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Currently (July 2016), concepts are supported only in GCC 6.1.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1, you can uncomment them.

Template use rule summary:

* [T.1: Use templates to raise the level of abstraction of code](#Rt-raise)
* [T.2: Use templates to express algorithms that apply to many argument types](#Rt-algo)
* [T.3: Use templates to express containers and ranges](#Rt-cont)
* [T.4: Use templates to express syntax tree manipulation](#Rt-expr)
* [T.5: Combine generic and OO techniques to amplify their strengths, not their costs](#Rt-generic-oo)

Concept use rule summary:

* [T.10: Specify concepts for all template arguments](#Rt-concepts)
* [T.11: Whenever possible use standard concepts](#Rt-std-concepts)
* [T.12: Prefer concept names over `auto` for local variables](#Rt-auto)
* [T.13: Prefer the shorthand notation for simple, single-type argument concepts](#Rt-shorthand)
* ???

Concept definition rule summary:

* [T.20: Avoid "concepts" without meaningful semantics](#Rt-low)
* [T.21: Require a complete set of operations for a concept](#Rt-complete)
* [T.22: Specify axioms for concepts](#Rt-axiom)
* [T.23: Differentiate a refined concept from its more general case by adding new use patterns](#Rt-refine)
* [T.24: Use tag classes or traits to differentiate concepts that differ only in semantics](#Rt-tag)
* [T.25: Avoid complementary constraints](#Rt-not)
* [T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax](#Rt-use)
* [T.30: Use concept negation (`!C<T>`) sparingly to express a minor difference](#Rt-not)
* [T.31: Use concept disjunction (`C1<T> || C2<T>`) sparingly to express alternatives](#Rt-or)
* ???

Template interface rule summary:

* [T.40: Use function objects to pass operations to algorithms](#Rt-fo)
* [T.41: Require only essential properties in a template's concepts](#Rt-essential)
* [T.42: Use template aliases to simplify notation and hide implementation details](#Rt-alias)
* [T.43: Prefer `using` over `typedef` for defining aliases](#Rt-using)
* [T.44: Use function templates to deduce class template argument types (where feasible)](#Rt-deduce)
* [T.46: Require template arguments to be at least `Regular` or `SemiRegular`](#Rt-regular)
* [T.47: Avoid highly visible unconstrained templates with common names](#Rt-visible)
* [T.48: If your compiler does not support concepts, fake them with `enable_if`](#Rt-concept-def)
* [T.49: Where possible, avoid type-erasure](#Rt-erasure)

Template definition rule summary:

* [T.60: Minimize a template's context dependencies](#Rt-depend)
* [T.61: Do not over-parameterize members (SCARY)](#Rt-scary)
* [T.62: Place non-dependent class template members in a non-templated base class](#Rt-nondependent)
* [T.64: Use specialization to provide alternative implementations of class templates](#Rt-specialization)
* [T.65: Use tag dispatch to provide alternative implementations of functions](#Rt-tag-dispatch)
* [T.67: Use specialization to provide alternative implementations for irregular types](#Rt-specialization2)
* [T.68: Use `{}` rather than `()` within templates to avoid ambiguities](#Rt-cast)
* [T.69: Inside a template, don't make an unqualified nonmember function call unless you intend it to be a customization point](#Rt-customization)

Template and hierarchy rule summary:

* [T.80: Do not naively templatize a class hierarchy](#Rt-hier)
* [T.81: Do not mix hierarchies and arrays](#Rt-array) // ??? somewhere in "hierarchies"
* [T.82: Linearize a hierarchy when virtual functions are undesirable](#Rt-linear)
* [T.83: Do not declare a member function template virtual](#Rt-virtual)
* [T.84: Use a non-template core implementation to provide an ABI-stable interface](#Rt-abi)
* [T.??: ????](#Rt-???)

Variadic template rule summary:

* [T.100: Use variadic templates when you need a function that takes a variable number of arguments of a variety of types](#Rt-variadic)
* [T.101: ??? How to pass arguments to a variadic template ???](#Rt-variadic-pass)
* [T.102: ??? How to process arguments to a variadic template ???](#Rt-variadic-process)
* [T.103: Don't use variadic templates for homogeneous argument lists](#Rt-variadic-not)
* [T.??: ????](#Rt-???)

Metaprogramming rule summary:

* [T.120: Use template metaprogramming only when you really need to](#Rt-metameta)
* [T.121: Use template metaprogramming primarily to emulate concepts](#Rt-emulate)
* [T.122: Use templates (usually template aliases) to compute types at compile time](#Rt-tmp)
* [T.123: Use `constexpr` functions to compute values at compile time](#Rt-fct)
* [T.124: Prefer to use standard-library TMP facilities](#Rt-std-tmp)
* [T.125: If you need to go beyond the standard-library TMP facilities, use an existing library](#Rt-lib)
* [T.??: ????](#Rt-???)

Other template rules summary:

* [T.140: Name all operations with potential for reuse](#Rt-name)
* [T.141: Use an unnamed lambda if you need a simple function object in one place only](#Rt-lambda)
* [T.142: Use template variables to simplify notation](#Rt-var)
* [T.143: Don't write unintentionally nongeneric code](#Rt-nongeneric)
* [T.144: Don't specialize function templates](#Rt-specialize-function)
* [T.150: Check that a class matches a concept using `static_assert`](#Rt-check-class)
* [T.??: ????](#Rt-???)

## <a name="SS-GP"></a>T.gp: Generic programming

Generic programming is programming using types and algorithms parameterized by types, values, and algorithms.

### <a name="Rt-raise"></a>T.1: Use templates to raise the level of abstraction of code

##### Reason

Generality. Re-use. Efficiency. Encourages consistent definition of user types.

##### Example, bad

Conceptually, the following requirements are wrong because what we want of `T` is more than just the very low-level concepts of "can be incremented" or "can be added":

    template<typename T>
        // requires Incrementable<T>
    T sum1(vector<T>& v, T s)
    {
        for (auto x : v) s += x;
        return s;
    }

    template<typename T>
        // requires Simple_number<T>
    T sum2(vector<T>& v, T s)
    {
        for (auto x : v) s = s + x;
        return s;
    }

Assuming that `Incrementable` does not support `+` and `Simple_number` does not support `+=`, we have overconstrained implementers of `sum1` and `sum2`.
And, in this case, missed an opportunity for a generalization.

##### Example

    template<typename T>
        // requires Arithmetic<T>
    T sum(vector<T>& v, T s)
    {
        for (auto x : v) s += x;
        return s;
    }

Assuming that `Arithmetic` requires both `+` and `+=`, we have constrained the user of `sum` to provide a complete arithmetic type.
That is not a minimal requirement, but it gives the implementer of algorithms much needed freedom and ensures that any `Arithmetic` type
can be used for a wide variety of algorithms.

For additional generality and reusability, we could also use a more general `Container` or `Range` concept instead of committing to only one container, `vector`.

##### Note

If we define a template to require exactly the operations required for a single implementation of a single algorithm
(e.g., requiring just `+=` rather than also `=` and `+`) and only those, we have overconstrained maintainers.
We aim to minimize requirements on template arguments, but the absolutely minimal requirements of an implementation is rarely a meaningful concept.

##### Note

Templates can be used to express essentially everything (they are Turing complete), but the aim of generic programming (as expressed using templates)
is to efficiently generalize operations/algorithms over a set of types with similar semantic properties.

##### Note

The `requires` in the comments are uses of `concepts`.
"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
Currently (July 2016), concepts are supported only in GCC 6.1.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1, you can uncomment them.

##### Enforcement

* Flag algorithms with "overly simple" requirements, such as direct use of specific operators without a concept.
* Do not flag the definition of the "overly simple" concepts themselves; they may simply be building blocks for more useful concepts.

### <a name="Rt-algo"></a>T.2: Use templates to express algorithms that apply to many argument types

##### Reason

Generality. Minimizing the amount of source code. Interoperability. Re-use.

##### Example

That's the foundation of the STL. A single `find` algorithm easily works with any kind of input range:

    template<typename Iter, typename Val>
        // requires Input_iterator<Iter>
        //       && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

Don't use a template unless you have a realistic need for more than one template argument type.
Don't overabstract.

##### Enforcement

??? tough, probably needs a human

### <a name="Rt-cont"></a>T.3: Use templates to express containers and ranges

##### Reason

Containers need an element type, and expressing that as a template argument is general, reusable, and type safe.
It also avoids brittle or inefficient workarounds. Convention: That's the way the STL does it.

##### Example

    template<typename T>
        // requires Regular<T>
    class Vector {
        // ...
        T* elem;   // points to sz Ts
        int sz;
    };

    Vector<double> v(10);
    v[7] = 9.9;

##### Example, bad

    class Container {
        // ...
        void* elem;   // points to size elements of some type
        int sz;
    };

    Container c(10, sizeof(double));
    ((double*) c.elem)[] = 9.9;

This doesn't directly express the intent of the programmer and hides the structure of the program from the type system and optimizer.

Hiding the `void*` behind macros simply obscures the problems and introduces new opportunities for confusion.

**Exceptions**: If you need an ABI-stable interface, you might have to provide a base implementation and express the (type-safe) template in terms of that.
See [Stable base](#Rt-abi).

##### Enforcement

* Flag uses of `void*`s and casts outside low-level implementation code

### <a name="Rt-expr"></a>T.4: Use templates to express syntax tree manipulation

##### Reason

 ???

##### Example

    ???

**Exceptions**: ???

### <a name="Rt-generic-oo"></a>T.5: Combine generic and OO techniques to amplify their strengths, not their costs

##### Reason

Generic and OO techniques are complementary.

##### Example

Static helps dynamic: Use static polymorphism to implement dynamically polymorphic interfaces.

    class Command {
        // pure virtual functions
    };

    // implementations
    template</*...*/>
    class ConcreteCommand : public Command {
        // implement virtuals
    };

##### Example

Dynamic helps static: Offer a generic, comfortable, statically bound interface, but internally dispatch dynamically, so you offer a uniform object layout.
Examples include type erasure as with `std::shared_ptr`'s deleter (but [don't overuse type erasure](#Rt-erasure)).

##### Note

In a class template, nonvirtual functions are only instantiated if they're used -- but virtual functions are instantiated every time.
This can bloat code size, and may overconstrain a generic type by instantiating functionality that is never needed.
Avoid this, even though the standard-library facets made this mistake.

##### See also

* ref ???
* ref ???
* ref ???

##### Enforcement

See the reference to more specific rules.

## <a name="SS-concepts"></a>T.concepts: Concept rules

Concepts is a facility for specifying requirements for template arguments.
It is an [ISO technical specification](#Ref-conceptsTS), but currently supported only by GCC.
Concepts are, however, crucial in the thinking about generic programming and the basis of much work on future C++ libraries
(standard and other).

This section assumes concept support

Concept use rule summary:

* [T.10: Specify concepts for all template arguments](#Rt-concepts)
* [T.11: Whenever possible use standard concepts](#Rt-std-concepts)
* [T.12: Prefer concept names over `auto`](#Rt-auto)
* [T.13: Prefer the shorthand notation for simple, single-type argument concepts](#Rt-shorthand)
* ???

Concept definition rule summary:

* [T.20: Avoid "concepts" without meaningful semantics](#Rt-low)
* [T.21: Require a complete set of operations for a concept](#Rt-complete)
* [T.22: Specify axioms for concepts](#Rt-axiom)
* [T.23: Differentiate a refined concept from its more general case by adding new use patterns](#Rt-refine)
* [T.24: Use tag classes or traits to differentiate concepts that differ only in semantics](#Rt-tag)
* [T.25: Avoid complimentary constraints](#Rt-not)
* [T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax](#Rt-use)
* ???

## <a name="SS-concept-use"></a>T.con-use: Concept use

### <a name="Rt-concepts"></a>T.10: Specify concepts for all template arguments

##### Reason

Correctness and readability.
The assumed meaning (syntax and semantics) of a template argument is fundamental to the interface of a template.
A concept dramatically improves documentation and error handling for the template.
Specifying concepts for template arguments is a powerful design tool.

##### Example

    template<typename Iter, typename Val>
    //    requires Input_iterator<Iter>
    //             && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

or equivalently and more succinctly:

    template<Input_iterator Iter, typename Val>
    //    requires Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Currently (July 2016), concepts are supported only in GCC 6.1.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1, you can uncomment them:

    template<typename Iter, typename Val>
        requires Input_iterator<Iter>
               && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

Plain `typename` (or `auto`) is the least constraining concept.
It should be used only rarely when nothing more than "it's a type" can be assumed.
This is typically only needed when (as part of template metaprogramming code) we manipulate pure expression trees, postponing type checking.

**References**: TC++PL4, Palo Alto TR, Sutton

##### Enforcement

Flag template type arguments without concepts

### <a name="Rt-std-concepts"></a>T.11: Whenever possible use standard concepts

##### Reason

 "Standard" concepts (as provided by the [GSL](#S-GSL) and the [Ranges TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf), and hopefully soon the ISO standard itself)
saves us the work of thinking up our own concepts, are better thought out than we can manage to do in a hurry, and improves interoperability.

##### Note

Unless you are creating a new generic library, most of the concepts you need will already be defined by the standard library.

##### Example (using TS concepts)

    template<typename T>
        // don't define this: Sortable is in the GSL
    concept Ordered_container = Sequence<T> && Random_access<Iterator<T>> && Ordered<Value_type<T>>;

    void sort(Ordered_container& s);

This `Ordered_container` is quite plausible, but it is very similar to the `Sortable` concept in the GSL (and the Range TS).
Is it better? Is it right? Does it accurately reflect the standard's requirements for `sort`?
It is better and simpler just to use `Sortable`:

    void sort(Sortable& s);   // better

##### Note

The set of "standard" concepts is evolving as we approach an ISO standard including concepts.

##### Note

Designing a useful concept is challenging.

##### Enforcement

Hard.

* Look for unconstrained arguments, templates that use "unusual"/non-standard concepts, templates that use "homebrew" concepts without axioms.
* Develop a concept-discovery tool (e.g., see [an early experiment](http://www.stroustrup.com/sle2010_webversion.pdf)).

### <a name="Rt-auto"></a>T.12: Prefer concept names over `auto` for local variables

##### Reason

 `auto` is the weakest concept. Concept names convey more meaning than just `auto`.

##### Example (using TS concepts)

    vector<string> v;
    auto& x = v.front();     // bad
    String& s = v.begin();   // good (String is a GSL concept)

##### Enforcement

* ???

### <a name="Rt-shorthand"></a>T.13: Prefer the shorthand notation for simple, single-type argument concepts

##### Reason

Readability. Direct expression of an idea.

##### Example (using TS concepts)

To say "`T` is `Sortable`":

    template<typename T>       // Correct but verbose: "The parameter is
    //    requires Sortable<T>   // of type T which is the name of a type
    void sort(T&);             // that is Sortable"

    template<Sortable T>       // Better (assuming support for concepts): "The parameter is of type T
    void sort(T&);             // which is Sortable"

    void sort(Sortable&);      // Best (assuming support for concepts): "The parameter is Sortable"

The shorter versions better match the way we speak. Note that many templates don't need to use the `template` keyword.

##### Note

"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Currently (July 2016), concepts are supported only in GCC 6.1.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use a compiler that supports concepts (e.g., GCC 6.1), you can remove the `//`.

##### Enforcement

* Not feasible in the short term when people convert from the `<typename T>` and `<class T`> notation.
* Later, flag declarations that first introduces a typename and then constrains it with a simple, single-type-argument concept.

## <a name="SS-concepts-def"></a>T.concepts.def: Concept definition rules

Defining good concepts is non-trivial.
Concepts are meant to represent fundamental concepts in an application domain (hence the name "concepts").
Similarly throwing together a set of syntactic constraints to be used for a the arguments for a single class or algorithm is not what concepts were designed for
and will not give the full benefits of the mechanism.

Obviously, defining concepts will be most useful for code that can use an implementation (e.g., GCC 6.1),
but defining concepts is in itself a useful design technique and help catch conceptual errors and clean up the concepts (sic!) of an implementation.

### <a name="Rt-low"></a>T.20: Avoid "concepts" without meaningful semantics

##### Reason

Concepts are meant to express semantic notions, such as "a number", "a range" of elements, and "totally ordered."
Simple constraints, such as "has a `+` operator" and "has a `>` operator" cannot be meaningfully specified in isolation
and should be used only as building blocks for meaningful concepts, rather than in user code.

##### Example, bad (using TS concepts)

    template<typename T>
    concept Addable = has_plus<T>;    // bad; insufficient

    template<Addable N> auto algo(const N& a, const N& b) // use two numbers
    {
        // ...
        return a + b;
    }

    int x = 7;
    int y = 9;
    auto z = plus(x, y);   // z = 16

    string xx = "7";
    string yy = "9";
    auto zz = plus(xx, yy);   // zz = "79"

Maybe the concatenation was expected. More likely, it was an accident. Defining minus equivalently would give dramatically different sets of accepted types.
This `Addable` violates the mathematical rule that addition is supposed to be commutative: `a+b == b+a`.

##### Note

The ability to specify a meaningful semantics is a defining characteristic of a true concept, as opposed to a syntactic constraint.

##### Example (using TS concepts)

    template<typename T>
    // The operators +, -, *, and / for a number are assumed to follow the usual mathematical rules
    concept Number = has_plus<T>
                     && has_minus<T>
                     && has_multiply<T>
                     && has_divide<T>;

    template<Number N> auto algo(const N& a, const N& b) // use two numbers
    {
        // ...
        return a + b;
    }

    int x = 7;
    int y = 9;
    auto z = plus(x, y);   // z = 18

    string xx = "7";
    string yy = "9";
    auto zz = plus(xx, yy);   // error: string is not a Number

##### Note

Concepts with multiple operations have far lower chance of accidentally matching a type than a single-operation concept.

##### Enforcement

* Flag single-operation `concepts` when used outside the definition of other `concepts`.
* Flag uses of `enable_if` that appears to simulate single-operation `concepts`.


### <a name="ations"></a>T.21: Require a complete set of operations for a concept

##### Reason

Ease of comprehension.
Improved interoperability.
Helps implementers and maintainers.

##### Note

This is a specific variant of the general rule that [a concept must make semantic sense](#Rt-low).

##### Example, bad (using TS concepts)

    template<typename T> concept Subtractable = requires(T a, T, b) { a-b; };

This makes no semantic sense.
You need at least `+` to make `-` meaningful and useful.

Examples of complete sets are

* `Arithmetic`: `+`, `-`, `*`, `/`, `+=`, `-=`, `*=`, `/=`
* `Comparable`: `<`, `>`, `<=`, `>=`, `==`, `!=`

##### Note

This rule applies whether we use direct language support for concepts or not.
It is a general design rule that even applies to non-templates:

    class Minimal {
        // ...
    };

    bool operator==(const Minimal&, const Minimal&);
    bool operator<(const Minimal&, const Minimal&);

    Minimal operator+(const Minimal&, const Minimal&);
    // no other operators

    void f(const Minimal& x, const Minimal& y)
    {
        if (!(x == y) { /* ... */ }     // OK
        if (x != y) { /* ... */ }       // surprise! error

        while (!(x < y)) { /* ... */ }  // OK
        while (x >= y) { /* ... */ }    // surprise! error

        x = x + y;        // OK
        x += y;             // surprise! error
    }

This is minimal, but surprising and constraining for users.
It could even be less efficient.

The rule supports the view that a concept should reflect a (mathematically) coherent set of operations.

##### Example

    class Convenient {
        // ...
    };

    bool operator==(const Convenient&, const Convenient&);
    bool operator<(const Convenient&, const Convenient&);
    // ... and the other comparison operators ...

    Minimal operator+(const Convenient&, const Convenient&);
    // .. and the other arithmetic operators ...

    void f(const Convenient& x, const Convenient& y)
    {
        if (!(x == y) { /* ... */ }     // OK
        if (x != y) { /* ... */ }       // OK

        while (!(x < y)) { /* ... */ }  // OK
        while (x >= y) { /* ... */ }    // OK

        x = x + y;     // OK
        x += y;      // OK
    }

It can be a nuisance to define all operators, but not hard.
Ideally, that rule should be language supported by giving you comparison operators by default.

##### Enforcement

* Flag classes the support "odd" subsets of a set of operators, e.g., `==` but not `!=` or `+` but not `-`.
  Yes, `std::string` is "odd", but it's too late to change that.


### <a name="Rt-axiom"></a>T.22: Specify axioms for concepts

##### Reason

A meaningful/useful concept has a semantic meaning.
Expressing these semantics in an informal, semi-formal, or formal way makes the concept comprehensible to readers and the effort to express it can catch conceptual errors.
Specifying semantics is a powerful design tool.

##### Example (using TS concepts)

    template<typename T>
        // The operators +, -, *, and / for a number are assumed to follow the usual mathematical rules
        // axiom(T a, T b) { a + b == b + a; a - a == 0; a * (b + c) == a * b + a * c; /*...*/ }
        concept Number = requires(T a, T b) {
            {a + b} -> T;   // the result of a + b is convertible to T
            {a - b} -> T;
            {a * b} -> T;
            {a / b} -> T;
        }

##### Note

This is an axiom in the mathematical sense: something that may be assumed without proof.
In general, axioms are not provable, and when they are the proof is often beyond the capability of a compiler.
An axiom may not be general, but the template writer may assume that it holds for all inputs actually used (similar to a precondition).

##### Note

In this context axioms are Boolean expressions.
See the [Palo Alto TR](#S-references) for examples.
Currently, C++ does not support axioms (even the ISO Concepts TS), so we have to make do with comments for a longish while.
Once language support is available, the `//` in front of the axiom can be removed

##### Note

The GSL concepts have well defined semantics; see the Palo Alto TR and the Ranges TS.

##### Exception (using TS concepts)

Early versions of a new "concept" still under development will often just define simple sets of constraints without a well-specified semantics.
Finding good semantics can take effort and time.
An incomplete set of constraints can still be very useful:

    // balancer for a generic binary tree
    template<typename Node> concept bool Balancer = requires(Node* p) {
        add_fixup(p);
        touch(p);
        detach(p);
    }

So a `Balancer` must supply at least thee operations on a tree `Node`,
but we are not yet ready to specify detailed semantics because a new kind of balanced tree might require more operations
and the precise general semantics for all nodes is hard to pin down in the early stages of design.

A "concept" that is incomplete or without a well-specified semantics can still be useful.
For example, it allows for some checking during initial experimentation.
However, it should not be assumed to be stable.
Each new use case may require such an incomplete concepts to be improved.

##### Enforcement

* Look for the word "axiom" in concept definition comments

### <a name="Rt-refine"></a>T.23: Differentiate a refined concept from its more general case by adding new use patterns.

##### Reason

Otherwise they cannot be distinguished automatically by the compiler.

##### Example (using TS concepts)

    template<typename I>
    concept bool Input_iter = requires(I iter) { ++iter; };

    template<typename I>
    concept bool Fwd_iter = Input_iter<I> && requires(I iter) { iter++; }

The compiler can determine refinement based on the sets of required operations (here, suffix `++`).
This decreases the burden on implementers of these types since
they do not need any special declarations to "hook into the concept".
If two concepts have exactly the same requirements, they are logically equivalent (there is no refinement).

##### Enforcement

* Flag a concept that has exactly the same requirements as another already-seen concept (neither is more refined).
To disambiguate them, see [T.24](#Rt-tag).

### <a name="Rt-tag"></a>T.24: Use tag classes or traits to differentiate concepts that differ only in semantics.

##### Reason

Two concepts requiring the same syntax but having different semantics leads to ambiguity unless the programmer differentiates them.

##### Example (using TS concepts)

    template<typename I>    // iterator providing random access
    concept bool RA_iter = ...;

    template<typename I>    // iterator providing random access to contiguous data
    concept bool Contiguous_iter =
        RA_iter<I> && is_contiguous<I>::value;  // using is_contiguous trait

The programmer (in a library) must define `is_contiguous` (a trait) appropriately.

Wrapping a tag class into a concept leads to a simpler expression of this idea:

    template<typename I> concept Contiguous = is_contiguous<I>::value;

    template<typename I>
    concept bool Contiguous_iter = RA_iter<I> && Contiguous<I>;

The programmer (in a library) must define `is_contiguous` (a trait) appropriately.

##### Note

Traits can be trait classes or type traits.
These can be user-defined or standard-library ones.
Prefer the standard-library ones.

##### Enforcement

* The compiler flags ambiguous use of identical concepts.
* Flag the definition of identical concepts.

### <a name="Rt-not"></a>T.25: Avoid complementary constraints

##### Reason

Clarity. Maintainability.
Functions with complementary requirements expressed using negation are brittle.

##### Example (using TS concepts)

Initially, people will try to define functions with complementary requirements:

    template<typename T>
        requires !C<T>    // bad
    void f();

    template<typename T>
        requires C<T>
    void f();

This is better:

    template<typename T>   // general template
        void f();

    template<typename T>   // specialization by concept
        requires C<T>
    void f();

The compiler will choose the unconstrained template only when `C<T>` is
unsatisfied. If you do not want to (or cannot) define an unconstrained
version of `f()`, then delete it.

    template<typename T>
    void f() = delete;

The compiler will select the overload and emit an appropriate error.

##### Note

Complementary constraints are unfortunately common in `enable_if` code:

    template<typename T>
    enable_if<!C<T>, void>   // bad
    f();

    template<typename T>
    enable_if<C<T>, void>
    f();


##### Note

Complementary requirements on one requirements is sometimes (wrongly) considered manageable.
However, for two or more requirements the number of definitions needs can go up exponentially (2,4,9,16,...):

    C1<T> && C2<T>
    !C1<T> && C2<T>
    C1<T> && !C2<T>
    !C1<T> && !C2<T>

Now the opportunities for errors multiply.

##### Enforcement

* Flag pairs of functions with `C<T>` and `!C<T>` constraints

### <a name="Rt-use"></a>T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax

##### Reason

The definition is more readable and corresponds directly to what a user has to write.
Conversions are taken into account. You don't have to remember the names of all the type traits.

##### Example (using TS concepts)

You might be tempted to define a concept `Equality` like this:

    template<typename T> concept Equality = has_equal<T> && has_not_equal<T>;

Obviously, it would be better and easier just to use the standard `EqualityComparable`,
but - just as an example - if you had to define such a concept, prefer:

    template<typename T> concept Equality = requires(T a, T b) {
        bool == { a == b }
        bool == { a != b }
        // axiom { !(a == b) == (a != b) }
        // axiom { a = b; => a == b }  // => means "implies"
    }

as opposed to defining two meaningless concepts `has_equal` and `has_not_equal` just as helpers in the definition of `Equality`.
By "meaningless" we mean that we cannot specify the semantics of `has_equal` in isolation.

##### Enforcement

???

## <a name="SS-temp-interface"></a>Template interfaces

Over the years, programming with templates have suffered from a weak distinction between the interface of a template
and its implementation.
Before concepts, that distinction had no direct language support.
However, the interface to a template is a critical concept - a contract between a user and an implementer - and should be carefully designed.

### <a name="Rt-fo"></a>T.40: Use function objects to pass operations to algorithms

##### Reason

Function objects can carry more information through an interface than a "plain" pointer to function.
In general, passing function objects gives better performance than passing pointers to functions.

##### Example (using TS concepts)

    bool greater(double x, double y) { return x > y; }
    sort(v, greater);                                    // pointer to function: potentially slow
    sort(v, [](double x, double y) { return x > y; });   // function object
    sort(v, std::greater<>);                             // function object

    bool greater_than_7(double x) { return x > 7; }
    auto x = find_if(v, greater_than_7);                 // pointer to function: inflexible
    auto y = find_if(v, [](double x) { return x > 7; }); // function object: carries the needed data
    auto z = find_if(v, Greater_than<double>(7));        // function object: carries the needed data

You can, of course, generalize those functions using `auto` or (when and where available) concepts. For example:

    auto y1 = find_if(v, [](Ordered x) { return x > 7; }); // require an ordered type
    auto z1 = find_if(v, [](auto x) { return x > 7; });    // hope that the type has a >

##### Note

Lambdas generate function objects.

##### Note

The performance argument depends on compiler and optimizer technology.

##### Enforcement

* Flag pointer to function template arguments.
* Flag pointers to functions passed as arguments to a template (risk of false positives).


### <a name="Rt-essential"></a>T.41: Require only essential properties in a template's concepts

##### Reason

Keep interfaces simple and stable.

##### Example (using TS concepts)

Consider, a `sort` instrumented with (oversimplified) simple debug support:

    void sort(Sortable& s)  // sort sequence s
    {
        if (debug) cerr << "enter sort( " << s <<  ")\n";
        // ...
        if (debug) cerr << "exit sort( " << s <<  ")\n";
    }

Should this be rewritten to:

    template<Sortable S>
        requires Streamable<S>
    void sort(S& s)  // sort sequence s
    {
        if (debug) cerr << "enter sort( " << s <<  ")\n";
        // ...
        if (debug) cerr << "exit sort( " << s <<  ")\n";
    }

After all, there is nothing in `Sortable` that requires `iostream` support.
On the other hand, there is nothing in the fundamental idea of sorting that says anything about debugging.

##### Note

If we require every operation used to be listed among the requirements, the interface becomes unstable:
Every time we change the debug facilities, the usage data gathering, testing support, error reporting, etc.
The definition of the template would need change and every use of the template would have to be recompiled.
This is cumbersome, and in some environments infeasible.

Conversely, if we use an operation in the implementation that is not guaranteed by concept checking,
we may get a late compile-time error.

By not using concept checking for properties of a template argument that is not considered essential,
we delay checking until instantiation time.
We consider this a worthwhile tradeoff.

Note that using non-local, non-dependent names (such as `debug` and `cerr`) also introduces context dependencies that may lead to "mysterious" errors.

##### Note

It can be hard to decide which properties of a type is essential and which are not.

##### Enforcement

???

### <a name="Rt-alias"></a>T.42: Use template aliases to simplify notation and hide implementation details

##### Reason

Improved readability.
Implementation hiding.
Note that template aliases replace many uses of traits to compute a type.
They can also be used to wrap a trait.

##### Example

    template<typename T, size_t N>
    class Matrix {
        // ...
        using Iterator = typename std::vector<T>::iterator;
        // ...
    };

This saves the user of `Matrix` from having to know that its elements are stored in a `vector` and also saves the user from repeatedly typing `typename std::vector<T>::`.

##### Example

    template<typename T>
    void user(T& c)
    {
        // ...
        typename container_traits<T>::value_type x; // bad, verbose
        // ...
    }

    template<typename T>
    using Value_type = typename container_traits<T>::value_type;


This saves the user of `Value_type` from having to know the technique used to implement `value_type`s.

    template<typename T>
    void user2(T& c)
    {
        // ...
        Value_type<T> x;
        // ...
    }

##### Note

A simple, common use could be expressed: "Wrap traits!"

##### Enforcement

* Flag use of `typename` as a disambiguator outside `using` declarations.
* ???

### <a name="Rt-using"></a>T.43: Prefer `using` over `typedef` for defining aliases

##### Reason

Improved readability: With `using`, the new name comes first rather than being embedded somewhere in a declaration.
Generality: `using` can be used for template aliases, whereas `typedef`s can't easily be templates.
Uniformity: `using` is syntactically similar to `auto`.

##### Example

    typedef int (*PFI)(int);   // OK, but convoluted

    using PFI2 = int (*)(int);   // OK, preferred

    template<typename T>
    typedef int (*PFT)(T);      // error

    template<typename T>
    using PFT2 = int (*)(T);   // OK

##### Enforcement

* Flag uses of `typedef`. This will give a lot of "hits" :-(

### <a name="Rt-deduce"></a>T.44: Use function templates to deduce class template argument types (where feasible)

##### Reason

Writing the template argument types explicitly can be tedious and unnecessarily verbose.

##### Example

    tuple<int, string, double> t1 = {1, "Hamlet", 3.14};   // explicit type
    auto t2 = make_tuple(1, "Ophelia"s, 3.14);         // better; deduced type

Note the use of the `s` suffix to ensure that the string is a `std::string`, rather than a C-style string.

##### Note

Since you can trivially write a `make_T` function, so could the compiler. Thus, `make_T` functions may become redundant in the future.

##### Exception

Sometimes there isn't a good way of getting the template arguments deduced and sometimes, you want to specify the arguments explicitly:

    vector<double> v = { 1, 2, 3, 7.9, 15.99 };
    list<Record*> lst;

##### Note

Note that C++17 will make this rule redundant by allowing the template arguments to be deduced directly from constructor arguments:
[Template parameter deduction for constructors (Rev. 3)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0091r1.html).
For example:

    tuple t1 = {1, "Hamlet"s, 3.14}; // deduced: tuple<int, string, double>

##### Enforcement

Flag uses where an explicitly specialized type exactly matches the types of the arguments used.

### <a name="Rt-regular"></a>T.46: Require template arguments to be at least `Regular` or `SemiRegular`

##### Reason

 Readability.
 Preventing surprises and errors.
 Most uses support that anyway.

##### Example

    class X {
            // ...
    public:
        explicit X(int);
        X(const X&);            // copy
        X operator=(const X&);
        X(X&&);                 // move
        X& operator=(X&&);
        ~X();
        // ... no more constructors ...
    };

    X x {1};    // fine
    X y = x;      // fine
    std::vector<X> v(10); // error: no default constructor

##### Note

Semiregular requires default constructible.

##### Enforcement

* Flag types that are not at least `SemiRegular`.

### <a name="Rt-visible"></a>T.47: Avoid highly visible unconstrained templates with common names

##### Reason

 An unconstrained template argument is a perfect match for anything so such a template can be preferred over more specific types that require minor conversions.
 This is particularly annoying/dangerous when ADL is used.
 Common names make this problem more likely.

##### Example

    namespace Bad {
        struct S { int m; };
        template<typename T1, typename T2>
        bool operator==(T1, T2) { cout << "Bad\n"; return true; }
    }

    namespace T0 {
        bool operator==(int, Bad::S) { cout << "T0\n"; return true; }  // compare to int

        void test()
        {
            Bad::S bad{ 1 };
            vector<int> v(10);
            bool b = 1 == bad;
            bool b2 = v.size() == bad;
        }
    }

This prints `T0` and `Bad`.

Now the `==` in `Bad` was designed to cause trouble, but would you have spotted the problem in real code?
The problem is that `v.size()` returns an `unsigned` integer so that a conversion is needed to call the local `==`;
the `==` in `Bad` requires no conversions.
Realistic types, such as the standard library iterators can be made to exhibit similar anti-social tendencies.

##### Note

If an unconstrained template is defined in the same namespace as a type,
that unconstrained template can be found by ADL (as happened in the example).
That is, it is highly visible.

##### Note

This rule should not be necessary, but the committee cannot agree to exclude unconstrained templated from ADL.

Unfortunately this will get many false positives; the standard library violates this widely, by putting many unconstrained templates and types into the single namespace `std`.


##### Enforcement

Flag templates defined in a namespace where concrete types are also defined (maybe not feasible until we have concepts).


### <a name="Rt-concept-def"></a>T.48: If your compiler does not support concepts, fake them with `enable_if`

##### Reason

Because that's the best we can do without direct concept support.
`enable_if` can be used to conditionally define functions and to select among a set of functions.

##### Example

    enable_if<???>

##### Note

Beware of [complementary constraints](# T.25).
Faking concept overloading using `enable_if` sometimes forces us to use that error-prone design technique.

##### Enforcement

???

### <a name="Rt-erasure"></a>T.49: Where possible, avoid type-erasure

##### Reason

Type erasure incurs an extra level of indirection by hiding type information behind a separate compilation boundary.

##### Example

    ???

**Exceptions**: Type erasure is sometimes appropriate, such as for `std::function`.

##### Enforcement

???


##### Note


## <a name="SS-temp-def"></a>T.def: Template definitions

A template definition (class or function) can contain arbitrary code, so only a comprehensive review of C++ programming techniques would cover this topic.
However, this section focuses on what is specific to template implementation.
In particular, it focuses on a template definition's dependence on its context.

### <a name="Rt-depend"></a>T.60: Minimize a template's context dependencies

##### Reason

Eases understanding.
Minimizes errors from unexpected dependencies.
Eases tool creation.

##### Example

    template<typename C>
    void sort(C& c)
    {
        std::sort(begin(c), end(c)); // necessary and useful dependency
    }

    template<typename Iter>
    Iter algo(Iter first, Iter last) {
        for (; first != last; ++first) {
            auto x = sqrt(*first); // potentially surprising dependency: which sqrt()?
            helper(first, x);      // potentially surprising dependency:
                                   // helper is chosen based on first and x
            TT var = 7;            // potentially surprising dependency: which TT?
        }
    }

##### Note

Templates typically appear in header files so their context dependencies are more vulnerable to `#include` order dependencies than functions in `.cpp` files.

##### Note

Having a template operate only on its arguments would be one way of reducing the number of dependencies to a minimum, but that would generally be unmanageable.
For example, an algorithm usually uses other algorithms and invoke operations that does not exclusively operate on arguments.
And don't get us started on macros!
See also [T.69](#Rt-customization)

##### Enforcement

??? Tricky

### <a name="Rt-scary"></a>T.61: Do not over-parameterize members (SCARY)

##### Reason

A member that does not depend on a template parameter cannot be used except for a specific template argument.
This limits use and typically increases code size.

##### Example, bad

    template<typename T, typename A = std::allocator{}>
        // requires Regular<T> && Allocator<A>
    class List {
    public:
        struct Link {   // does not depend on A
            T elem;
            T* pre;
            T* suc;
        };

        using iterator = Link*;

        iterator first() const { return head; }

        // ...
    private:
        Node* head;
    };

    List<int> lst1;
    List<int, My_allocator> lst2;

    ???

This looks innocent enough, but ???

    template<typename T>
    struct Link {
        T elem;
        T* pre;
        T* suc;
    };

    template<typename T, typename A = std::allocator{}>
        // requires Regular<T> && Allocator<A>
    class List2 {
    public:
        using iterator = Link<T>*;

        iterator first() const { return head; }

        // ...
    private:
        Node* head;
    };

    List<int> lst1;
    List<int, My_allocator> lst2;

    ???

##### Enforcement

* Flag member types that do not depend on every template argument
* Flag member functions that do not depend on every template argument

### <a name="Rt-nondependent"></a>T.62: Place non-dependent class template members in a non-templated base class

##### Reason

 Allow the base class members to be used without specifying template arguments and without template instantiation.

##### Example

    template<typename T>
    class Foo {
    public:
        enum { v1, v2 };
        // ...
    };

???

    struct Foo_base {
        enum { v1, v2 };
        // ...
    };

    template<typename T>
    class Foo : public Foo_base {
    public:
        // ...
    };

##### Note

A more general version of this rule would be
"If a template class member depends on only N template parameters out of M, place it in a base class with only N parameters."
For N == 1, we have a choice of a base class of a class in the surrounding scope as in [T.61](#Rt-scary).

??? What about constants? class statics?

##### Enforcement

* Flag ???

### <a name="Rt-specialization"></a>T.64: Use specialization to provide alternative implementations of class templates

##### Reason

A template defines a general interface.
Specialization offers a powerful mechanism for providing alternative implementations of that interface.

##### Example

    ??? string specialization (==)

    ??? representation specialization ?

##### Note

???

##### Enforcement

???

### <a name="Rt-tag-dispatch"></a>T.65: Use tag dispatch to provide alternative implementations of a function

##### Reason

* A template defines a general interface.
* Tag dispatch allows us to select implementations based on specific properties of an argument type.
* Performance.

##### Example

This is a simplified version of `std::copy` (ignoring the possibility of non-contiguous sequences)

    struct pod_tag {};
    struct non_pod_tag {};

    template<class T> struct copy_trait { using tag = non_pod_tag; };   // T is not "plain old data"

    template<> struct copy_trait<int> { using tag = pod_tag; };         // int is "plain old data"

    template<class Iter>
    Out copy_helper(Iter first, Iter last, Iter out, pod_tag)
    {
        // use memmove
    }

    template<class Iter>
    Out copy_helper(Iter first, Iter last, Iter out, non_pod_tag)
    {
        // use loop calling copy constructors
    }

    template<class Itert>
    Out copy(Iter first, Iter last, Iter out)
    {
        return copy_helper(first, last, out, typename copy_trait<Iter>::tag{})
    }

    void use(vector<int>& vi, vector<int>& vi2, vector<string>& vs, vector<string>& vs2)
    {
        copy(vi.begin(), vi.end(), vi2.begin()); // uses memmove
        copy(vs.begin(), vs.end(), vs2.begin()); // uses a loop calling copy constructors
    }

This is a general and powerful technique for compile-time algorithm selection.

##### Note

When `concept`s become widely available such alternatives can be distinguished directly:

    template<class Iter>
        requires Pod<Value_type<iter>>
    Out copy_helper(In, first, In last, Out out)
    {
        // use memmove
    }

    template<class Iter>
    Out copy_helper(In, first, In last, Out out)
    {
        // use loop calling copy constructors
    }

##### Enforcement

???


### <a name="Rt-specialization2"></a>T.67: Use specialization to provide alternative implementations for irregular types

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rt-cast"></a>T.68: Use `{}` rather than `()` within templates to avoid ambiguities

##### Reason

 `()` is vulnerable to grammar ambiguities.

##### Example

    template<typename T, typename U>
    void f(T t, U u)
    {
        T v1(x);    // is v1 a function of a variable?
        T v2 {x};   // variable
        auto x = T(u);  // construction or cast?
    }

    f(1, "asdf"); // bad: cast from const char* to int

##### Enforcement

* flag `()` initializers
* flag function-style casts


### <a name="Rt-customization"></a>T.69: Inside a template, don't make an unqualified nonmember function call unless you intend it to be a customization point

##### Reason

* Provide only intended flexibility.
* Avoid vulnerability to accidental environmental changes.

##### Example

There are three major ways to let calling code customize a template.

    template<class T>
        // Call a member function
    void test1(T t)
    {
        t.f();    // require T to provide f()
    }

    template<class T>
    void test2(T t)
        // Call a nonmember function without qualification
    {
        f(t);  // require f(/*T*/) be available in caller's scope or in T's namespace
    }

    template<class T>
    void test3(T t)
        // Invoke a "trait"
    {
        test_traits<T>::f(t); // require customizing test_traits<>
                              // to get non-default functions/types
    }

A trait is usually a type alias to compute a type,
a `constexpr` function to compute a value,
or a traditional traits template to be specialized on the user's type.

##### Note

If you intend to call your own helper function `helper(t)` with a value `t` that depends on a template type parameter,
put it in a `::detail` namespace and qualify the call as `detail::helper(t);`.
An unqualified call becomes a customization point where any function `helper` in the namespace of `t`'s type can be invoked;
this can cause problems like [unintentionally invoking unconstrained function templates](#Rt-unconstrained-adl).


##### Enforcement

* In a template, flag an unqualified call to a nonmember function that passes a variable of dependent type when there is a nonmember function of the same name in the template's namespace.


## <a name="SS-temp-hier"></a>T.temp-hier: Template and hierarchy rules:

Templates are the backbone of C++'s support for generic programming and class hierarchies the backbone of its support
for object-oriented programming.
The two language mechanisms can be used effectively in combination, but a few design pitfalls must be avoided.

### <a name="Rt-hier"></a>T.80: Do not naively templatize a class hierarchy

##### Reason

Templating a class hierarchy that has many functions, especially many virtual functions, can lead to code bloat.

##### Example, bad

    template<typename T>
    struct Container {         // an interface
        virtual T* get(int i);
        virtual T* first();
        virtual T* next();
        virtual void sort();
    };

    template<typename T>
    class Vector : public Container<T> {
    public:
        // ...
    };

    vector<int> vi;
    vector<string> vs;

It is probably a dumb idea to define a `sort` as a member function of a container, but it is not unheard of and it makes a good example of what not to do.

Given this, the compiler cannot know if `vector<int>::sort()` is called, so it must generate code for it.
Similar for `vector<string>::sort()`.
Unless those two functions are called that's code bloat.
Imagine what this would do to a class hierarchy with dozens of member functions and dozens of derived classes with many instantiations.

##### Note

In many cases you can provide a stable interface by not parameterizing a base;
see ["stable base"](#Rt-abi) and [OO and GP](#Rt-generic-oo)

##### Enforcement

* Flag virtual functions that depend on a template argument. ??? False positives

### <a name="Rt-array"></a>T.81: Do not mix hierarchies and arrays

##### Reason

An array of derived classes can implicitly "decay" to a pointer to a base class with potential disastrous results.

##### Example

Assume that `Apple` and `Pear` are two kinds of `Fruit`s.

    void maul(Fruit* p)
    {
        *p = Pear{};     // put a Pear into *p
        p[1] = Pear{};   // put a Pear into p[2]
    }

    Apple aa [] = { an_apple, another_apple };   // aa contains Apples (obviously!)

    maul(aa);
    Apple& a0 = &aa[0];   // a Pear?
    Apple& a1 = &aa[1];   // a Pear?

Probably, `aa[0]` will be a `Pear` (without the use of a cast!).
If `sizeof(Apple) != sizeof(Pear)` the access to `aa[1]` will not be aligned to the proper start of an object in the array.
We have a type violation and possibly (probably) a memory corruption.
Never write such code.

Note that `maul()` violates the a `T*` points to an individual object [Rule](#???).

**Alternative**: Use a proper (templatized) container:

    void maul2(Fruit* p)
    {
        *p = Pear{};   // put a Pear into *p
    }

    vector<Apple> va = { an_apple, another_apple };   // aa contains Apples (obviously!)

    maul2(aa);       // error: cannot convert a vector<Apple> to a Fruit*
    maul2(&aa[0]);   // you asked for it

    Apple& a0 = &aa[0];   // a Pear?

Note that the assignment in `maul2()` violated the no-slicing [Rule](#???).

##### Enforcement

* Detect this horror!

### <a name="Rt-linear"></a>T.82: Linearize a hierarchy when virtual functions are undesirable

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rt-virtual"></a>T.83: Do not declare a member function template virtual

##### Reason

C++ does not support that.
If it did, vtbls could not be generated until link time.
And in general, implementations must deal with dynamic linking.

##### Example, don't

    class Shape {
        // ...
        template<class T>
        virtual bool intersect(T* p);   // error: template cannot be virtual
    };

##### Note

We need a rule because people keep asking about this

##### Alternative

Double dispatch, visitors, calculate which function to call

##### Enforcement

The compiler handles that.

### <a name="Rt-abi"></a>T.84: Use a non-template core implementation to provide an ABI-stable interface

##### Reason

Improve stability of code.
Avoid code bloat.

##### Example

It could be a base class:

    struct Link_base {   // stable
        Link* suc;
        Link* pre;
    };

    template<typename T>   // templated wrapper to add type safety
    struct Link : Link_base {
        T val;
    };

    struct List_base {
        Link_base* first;   // first element (if any)
        int sz;             // number of elements
        void add_front(Link_base* p);
        // ...
    };

    template<typename T>
    class List : List_base {
    public:
        void put_front(const T& e) { add_front(new Link<T>{e}); }   // implicit cast to Link_base
        T& front() { static_cast<Link<T>*>(first).val; }   // explicit cast back to Link<T>
        // ...
    };

    List<int> li;
    List<string> ls;

Now there is only one copy of the operations linking and unlinking elements of a `List`.
The `Link` and `List` classes does nothing but type manipulation.

Instead of using a separate "base" type, another common technique is to specialize for `void` or `void*` and have the general template for `T` be just the safely-encapsulated casts to and from the core `void` implementation.

**Alternative**: Use a [PIMPL](#???) implementation.

##### Enforcement

???

## <a name="SS-variadic"></a>T.var: Variadic template rules

???

### <a name="Rt-variadic"></a>T.100: Use variadic templates when you need a function that takes a variable number of arguments of a variety of types

##### Reason

Variadic templates is the most general mechanism for that, and is both efficient and type-safe. Don't use C varargs.

##### Example

    ??? printf

##### Enforcement

* Flag uses of `va_arg` in user code.

### <a name="Rt-variadic-pass"></a>T.101: ??? How to pass arguments to a variadic template ???

##### Reason

 ???

##### Example

    ??? beware of move-only and reference arguments

##### Enforcement

???

### <a name="Rt-variadic-process"></a>T.102: How to process arguments to a variadic template

##### Reason

 ???

##### Example

    ??? forwarding, type checking, references

##### Enforcement

???

### <a name="Rt-variadic-not"></a>T.103: Don't use variadic templates for homogeneous argument lists

##### Reason

There are more precise ways of specifying a homogeneous sequence, such as an `initializer_list`.

##### Example

    ???

##### Enforcement

???

## <a name="SS-meta"></a>T.meta: Template metaprogramming (TMP)

Templates provide a general mechanism for compile-time programming.

Metaprogramming is programming where at least one input or one result is a type.
Templates offer Turing-complete (modulo memory capacity) duck typing at compile time.
The syntax and techniques needed are pretty horrendous.

### <a name="Rt-metameta"></a>T.120: Use template metaprogramming only when you really need to

##### Reason

Template metaprogramming is hard to get right, slows down compilation, and is often very hard to maintain.
However, there are real-world examples where template metaprogramming provides better performance that any alternative short of expert-level assembly code.
Also, there are real-world examples where template metaprogramming expresses the fundamental ideas better than run-time code.
For example, if you really need AST manipulation at compile time (e.g., for optional matrix operation folding) there may be no other way in C++.

##### Example, bad

    ???

##### Example, bad

    enable_if

Instead, use concepts. But see [How to emulate concepts if you don't have language support](#Rt-emulate).

##### Example

    ??? good

**Alternative**: If the result is a value, rather than a type, use a [`constexpr` function](#Rt-fct).

##### Note

If you feel the need to hide your template metaprogramming in macros, you have probably gone too far.

### <a name="Rt-emulate"></a>T.121: Use template metaprogramming primarily to emulate concepts

##### Reason

Until concepts become generally available, we need to emulate them using TMP.
Use cases that require concepts (e.g. overloading based on concepts) are among the most common (and simple) uses of TMP.

##### Example

    template<typename Iter>
        /*requires*/ enable_if<random_access_iterator<Iter>, void>
    advance(Iter p, int n) { p += n; }

    template<typename Iter>
        /*requires*/ enable_if<forward_iterator<Iter>, void>
    advance(Iter p, int n) { assert(n >= 0); while (n--) ++p;}

##### Note

Such code is much simpler using concepts:

    void advance(RandomAccessIterator p, int n) { p += n; }

    void advance(ForwardIterator p, int n) { assert(n >= 0); while (n--) ++p;}

##### Enforcement

???

### <a name="Rt-tmp"></a>T.122: Use templates (usually template aliases) to compute types at compile time

##### Reason

Template metaprogramming is the only directly supported and half-way principled way of generating types at compile time.

##### Note

"Traits" techniques are mostly replaced by template aliases to compute types and `constexpr` functions to compute values.

##### Example

    ??? big object / small object optimization

##### Enforcement

???

### <a name="Rt-fct"></a>T.123: Use `constexpr` functions to compute values at compile time

##### Reason

A function is the most obvious and conventional way of expressing the computation of a value.
Often a `constexpr` function implies less compile-time overhead than alternatives.

##### Note

"Traits" techniques are mostly replaced by template aliases to compute types and `constexpr` functions to compute values.

##### Example

    template<typename T>
        // requires Number<T>
    constexpr T pow(T v, int n)   // power/exponential
    {
        T res = 1;
        while (n--) res *= v;
        return res;
    }

    constexpr auto f7 = pow(pi, 7);

##### Enforcement

* Flag template metaprograms yielding a value. These should be replaced with `constexpr` functions.

### <a name="Rt-std-tmp"></a>T.124: Prefer to use standard-library TMP facilities

##### Reason

Facilities defined in the standard, such as `conditional`, `enable_if`, and `tuple`, are portable and can be assumed to be known.

##### Example

    ???

##### Enforcement

???

### <a name="Rt-lib"></a>T.125: If you need to go beyond the standard-library TMP facilities, use an existing library

##### Reason

Getting advanced TMP facilities is not easy and using a library makes you part of a (hopefully supportive) community.
Write your own "advanced TMP support" only if you really have to.

##### Example

    ???

##### Enforcement

???

## <a name="SS-temp-other"></a>Other template rules

### <a name="Rt-name"></a>T.140: Name all operations with potential for reuse

##### Reason

Documentation, readability, opportunity for reuse.

##### Example

    struct Rec {
        string name;
        string addr;
        int id;         // unique identifier
    };

    bool same(const Rec& a, const Rec& b)
    {
        return a.id == b.id;
    }

    vector<Rec*> find_id(const string& name);    // find all records for "name"

    auto x = find_if(vr.begin(), vr.end(),
        [&](Rec& r) {
            if (r.name.size() != n.size()) return false; // name to compare to is in n
            for (int i = 0; i < r.name.size(); ++i)
                if (tolower(r.name[i]) != tolower(n[i])) return false;
            return true;
        }
    );

There is a useful function lurking here (case insensitive string comparison), as there often is when lambda arguments get large.

    bool compare_insensitive(const string& a, const string& b)
    {
        if (a.size() != b.size()) return false;
        for (int i = 0; i < a.size(); ++i) if (tolower(a[i]) != tolower(b[i])) return false;
        return true;
    }

    auto x = find_if(vr.begin(), vr.end(),
        [&](Rec& r) { compare_insensitive(r.name, n); }
    );

Or maybe (if you prefer to avoid the implicit name binding to n):

    auto cmp_to_n = [&n](const string& a) { return compare_insensitive(a, n); };

    auto x = find_if(vr.begin(), vr.end(),
        [](const Rec& r) { return cmp_to_n(r.name); }
    );

##### Note

whether functions, lambdas, or operators.

##### Exception

* Lambdas logically used only locally, such as an argument to `for_each` and similar control flow algorithms.
* Lambdas as [initializers](#???)

##### Enforcement

* (hard) flag similar lambdas
* ???

### <a name="Rt-lambda"></a>T.141: Use an unnamed lambda if you need a simple function object in one place only

##### Reason

That makes the code concise and gives better locality than alternatives.

##### Example

    auto earlyUsersEnd = std::remove_if(users.begin(), users.end(),
                                        [](const User &a) { return a.id > 100; });


##### Exception

Naming a lambda can be useful for clarity even if it is used only once.

##### Enforcement

* Look for identical and near identical lambdas (to be replaced with named functions or named lambdas).

### <a name="Rt-var"></a>T.142?: Use template variables to simplify notation

##### Reason

Improved readability.

##### Example

    ???

##### Enforcement

???

### <a name="Rt-nongeneric"></a>T.143: Don't write unintentionally nongeneric code

##### Reason

Generality. Reusability. Don't gratuitously commit to details; use the most general facilities available.

##### Example

Use `!=` instead of `<` to compare iterators; `!=` works for more objects because it doesn't rely on ordering.

    for (auto i = first; i < last; ++i) {   // less generic
        // ...
    }

    for (auto i = first; i != last; ++i) {   // good; more generic
        // ...
    }

Of course, range-`for` is better still where it does what you want.

##### Example

Use the least-derived class that has the functionality you need.

    class Base {
    public:
        Bar f();
        Bar g();
    };

    class Derived1 : public Base {
    public:
        Bar h();
    };

    class Derived2 : public Base {
    public:
        Bar j();
    };

    // bad, unless there is a specific reason for limiting to Derived1 objects only
    void my_func(Derived1& param)
    {
        use(param.f());
        use(param.g());
    }

    // good, uses only Base interface so only commit to that
    void my_func(Base& param)
    {
        use(param.f());
        use(param.g());
    }

##### Enforcement

* Flag comparison of iterators using `<` instead of `!=`.
* Flag `x.size() == 0` when `x.empty()` or `x.is_empty()` is available. Emptiness works for more containers than size(), because some containers don't know their size or are conceptually of unbounded size.
* Flag functions that take a pointer or reference to a more-derived type but only use functions declared in a base type.

### <a name="Rt-specialize-function"></a>T.144: Don't specialize function templates

##### Reason

You can't partially specialize a function template per language rules. You can fully specialize a function template but you almost certainly want to overload instead -- because function template specializations don't participate in overloading, they don't act as you probably wanted. Rarely, you should actually specialize by delegating to a class template that you can specialize properly.

##### Example

    ???

**Exceptions**: If you do have a valid reason to specialize a function template, just write a single function template that delegates to a class template, then specialize the class template (including the ability to write partial specializations).

##### Enforcement

* Flag all specializations of a function template. Overload instead.


### <a name="Rt-check-class"></a>T.150: Check that a class matches a concept using `static_assert`

##### Reason

If you intend for a class to match a concept, verifying that early saves users pain.

###### Example

    class X {
        X() = delete;
        X(const X&) = default;
        X(X&&) = default;
        X& operator=(const X&) = default;
        // ...
    };

Somewhere, possibly in an implementation file, let the compiler check the desired properties of `X`:

    static_assert(Default_constructible<X>);    // error: X has no default constructor
    static_assert(Copyable<X>);                 // error: we forgot to define X's move constructor


###### Enforcement

Not feasible.

# <a name="S-cpl"></a>CPL: C-style programming

C and C++ are closely related languages.
They both originate in "Classic C" from 1978 and have evolved in ISO committees since then.
Many attempts have been made to keep them compatible, but neither is a subset of the other.

C rule summary:

* [CPL.1: Prefer C++ to C](#Rcpl-C)
* [CPL.2: If you must use C, use the common subset of C and C++, and compile the C code as C++](#Rcpl-subset)
* [CPL.3: If you must use C for interfaces, use C++ in the code using such interfaces](#Rcpl-interface)

### <a name="Rcpl-C"></a>CPL.1: Prefer C++ to C

##### Reason

C++ provides better type checking and more notational support.
It provides better support for high-level programming and often generates faster code.

##### Example

    char ch = 7;
    void* pv = &ch;
    int* pi = pv;   // not C++
    *pi = 999;      // overwrite sizeof(int) bytes near &ch

The rules for implicit casting to and from `void*` in C are subtle and unenforced.
In particular, this example violates a rule against converting to a type with stricter alignment.

##### Enforcement

Use a C++ compiler.

### <a name="Rcpl-subset"></a>CPL.2: If you must use C, use the common subset of C and C++, and compile the C code as C++

##### Reason

That subset can be compiled with both C and C++ compilers, and when compiled as C++ is better type checked than "pure C."

##### Example

    int* p1 = malloc(10 * sizeof(int));                      // not C++
    int* p2 = static_cast<int*>(malloc(10 * sizeof(int)));   // not C, C-style C++
    int* p3 = new int[10];                                   // not C
    int* p4 = (int*) malloc(10 * sizeof(int));               // both C and C++

##### Enforcement

* Flag if using a build mode that compiles code as C.

  * The C++ compiler will enforce that the code is valid C++ unless you use C extension options.

### <a name="Rcpl-interface"></a>CPL.3: If you must use C for interfaces, use C++ in the calling code using such interfaces

##### Reason

C++ is more expressive than C and offers better support for many types of programming.

##### Example

For example, to use a 3rd party C library or C systems interface, define the low-level interface in the common subset of C and C++ for better type checking.
Whenever possible encapsulate the low-level interface in an interface that follows the C++ guidelines (for better abstraction, memory safety, and resource safety) and use that C++ interface in C++ code.

##### Example

You can call C from C++:

    // in C:
    double sqrt(double);

    // in C++:
    extern "C" double sqrt(double);

    sqrt(2);

##### Example

You can call C++ from C:

    // in C:
    X call_f(struct Y*, int);

    // in C++:
    extern "C" X call_f(Y* p, int i)
    {
        return p->f(i);   // possibly a virtual function call
    }

##### Enforcement

None needed

# <a name="S-source"></a>SF: Source files

Distinguish between declarations (used as interfaces) and definitions (used as implementations).
Use header files to represent interfaces and to emphasize logical structure.

Source file rule summary:

* [SF.1: Use a `.cpp` suffix for code files and `.h` for interface files if your project doesn't already follow another convention](#Rs-file-suffix)
* [SF.2: A `.h` file may not contain object definitions or non-inline function definitions](#Rs-inline)
* [SF.3: Use `.h` files for all declarations used in multiple source files](#Rs-declaration-header)
* [SF.4: Include `.h` files before other declarations in a file](#Rs-include-order)
* [SF.5: A `.cpp` file must include the `.h` file(s) that defines its interface](#Rs-consistency)
* [SF.6: Use `using`-directives for transition, for foundation libraries (such as `std`), or within a local scope](#Rs-using)
* [SF.7: Don't put a `using`-directive in a header file](#Rs-using-directive)
* [SF.8: Use `#include` guards for all `.h` files](#Rs-guards)
* [SF.9: Avoid cyclic dependencies among source files](#Rs-cycles)

* [SF.20: Use `namespace`s to express logical structure](#Rs-namespace)
* [SF.21: Don't use an unnamed (anonymous) namespace in a header](#Rs-unnamed)
* [SF.22: Use an unnamed (anonymous) namespace for all internal/nonexported entities](#Rs-unnamed2)

### <a name="Rs-file-suffix"></a>SF.1: Use a `.cpp` suffix for code files and `.h` for interface files if your project doesn't already follow another convention

##### Reason

It's a longstanding convention.
But consistency is more important, so if your project uses something else, follow that.

##### Note

This convention reflects a common use pattern:
Headers are more often shared with C to compile as both C++ and C, which typically uses `.h`,
and it's easier to name all headers `.h` instead of having different extensions for just those headers that are intended to be shared with C.
On the other hand, implementation files are rarely shared with C and so should typically be distinguished from `.c` files,
so it's normally best to name all C++ implementation files something else (such as `.cpp`).

The specific names `.h` and `.cpp` are not required (just recommended as a default) and other names are in widespread use.
Examples are `.hh`, `.C`, and `.cxx`. Use such names equivalently.
In this document, we refer to `.h` and `.cpp` as a shorthand for header and implementation files,
even though the actual extension may be different.

Your IDE (if you use one) may have strong opinions about suffices.

##### Example

    // foo.h:
    extern int a;   // a declaration
    extern void foo();

    // foo.cpp:
    int a;   // a definition
    void foo() { ++a; }

`foo.h` provides the interface to `foo.cpp`. Global variables are best avoided.

##### Example, bad

    // foo.h:
    int a;   // a definition
    void foo() { ++a; }

`#include<foo.h>` twice in a program and you get a linker error for two one-definition-rule violations.

##### Enforcement

* Flag non-conventional file names.
* Check that `.h` and `.cpp` (and equivalents) follow the rules below.

### <a name="Rs-inline"></a>SF.2: A `.h` file may not contain object definitions or non-inline function definitions

##### Reason

Including entities subject to the one-definition rule leads to linkage errors.

##### Example

    // file.h:
    namespace Foo {
        int x = 7;
        int xx() { return x+x; }
    }

    // file1.cpp:
    #include<file.h>
    // ... more ...

     // file2.cpp:
    #include<file.h>
    // ... more ...

Linking `file1.cpp` and `file2.cpp` will give two linker errors.

**Alternative formulation**: A `.h` file must contain only:

* `#include`s of other `.h` files (possibly with include guards)
* templates
* class definitions
* function declarations
* `extern` declarations
* `inline` function definitions
* `constexpr` definitions
* `const` definitions
* `using` alias definitions
* ???

##### Enforcement

Check the positive list above.

### <a name="Rs-declaration-header"></a>SF.3: Use `.h` files for all declarations used in multiple source files

##### Reason

Maintainability. Readability.

##### Example, bad

    // bar.cpp:
    void bar() { cout << "bar\n"; }

    // foo.cpp:
    extern void bar();
    void foo() { bar(); }

A maintainer of `bar` cannot find all declarations of `bar` if its type needs changing.
The user of `bar` cannot know if the interface used is complete and correct. At best, error messages come (late) from the linker.

##### Enforcement

* Flag declarations of entities in other source files not placed in a `.h`.

### <a name="Rs-include-order"></a>SF.4: Include `.h` files before other declarations in a file

##### Reason

Minimize context dependencies and increase readability.

##### Example

    #include<vector>
    #include<algorithm>
    #include<string>

    // ... my code here ...

##### Example, bad

    #include<vector>

    // ... my code here ...

    #include<algorithm>
    #include<string>

##### Note

This applies to both `.h` and `.cpp` files.

##### Note

There is an argument for insulating code from declarations and macros in header files by `#including` headers *after* the code we want to protect
(as in the example labeled "bad").
However

* that only works for one file (at one level): Use that technique in a header included with other headers and the vulnerability reappears.
* a namespace (an "implementation namespace") can protect against many context dependencies.
* full protection and flexibility require [modules](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4592.pdf).
[See also](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0141r0.pdf).


##### Enforcement

Easy.

### <a name="Rs-consistency"></a>SF.5: A `.cpp` file must include the `.h` file(s) that defines its interface

##### Reason

This enables the compiler to do an early consistency check.

##### Example, bad

    // foo.h:
    void foo(int);
    int bar(long);
    int foobar(int);

    // foo.cpp:
    void foo(int) { /* ... */ }
    int bar(double) { /* ... */ }
    double foobar(int);

The errors will not be caught until link time for a program calling `bar` or `foobar`.

##### Example

    // foo.h:
    void foo(int);
    int bar(long);
    int foobar(int);

    // foo.cpp:
    #include<foo.h>

    void foo(int) { /* ... */ }
    int bar(double) { /* ... */ }
    double foobar(int);   // error: wrong return type

The return-type error for `foobar` is now caught immediately when `foo.cpp` is compiled.
The argument-type error for `bar` cannot be caught until link time because of the possibility of overloading, but systematic use of `.h` files increases the likelihood that it is caught earlier by the programmer.

##### Enforcement

???

### <a name="Rs-using"></a>SF.6: Use `using`-directives for transition, for foundation libraries (such as `std`), or within a local scope

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rs-using-directive"></a>SF.7: Don't put a `using`-directive in a header file

##### Reason

Doing so takes away an `#include`r's ability to effectively disambiguate and to use alternatives.

##### Example

    ???

##### Enforcement

???

### <a name="Rs-guards"></a>SF.8: Use `#include` guards for all `.h` files

##### Reason

To avoid files being `#include`d several times.

##### Example

    // file foobar.h:
    #ifndef FOOBAR_H
    #define FOOBAR_H
    // ... declarations ...
    #endif // FOOBAR_H

##### Enforcement

Flag `.h` files without `#include` guards.

### <a name="Rs-cycles"></a>SF.9: Avoid cyclic dependencies among source files

##### Reason

Cycles complicates comprehension and slows down compilation.
Complicates conversion to use language-supported modules (when they become available).

##### Note

Eliminate cycles; don't just break them with `#include` guards.

##### Example, bad

    // file1.h:
    #include "file2.h"

    // file2.h:
    #include "file3.h"

    // file3.h:
    #include "file1.h"

##### Enforcement

Flag all cycles.

### <a name="Rs-namespace"></a>SF.20: Use `namespace`s to express logical structure

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rs-unnamed"></a>SF.21: Don't use an unnamed (anonymous) namespace in a header

##### Reason

It is almost always a bug to mention an unnamed namespace in a header file.

##### Example

    ???

##### Enforcement

* Flag any use of an anonymous namespace in a header file.

### <a name="Rs-unnamed2"></a>SF.22: Use an unnamed (anonymous) namespace for all internal/nonexported entities

##### Reason

Nothing external can depend on an entity in a nested unnamed namespace.
Consider putting every definition in an implementation source file in an unnamed namespace unless that is defining an "external/exported" entity.

##### Example

An API class and its members can't live in an unnamed namespace; but any "helper" class or function that is defined in an implementation source file should be at an unnamed namespace scope.

    ???

##### Enforcement

* ???

# <a name="S-stdlib"></a>SL: The Standard Library

Using only the bare language, every task is tedious (in any language).
Using a suitable library any task can be reasonably simple.

The standard library has steadily grown over the years.
Its description in the standard is now larger than that of the language features.
So, it is likely that this library section of the guidelines will eventually grow in size to equal or exceed all the rest.

<< ??? We need another level of rule numbering ??? >>

C++ Standard library component summary:

* [SL.con: Containers](#SS-con)
* [SL.str: String](#SS-string)
* [SL.io: Iostream](#SS-io)
* [SL.regex: Regex](#SS-regex)
* [SL.chrono: Time](#SS-chrono)
* [SL.C: The C standard library](#SS-clib)

Standard-library rule summary:

* [SL.1: Use libraries wherever possible](#Rsl-lib)
* [SL.2: Prefer the standard library to other libraries](#Rsl-sl)
* ???

### <a name="Rsl-lib"></a>SL.1:  Use libraries wherever possible

##### Reason

Save time. Don't re-invent the wheel.
Don't replicate the work of others.
Benefit from other people's work when they make improvements.
Help other people when you make improvements.

### <a name="Rsl-sl"></a>SL.2: Prefer the standard library to other libraries

##### Reason

More people know the standard library.
It is more likely to be stable, well-maintained, and widely available than your own code or most other libraries.

## <a name="SS-con"></a>SL.con: Containers

???

Container rule summary:

* [SL.con.1: Prefer using STL `array` or `vector` instead of a C array](#Rsl-arrays)
* [SL.con.2: Prefer using STL `vector` by default unless you have a reason to use a different container](#Rsl-vector)
*  ???

### <a name="Rsl-arrays"></a>SL.con.1: Prefer using STL `array` or `vector` instead of a C array

##### Reason

C arrays are less safe, and have no advantages over `array` and `vector`.
For a fixed-length array, use `std::array`, which does not degenerate to a pointer when passed to a function and does know its size.
Also, like a built-in array, a stack-allocated `std::array` keeps its elements on the stack.
For a variable-length array, use `std::vector`, which additionally can change its size and handles memory allocation.

##### Example

    int v[SIZE];                        // BAD

    std::array<int, SIZE> w;             // ok

##### Example

    int* v = new int[initial_size];     // BAD, owning raw pointer
    delete[] v;                         // BAD, manual delete

    std::vector<int> w(initial_size);   // ok

##### Enforcement

* Flag declaration of a C array inside a function or class that also declares an STL container (to avoid excessive noisy warnings on legacy non-STL code). To fix: At least change the C array to a `std::array`.

### <a name="Rsl-vector"></a>SL.con.2: Prefer using STL `vector` by default unless you have a reason to use a different container

##### Reason

`vector` and `array` are the only standard containers that offer the fastest general-purpose access (random access, including being vectorization-friendly), the fastest default access pattern (begin-to-end or end-to-begin is prefetcher-friendly), and the lowest space overhead (contiguous layout has zero per-element overhead, which is cache-friendly).
Usually you need to add and remove elements from the container, so use `vector` by default; if you don't need to modify the container's size, use `array`.

Even when other containers seem more suited, such a `map` for O(log N) lookup performance or a `list` for efficient insertion in the middle, a `vector` will usually still perform better for containers up to a few KB in size.

##### Note

`string` should not be used as a container of individual characters. A `string` is a textual string; if you want a container of characters, use `vector</*char_type*/>` or `array</*char_type*/>` instead.

##### Exceptions

If you have a good reason to use another container, use that instead. For example:

* If `vector` suits your needs but you don't need the container to be variable size, use `array` instead.

* If you want a dictionary-style lookup container that guarantees O(K) or O(log N) lookups, the container will be larger (more than a few KB) and you perform frequent inserts so that the overhead of maintaining a sorted `vector` is infeasible, go ahead and use an `unordered_map` or `map` instead.

##### Enforcement

* Flag a `vector` whose size never changes after construction (such as because it's `const` or because no non-`const` functions are called on it). To fix: Use an `array` instead.

## <a name="SS-string"></a>SL.str: String

???

## <a name="SS-io"></a>SL.io: Iostream

???

Iostream rule summary:

* [SL.io.1: Use character-level input only when you have to](#Rio-low)
* [SL.io.2: When reading, always consider ill-formed input](#Rio-validate)
* [???](#???)
* [SL.io.50: Avoid `endl`](#Rio-endl)
* [???](#???)

### <a name="Rio-low"></a>SL.io.1: Use character-level input only when you have to

???

### <a name="Rio-validate"></a>SL.io.2: When reading, always consider ill-formed input

???

### <a name="Rio-endl"></a>SL.io.50: Avoid `endl`

### Reason

The `endl` manipulator is mostly equivalent to `'\n'` and `"\n"`;
as most commonly used it simply slows down output by doing redundant `flush()`s.
This slowdown can be significant compared to `printf`-style output.

##### Example

    cout << "Hello, World!" << endl;    // two output operations and a flush
    cout << "Hello, World!\n";          // one output operation and no flush

##### Note

For `cin`/`cout` (and equivalent) interaction, there is no reason to flush; that's done automatically.
For writing to a file, there is rarely a need to `flush`.

##### Note

Apart from the (occasionally important) issue of performance,
the choice between `'\n'` and `endl` is almost completely aesthetic.

## <a name="SS-regex"></a>SL.regex: Regex

???

## <a name="SS-chrono"></a>SL.chrono: Time

???

## <a name="SS-clib"></a>SL.C: The C standard library

???

C standard library rule summary:

* [???](#???)
* [???](#???)
* [???](#???)


# <a name="S-A"></a>A(Architectural Ideas): 구조적 아이디어

이 절은 보다 고수준의 구조적 아이디어와 라이브러리에 대한 아이디어를 포함한다.

구조적 아이디어 규칙 요약:

* [A.1 코드의 덜 안정적인 부분에서 안정적인 부분을 분리하라](#Ra-stable)
* [A.2 잠재적으로 재사용 가능한 부분을 라이브러리로 표현하라](#Ra-lib)
* [A.4 There should be no cycles among libraries](#?Ra-dag)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)

### <a name="Ra-stable"></a>A.1 코드의 덜 안정적인 부분에서 안정적인 부분을 분리하라

???

### <a name="Ra-lib"></a>A.2 잠재적으로 재사용 가능한 부분을 라이브러리로 표현하라

##### 이유

##### 비고

A library is a collection of declarations and definitions maintained, documented, and shipped together.
A library could be a set of headers (a "header only library") or a set of headers plus a set of object files.
A library can be statically or dynamically linked into a program, or it may be `#included`

### <a name="Ra-dag"></a>A.4 There should be no cycles among libraries

##### Reason

* A cycle implies complication of the build process.
* Cycles are hard to understand and may introduce indeterminism (unspecified behavior).

##### Note

A library can contain cyclic references in the definition of its components.
For example:

    ???

However, a library should not depend on another that depends on it.


# <a name="S-not"></a>NR(Non-Rules and myths): 규칙이 아닌 미신

이 절은 어디선가 인기있는 규칙과 가이드라인을 포함한다. 하지만 우리는 이러한 규칙과 가이드라인을 의도적으로 추천하지는 않는다.
We know full well that there have been times and places where these rules made sense, and we have used them ourselves at times.
그러나, 가이드라인에서 추천하고 지원하는 프로그래밍 스타일의 맥락에서 "규칙이 아닌 미신"은 여러분에게 해를 입힐 수 있따.

Even today, there can be contexts where the rules make sense.
For example, lack of suitable tool support can make exceptions unsuitable in hard-real-time systems,
but please don't blindly trust "common wisdom" (e.g., unsupported statements about "efficiency");
such "wisdom" may be based on decades-old information or experienced from languages with very different properties than C++
(e.g., C or Java).

The positive arguments for alternatives to these non-rules are listed in the rules offered as "Alternatives".

규칙이 아닌 미신 요약:

* [NR.1: Don't: All declarations should be at the top of a function](#Rnr-top)
* [NR.2: Don't: Have only a single single `return`-statement in a function](#Rnr-single-return)
* [NR.3: Don't: Don't use exceptions](#Rnr-no-exceptions)
* [NR.4: Don't: Place each class declaration in its own source file](#Rnr-lots-of-files)
* [NR.5: Don't: Don't do substantive work in a constructor; instead use two-phase initialization](#Rnr-two-phase-init)
* [NR.6: Don't: Place all cleanup actions at the end of a function and `goto exit`](#Rnr-goto-exit)
* [NR.7: Don't: Make all data members `protected`](#Rnr-protected-data)
* ???

### <a name="Rnr-top"></a>NR.1: Don't: All declarations should be at the top of a function

##### Reason (not to follow this rule)

This rule is a legacy of old programming languages that didn't allow initialization of variables and constants after a statement.
This leads to longer programs and more errors caused by uninitialized and wrongly initialized variables.

##### Example, bad

    ???

The larger the distance between the uninitialized variable and its use, the larger the chance of a bug.
Fortunately, compilers catch many "used before set" errors.


##### Alternative

* [Always initialize an object](#Res-always)
* [ES.21: Don't introduce a variable (or constant) before you need to use it](#Res-introduce)

### <a name="Rnr-single-return"></a>NR.2: Don't: Have only a single single `return`-statement in a function

##### Reason (not to follow this rule)

The single-return rule can lead to unnecessarily convoluted code and the introduction of extra state variables.
In particular, the single-return rule makes it harder to concentrate error checking at the top of a function.

##### Example

    template<class T>
    //  requires Number<T>
    string sign(T x)
    {
        if (x < 0)
            return "negative";
        else if (x > 0)
            return "positive";
        return "zero";
    }

to use a single return only we would have to do something like

    template<class T>
    //  requires Number<T>
    string sign(T x)        // bad
    {
        string res;
        if (x < 0)
            res = "negative";
        else if (x > 0)
            res = "positive";
        else
            res = "zero";
        return res;
    }

This is both longer and likely to be less efficient.
The larger and more complicated the function is, the more painful the workarounds get.
Of course many simple functions will naturally have just one `return` because of their simpler inherent logic.

##### Example

    int index(const char* p)
    {
        if (p == nullptr) return -1;  // error indicator: alternatively "throw nullptr_error{}"
        // ... do a lookup to find the index for p
        return i;
    }

If we applied the rule, we'd get something like

    int index2(const char* p)
    {
        int i;
        if (p == nullptr)
            i = -1;  // error indicator
        else {
            // ... do a lookup to find the index for p
        }
        return i;
    }

Note that we (deliberately) violated the rule against uninitialized variables because this style commonly leads to that.
Also, this style is a temptation to use the [goto exit](#Rnr-goto-exit) non-rule.

##### Alternative

* Keep functions short and simple
* Feel free to use multiple `return` statements (and to throw exceptions).

### <a name="Rnr-no-exceptions"></a>NR.3: Don't: Don't use exceptions

##### Reason (not to follow this rule)

There seem to be three main reasons given for this non-rule:

* exceptions are inefficient
* exceptions lead to leaks and errors
* exception performance is not predictable

There is no way we can settle this issue to the satisfaction of everybody.
After all, the discussions about exceptions have been going on for 40+ years.
Some languages cannot be used without exceptions, but others do not support them.
This leads to strong traditions for the use and non-use of exceptions, and to heated debates.

However, we can briefly outline why we consider exceptions the best alternative for general-purpose programming
and in the context of these guidelines.
Simple arguments for and against are often inconclusive.
There are specialized applications where exceptions indeed can be inappropriate
(e.g., hard-real time systems without support for reliable estimates of the cost of handling an exception).

Consider the major objections to exceptions in turn

* Exceptions are inefficient:
Compared to what?
When comparing make sure that the same set of errors are handled and that they are handled equivalently.
In particular, do not compare a program that immediately terminate on seeing an error with a program
that carefully cleans up resources before logging an error.
Yes, some systems have poor exception handling implementations; sometimes, such implementations force us to use
other error-handling approaches, but that's not a fundamental problem with exceptions.
When using an efficiency argument - in any context - be careful that you have good data that actually provides
insight into the problem under discussion.
* Exceptions lead to leaks and errors.
They do not.
If your program is a rat's nest of pointers without an overall strategy for resource management,
you have a problem whatever you do.
If your system consists of a million lines of such code,
you probably will not be able to use exceptions,
but that's a problem with excessive and undisciplined pointer use, rather than with exceptions.
In our opinion, you need RAII to make exception-based error handling simple and safe -- simpler and safer than alternatives.
* Exception performance is not predictable
If you are in a hard-real-time system where you must guarantee completion of a task in a given time,
you need tools to back up such guarantees.
As far as we know such tools are not available (at least not to most programmers).

Many, possibly most, problems with exceptions stem from historical needs to interact with messy old code.

The fundamental arguments for the use of exceptions are

* They clearly separates error return from ordinary return
* They cannot be forgotten or ignored
* They can be used systematically

Remember

* Exceptions are for reporting errors (in C++; other languages can have different uses for exceptions).
* Exceptions are not for errors that can be handled locally.
* Don't try to catch every exception in every function (that's tedious, clumsy, and leads to slow code).
* Exceptions are not for errors that require instant termination of a module/system after a non-recoverable error.

##### Example

    ???

##### Alternative

* [RAII](#Re-raii)
* Contracts/assertions: Use GSL's `Expects` and `Ensures` (until we get language support for contracts)

### <a name="Rnr-lots-of-files"></a>NR.4: Don't: Place each class declaration in its own source file

##### Reason (not to follow this rule)

The resulting number of files are hard to manage and can slow down compilation.
Individual classes are rarely a good logical unit of maintenance and distribution.

##### Example

    ???

##### Alternative

* Use namespaces containing logically cohesive sets of classes and functions.

### <a name="Rnr-two-phase-init"></a>NR.5: Don't: Don't do substantive work in a constructor; instead use two-phase initialization

##### Reason (not to follow this rule)

Following this rule leads to weaker invariants,
more complicated code (having to deal with semi-constructed objects),
and errors (when we didn't deal correctly with semi-constructed objects consistently).

##### Example

    ???

##### Alternative

* Always establish a class invariant in a constructor.
* Don't define an object before it is needed.

### <a name="Rnr-goto-exit"></a>NR.6: Don't: Place all cleanup actions at the end of a function and `goto exit`

##### Reason (not to follow this rule)

`goto` is error-prone.
This technique is a pre-exception technique for RAII-like resource and error handling.

##### Example, bad

    void do_something(int n)
    {
        if (n < 100) goto exit;
        // ...
        int* p = (int*) malloc(n);
        // ...
        if (some_ error) goto_exit;
        // ...
    exit:
        free(p);
    }

and spot the bug.

##### Alternative

* Use exceptions and [RAII](#Re-raii)
* for non-RAII resources, use [`finally`](#Re-finally).

### <a name="Rnr-protected-data"></a>NR.7: Don't: Make all data members `protected`

##### Reason (not to follow this rule)

`protected` data is a source of errors.
`protected` data can be manipulated from an unbounded amount of code in various places.
`protected` data is the class hierarchy equivalent to global data.

##### Example

    ???

##### Alternative

* [Make member data `public` or (preferably) `private`](#Rh-protected)


# <a name="S-references"></a>RF: References

Many coding standards, rules, and guidelines have been written for C++, and especially for specialized uses of C++.
Many

* focus on lower-level issues, such as the spelling of identifiers
* are written by C++ novices
* see "stopping programmers from doing unusual things" as their primary aim
* aim at portability across many compilers (some 10 years old)
* are written to preserve decades old code bases
* aim at a single application domain
* are downright counterproductive
* are ignored (must be ignored by programmers to get their work done well)

A bad coding standard is worse than no coding standard.
However an appropriate set of guidelines are much better than no standards: "Form is liberating."

Why can't we just have a language that allows all we want and disallows all we don't want ("a perfect language")?
Fundamentally, because affordable languages (and their tool chains) also serve people with needs that differ from yours and serve more needs than you have today.
Also, your needs change over time and a general-purpose language is needed to allow you to adapt.
A language that is ideal for today would be overly restrictive tomorrow.

Coding guidelines adapt the use of a language to specific needs.
Thus, there cannot be a single coding style for everybody.
We expect different organizations to provide additions, typically with more restrictions and firmer style rules.

Reference sections:

* [RF.rules: Coding rules](#SS-rules)
* [RF.books: Books with coding guidelines](#SS-books)
* [RF.C++: C++ Programming (C++11/C++14)](#SS-Cplusplus)
* [RF.web: Websites](#SS-web)
* [RS.video: Videos about "modern C++"](#SS-vid)
* [RF.man: Manuals](#SS-man)

## <a name="SS-rules"></a>RF.rules: Coding rules

* [Boost Library Requirements and Guidelines](http://www.boost.org/development/requirements.html).
  ???.
* [Bloomberg: BDE C++ Coding](https://github.com/bloomberg/bde/wiki/CodingStandards.pdf).
  Has a strong emphasis on code organization and layout.
* Facebook: ???
* [GCC Coding Conventions](https://gcc.gnu.org/codingconventions.html).
  C++03 and (reasonably) a bit backwards looking.
* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html).
  Geared toward C++03 and (also) older code bases. Google experts are now actively collaborating here on helping to improve these Guidelines, and hopefully to merge efforts so these can be a modern common set they could also recommend.
* [JSF++: JOINT STRIKE FIGHTER AIR VEHICLE C++ CODING STANDARDS](http://www.stroustrup.com/JSF-AV-rules.pdf).
  Document Number 2RDU00001 Rev C. December 2005.
  For flight control software.
  For hard real time.
  This means that it is necessarily very restrictive ("if the program fails somebody dies").
  For example, no free store allocation or deallocation may occur after the plane takes off (no memory overflow and no fragmentation allowed).
  No exception may be used (because there was no available tool for guaranteeing that an exception would be handled within a fixed short time).
  Libraries used have to have been approved for mission critical applications.
  Any similarities to this set of guidelines are unsurprising because Bjarne Stroustrup was an author of JSF++.
  Recommended, but note its very specific focus.
* [Mozilla Portability Guide](https://developer.mozilla.org/en-US/docs/Mozilla/C%2B%2B_Portability_Guide).
  As the name indicates, this aims for portability across many (old) compilers.
  As such, it is restrictive.
* [Geosoft.no: C++ Programming Style Guidelines](http://geosoft.no/development/cppstyle.html).
  ???.
* [Possibility.com: C++ Coding Standard](http://www.possibility.com/Cpp/CppCodingStandard.html).
  ???.
* [SEI CERT: Secure C++ Coding Standard](https://www.securecoding.cert.org/confluence/pages/viewpage.action?pageId=637).
  A very nicely done set of rules (with examples and rationales) done for security-sensitive code.
  Many of their rules apply generally.
* [High Integrity C++ Coding Standard](http://www.codingstandard.com/).
* [llvm](http://llvm.org/docs/CodingStandards.html).
  Somewhat brief, pre-C++11, and (not unreasonably) adjusted to its domain.
* ???

## <a name="SS-books"></a>RF.books: Books with coding guidelines

* [Meyers96](#Meyers96) Scott Meyers: *More Effective C++*. Addison-Wesley 1996.
* [Meyers97](#Meyers97) Scott Meyers: *Effective C++, Second Edition*. Addison-Wesley 1997.
* [Meyers01](#Meyers01) Scott Meyers: *Effective STL*. Addison-Wesley 2001.
* [Meyers05](#Meyers05) Scott Meyers: *Effective C++, Third Edition*. Addison-Wesley 2005.
* [Meyers15](#Meyers15) Scott Meyers: *Effective Modern C++*. O'Reilly 2015.
* [SuttAlex05](#SuttAlex05) Sutter and Alexandrescu: *C++ Coding Standards*. Addison-Wesley 2005. More a set of meta-rules than a set of rules. Pre-C++11.
* [Stroustrup05](#Stroustrup05) Bjarne Stroustrup: [A rationale for semantically enhanced library languages](http://www.stroustrup.com/SELLrationale.pdf).
  LCSD05. October 2005.
* [Stroustrup14](#Stroustrup05) Stroustrup: [A Tour of C++](http://www.stroustrup.com/Tour.html).
  Addison Wesley 2014.
  Each chapter ends with an advice section consisting of a set of recommendations.
* [Stroustrup13](#Stroustrup13) Stroustrup: [The C++ Programming Language (4th Edition)](http://www.stroustrup.com/4th.html).
  Addison Wesley 2013.
  Each chapter ends with an advice section consisting of a set of recommendations.
* Stroustrup: [Style Guide](http://www.stroustrup.com/Programming/PPP-style.pdf)
  for [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html).
  Mostly low-level naming and layout rules.
  Primarily a teaching tool.

## <a name="SS-Cplusplus"></a>RF.C++: C++ Programming (C++11/C++14)

* [TC++PL4](http://www.stroustrup.com/4th.html):
A thorough description of the C++ language and standard libraries for experienced programmers.
* [Tour++](http://www.stroustrup.com/Tour.html):
An overview of the C++ language and standard libraries for experienced programmers.
* [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html):
A textbook for beginners and relative novices.

## <a name="SS-web"></a>RF.web: Websites

* [isocpp.org](https://isocpp.org)
* [Bjarne Stroustrup's home pages](http://www.stroustrup.com)
* [WG21](http://www.open-std.org/jtc1/sc22/wg21/)
* [Boost](http://www.boost.org)<a name="Boost"></a>
* [Adobe open source](http://www.adobe.com/open-source.html)
* [Poco libraries](http://pocoproject.org/)
* Sutter's Mill?
* ???

## <a name="SS-vid"></a>RS.video: Videos about "modern C++"

* Bjarne Stroustrup: [C++11 Style](http://channel9.msdn.com/Events/GoingNative/GoingNative-2012/Keynote-Bjarne-Stroustrup-Cpp11-Style). 2012.
* Bjarne Stroustrup: [The Essence of C++: With Examples in C++84, C++98, C++11, and C++14](http://channel9.msdn.com/Events/GoingNative/2013/Opening-Keynote-Bjarne-Stroustrup). 2013
* All the talks from [CppCon '14](https://isocpp.org/blog/2014/11/cppcon-videos-c9)
* Bjarne Stroustrup: [The essence of C++](https://www.youtube.com/watch?v=86xWVb4XIyE) at the University of Edinburgh. 2014.
* Sutter: ???
* CppCon 15
* ??? C++ Next
* ??? Meting C++
* ??? more ???

## <a name="SS-man"></a>RF.man: Manuals

* ISO C++ Standard C++11.
* ISO C++ Standard C++14.
* [ISO C++ Standard C++17 CD](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4606.pdf). Committee Draft.
* [Palo Alto "Concepts" TR](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2012/n3351.pdf).
* [ISO C++ Concepts TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
* [WG21 Ranges report](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf). Draft.

## <a name="SS-ack"></a>Acknowledgements

Thanks to the many people who contributed rules, suggestions, supporting information, references, etc.:

* Peter Juhl
* Neil MacIntosh
* Axel Naumann
* Andrew Pardoe
* Gabriel Dos Reis
* Zhuang, Jiangang (Jeff)
* Sergey Zubkov

and see the contributor list on the github.

# <a name="S-profile"></a>Pro: Profiles

A "profile" is a set of deterministic and portably enforceable subset rules (i.e., restrictions) that are designed to achieve a specific guarantee. "Deterministic" means they require only local analysis and could be implemented in a compiler (though they don't need to be). "Portably enforceable" means they are like language rules, so programmers can count on enforcement tools giving the same answer for the same code.

Code written to be warning-free using such a language profile is considered to conform to the profile. Conforming code is considered to be safe by construction with regard to the safety properties targeted by that profile. Conforming code will not be the root cause of errors for that property, although such errors may be introduced into a program by other code, libraries or the external environment. A profile may also introduce additional library types to ease conformance and encourage correct code.

Profiles summary:

* [Pro.type: Type safety](#SS-type)
* [Pro.bounds: Bounds safety](#SS-bounds)
* [Pro.lifetime: Lifetime safety](#SS-lifetime)

In the future, we expect to define many more profiles and add more checks to existing profiles.
Candidates include:

* narrowing arithmetic promotions/conversions (likely part of a separate safe-arithmetic profile)
* arithmetic cast from negative floating point to unsigned integral type (ditto)
* selected undefined behavior: ??? start with Gaby's UB list
* selected unspecified behavior: ??? a portability concern?
* `const` violations

To suppress enforcement of a profile check, place a `suppress` annotation on a language contract. For example:

    [[suppress(bounds)]] char* raw_find(char* p, int n, char x)    // find x in p[0]..p[n-1]
    {
        // ...
    }

Now `raw_find()` can scramble memory to its heart's content.
Obviously, suppression should be very rare.

## <a name="SS-type"></a>Pro.safety: Type safety profile

This profile makes it easier to construct code that uses types correctly and avoids inadvertent type punning.
It does so by focusing on removing the primary sources of type violations, including unsafe uses of casts and unions.

For the purposes of this section,
type-safety is defined to be the property that a variable is not used in a way that doesn't obey the rules for the type of its definition.
Memory accessed as a type `T` should not be valid memory that actually contains an object of an unrelated type `U`.
Note that the safety is intended to be complete when combined also with [Bounds safety](#SS-bounds) and [Lifetime safety](#SS-lifetime).

An implementation of this profile shall recognize the following patterns in source code as non-conforming and issue a diagnostic.

Type safety profile summary:

* [Type.1: Don't use `reinterpret_cast`](#Pro-type-reinterpretcast)
* [Type.2: Don't use `static_cast` downcasts. Use `dynamic_cast` instead](#Pro-type-downcast)
* [Type.3: Don't use `const_cast` to cast away `const` (i.e., at all)](#Pro-type-constcast)
* [Type.4: Don't use C-style `(T)expression` casts that would perform a `static_cast` downcast, `const_cast`, or `reinterpret_cast`](#Pro-type-cstylecast)
* [Type.5: Don't use a variable before it has been initialized](#Pro-type-init)
* [Type.6: Always initialize a member variable](#Pro-type-memberinit)

### <a name="Pro-type-reinterpretcast"></a>Type.1: Don't use `reinterpret_cast`.

##### Reason

Use of these casts can violate type safety and cause the program to access a variable that is actually of type `X` to be accessed as if it were of an unrelated type `Z`.

##### Example, bad

    std::string s = "hello world";
    double* p = reinterpret_cast<double*>(&s); // BAD

##### Enforcement

Issue a diagnostic for any use of `reinterpret_cast`. To fix: Consider using a `variant` instead.

### <a name="Pro-type-downcast"></a>Type.2: Don't use `static_cast` downcasts. Use `dynamic_cast` instead.

##### Reason

Use of these casts can violate type safety and cause the program to access a variable that is actually of type `X` to be accessed as if it were of an unrelated type `Z`.

##### Example, bad

    class Base { public: virtual ~Base() = 0; };

    class Derived1 : public Base { };

    class Derived2 : public Base {
        std::string s;
    public:
        std::string get_s() { return s; }
    };

    Derived1 d1;
    Base* p1 = &d1; // ok, implicit conversion to pointer to Base is fine

    // BAD, tries to treat d1 as a Derived2, which it is not
    Derived2* p2 = static_cast<Derived2*>(p1);
    // tries to access d1's nonexistent string member, instead sees arbitrary bytes near d1
    cout << p2->get_s();

##### Example, bad

    struct Foo { int a, b; };
    struct Foobar : Foo { int bar; };

    void use(int i, Foo& x)
    {
        if (0 < i) {
            Foobar& x1 = dynamic_cast<Foobar&>(x);  // error: Foo is not polymorphic
            Foobar& x2 = static_cast<Foobar&>(x);   // bad
            // ...
        }
        // ...
    }

    // ...

    use(99, *new Foo{1, 2});  // not a Foobar

If a class hierarchy isn't polymorphic, avoid casting.
It is entirely unsafe.
Look for a better design.
See also [C.146](#Rh-dynamic_cast).

##### Enforcement

Issue a diagnostic for any use of `static_cast` to downcast, meaning to cast from a pointer or reference to `X` to a pointer or reference to a type that is not `X` or an accessible base of `X`. To fix: If this is a downcast or cross-cast then use a `dynamic_cast` instead, otherwise consider using a `variant` instead.

### <a name="Pro-type-constcast"></a>Type.3: Don't use `const_cast` to cast away `const` (i.e., at all).

##### Reason

Casting away `const` is a lie. If the variable is actually declared `const`, it's a lie punishable by undefined behavior.

##### Example, bad

    void f(const int& i)
    {
        const_cast<int&>(i) = 42;   // BAD
    }

    static int i = 0;
    static const int j = 0;

    f(i); // silent side effect
    f(j); // undefined behavior

##### Example

Sometimes you may be tempted to resort to `const_cast` to avoid code duplication, such as when two accessor functions that differ only in `const`-ness have similar implementations. For example:

    class Bar;

    class Foo {
    public:
        // BAD, duplicates logic
        Bar& get_bar() {
            /* complex logic around getting a non-const reference to my_bar */
        }

        const Bar& get_bar() const {
            /* same complex logic around getting a const reference to my_bar */
        }
    private:
        Bar my_bar;
    };

Instead, prefer to share implementations. Normally, you can just have the non-`const` function call the `const` function. However, when there is complex logic this can lead to the following pattern that still resorts to a `const_cast`:

    class Foo {
    public:
        // not great, non-const calls const version but resorts to const_cast
        Bar& get_bar() {
            return const_cast<Bar&>(static_cast<const Foo&>(*this).get_bar());
        }
        const Bar& get_bar() const {
            /* the complex logic around getting a const reference to my_bar */
        }
    private:
        Bar my_bar;
    };

Although this pattern is safe when applied correctly, because the caller must have had a non-`const` object to begin with, it's not ideal because the safety is hard to enforce automatically as a checker rule.

Instead, prefer to put the common code in a common helper function -- and make it a template so that it deduces `const`. This doesn't use any `const_cast` at all:

    class Foo {
    public:                         // good
              Bar& get_bar()       { return get_bar_impl(*this); }
        const Bar& get_bar() const { return get_bar_impl(*this); }
    private:
        Bar my_bar;

        template<class T>           // good, deduces whether T is const or non-const
        static auto get_bar_impl(T& t) -> decltype(t.get_bar())
            { /* the complex logic around getting a possibly-const reference to my_bar */ }
    };

##### Exception

You may need to cast away `const` when calling `const`-incorrect functions. Prefer to wrap such functions in inline `const`-correct wrappers to encapsulate the cast in one place.

##### Enforcement

Issue a diagnostic for any use of `const_cast`. To fix: Either don't use the variable in a non-`const` way, or don't make it `const`.

### <a name="Pro-type-cstylecast"></a>Type.4: Don't use C-style `(T)expression` casts that would perform a `static_cast` downcast, `const_cast`, or `reinterpret_cast`.

##### Reason

Use of these casts can violate type safety and cause the program to access a variable that is actually of type `X` to be accessed as if it were of an unrelated type `Z`.
Note that a C-style `(T)expression` cast means to perform the first of the following that is possible: a `const_cast`, a `static_cast`, a `static_cast` followed by a `const_cast`, a `reinterpret_cast`, or a `reinterpret_cast` followed by a `const_cast`. This rule bans `(T)expression` only when used to perform an unsafe cast.

##### Example, bad

    std::string s = "hello world";
    double* p0 = (double*)(&s); // BAD

    class Base { public: virtual ~Base() = 0; };

    class Derived1 : public Base { };

    class Derived2 : public Base {
        std::string s;
    public:
        std::string get_s() { return s; }
    };

    Derived1 d1;
    Base* p1 = &d1; // ok, implicit conversion to pointer to Base is fine

    // BAD, tries to treat d1 as a Derived2, which it is not
    Derived2* p2 = (Derived2*)(p1);
    // tries to access d1's nonexistent string member, instead sees arbitrary bytes near d1
    cout << p2->get_s();

    void f(const int& i) {
        (int&)(i) = 42;   // BAD
    }

    static int i = 0;
    static const int j = 0;

    f(i); // silent side effect
    f(j); // undefined behavior

##### Enforcement

Issue a diagnostic for any use of a C-style `(T)expression` cast that would invoke a `static_cast` downcast, `const_cast`, or `reinterpret_cast`. To fix: Use a `dynamic_cast`, `const`-correct declaration, or `variant`, respectively.

### <a name="Pro-type-init"></a>Type.5: Don't use a variable before it has been initialized.

[ES.20: Always initialize an object](#Res-always) is required.

### <a name="Pro-type-memberinit"></a>Type.6: Always initialize a member variable.

##### Reason

Before a variable has been initialized, it does not contain a deterministic valid value of its type. It could contain any arbitrary bit pattern, which could be different on each call.

##### Example

    struct X { int i; };

    X x;
    use(x); // BAD, x has not been initialized

    X x2{}; // GOOD
    use(x2);

##### Enforcement

* Issue a diagnostic for any constructor of a non-trivially-constructible type that does not initialize all member variables. To fix: Write a data member initializer, or mention it in the member initializer list.
* Issue a diagnostic when constructing an object of a trivially constructible type without `()` or `{}` to initialize its members. To fix: Add `()` or `{}`.

### <a name="Pro-type-unions"></a>Type.7: Avoid accessing members of raw unions. Prefer `variant` instead.

##### Reason

Reading from a union member assumes that member was the last one written, and writing to a union member assumes another member with a nontrivial destructor had its destructor called. This is fragile because it cannot generally be enforced to be safe in the language and so relies on programmer discipline to get it right.

##### Example

    union U { int i; double d; };

    U u;
    u.i = 42;
    use(u.d); // BAD, undefined

    variant<int, double> u;
    u = 42; // u now contains int
    use(u.get<int>()); // ok
    use(u.get<double>()); // throws ??? update this when standardization finalizes the variant design

Note that just copying a union is not type-unsafe, so safe code can pass a union from one piece of unsafe code to another.

##### Enforcement

* Issue a diagnostic for accessing a member of a union. To fix: Use a `variant` instead.

### <a name="Pro-type-varargs"></a>Type.8: Avoid reading from varargs or passing vararg arguments. Prefer variadic template parameters instead.

##### Reason

Reading from a vararg assumes that the correct type was actually passed. Passing to varargs assumes the correct type will be read. This is fragile because it cannot generally be enforced to be safe in the language and so relies on programmer discipline to get it right.

##### Example

    int sum(...) {
        // ...
        while (/*...*/)
            result += va_arg(list, int); // BAD, assumes it will be passed ints
        // ...
    }

    sum(3, 2); // ok
    sum(3.14159, 2.71828); // BAD, undefined

    template<class ...Args>
    auto sum(Args... args) { // GOOD, and much more flexible
        return (... + args); // note: C++17 "fold expression"
    }

    sum(3, 2); // ok: 5
    sum(3.14159, 2.71828); // ok: ~5.85987

Note: Declaring a `...` parameter is sometimes useful for techniques that don't involve actual argument passing, notably to declare "take-anything" functions so as to disable "everything else" in an overload set or express a catchall case in a template metaprogram.

##### Enforcement

* Issue a diagnostic for using `va_list`, `va_start`, or `va_arg`. To fix: Use a variadic template parameter list instead.
* Issue a diagnostic for passing an argument to a vararg parameter of a function that does not offer an overload for a more specific type in the position of the vararg. To fix: Use a different function, or `[[suppress(types)]]`.

## <a name="SS-bounds"></a>Pro.bounds: Bounds safety profile

This profile makes it easier to construct code that operates within the bounds of allocated blocks of memory. It does so by focusing on removing the primary sources of bounds violations: pointer arithmetic and array indexing. One of the core features of this profile is to restrict pointers to only refer to single objects, not arrays.

For the purposes of this document, bounds-safety is defined to be the property that a program does not use a variable to access memory outside of the range that was allocated and assigned to that variable. (Note that the safety is intended to be complete when combined also with [Type safety](#SS-type) and [Lifetime safety](#SS-lifetime), which cover other unsafe operations that allow bounds violations, such as type-unsafe casts that 'widen' pointers.)

The following are under consideration but not yet in the rules below, and may be better in other profiles:

* ???

An implementation of this profile shall recognize the following patterns in source code as non-conforming and issue a diagnostic.

### <a name="Pro-bounds-arithmetic"></a>Bounds.1: Don't use pointer arithmetic. Use `span` instead.

##### Reason

Pointers should only refer to single objects, and pointer arithmetic is fragile and easy to get wrong. `span` is a bounds-checked, safe type for accessing arrays of data.

##### Example, bad

    void f(int* p, int count)
    {
        if (count < 2) return;

        int* q = p + 1; // BAD

        ptrdiff_t d;
        int n;
        d = (p - &n); // OK
        d = (q - p); // OK

        int n = *p++; // BAD

        if (count < 6) return;

        p[4] = 1; // BAD

        p[count - 1] = 2; // BAD

        use(&p[0], 3); // BAD
    }

##### Example, good

    void f(span<int> a) // BETTER: use span in the function declaration
    {
        if (a.length() < 2) return;

        int n = *a++; // OK

        span<int> q = a + 1; // OK

        if (a.length() < 6) return;

        a[4] = 1; // OK

        a[count - 1] = 2; // OK

        use(a.data(), 3); // OK
    }

##### Enforcement

Issue a diagnostic for any arithmetic operation on an expression of pointer type that results in a value of pointer type.

### <a name="Pro-bounds-arrayindex"></a>Bounds.2: Only index into arrays using constant expressions.

##### Reason

Dynamic accesses into arrays are difficult for both tools and humans to validate as safe. `span` is a bounds-checked, safe type for accessing arrays of data. `at()` is another alternative that ensures single accesses are bounds-checked. If iterators are needed to access an array, use the iterators from a `span` constructed over the array.

##### Example, bad

    void f(array<int, 10> a, int pos)
    {
        a[pos / 2] = 1; // BAD
        a[pos - 1] = 2; // BAD
        a[-1] = 3;    // BAD -- no replacement, just don't do this
        a[10] = 4;    // BAD -- no replacement, just don't do this
    }

##### Example, good

    // ALTERNATIVE A: Use a span

    // A1: Change parameter type to use span
    void f1(span<int, 10> a, int pos)
    {
        a[pos / 2] = 1; // OK
        a[pos - 1] = 2; // OK
    }

    // A2: Add local span and use that
    void f2(array<int, 10> arr, int pos)
    {
        span<int> a = {arr, pos}
        a[pos / 2] = 1; // OK
        a[pos - 1] = 2; // OK
    }

    // ALTERNATIVE B: Use at() for access
    void f3(array<int, 10> a, int pos)
    {
        at(a, pos / 2) = 1; // OK
        at(a, pos - 1) = 2; // OK
    }

##### Example, bad

    void f()
    {
        int arr[COUNT];
        for (int i = 0; i < COUNT; ++i)
            arr[i] = i; // BAD, cannot use non-constant indexer
    }

##### Example, good

    // ALTERNATIVE A: Use a span
    void f1()
    {
        int arr[COUNT];
        span<int> av = arr;
        for (int i = 0; i < COUNT; ++i)
            av[i] = i;
    }

    // ALTERNATIVE B: Use at() for access
    void f2()
    {
        int arr[COUNT];
        for (int i = 0; i < COUNT; ++i)
            at(arr, i) = i;
    }

##### Enforcement

Issue a diagnostic for any indexing expression on an expression or variable of array type (either static array or `std::array`) where the indexer is not a compile-time constant expression.

Issue a diagnostic for any indexing expression on an expression or variable of array type (either static array or `std::array`) where the indexer is not a value between `0` or and the upper bound of the array.

**Rewrite support**: Tooling can offer rewrites of array accesses that involve dynamic index expressions to use `at()` instead:

    static int a[10];

    void f(int i, int j)
    {
        a[i + j] = 12;      // BAD, could be rewritten as ...
        at(a, i + j) = 12;  // OK -- bounds-checked
    }

### <a name="Pro-bounds-decay"></a>Bounds.3: No array-to-pointer decay.

##### Reason

Pointers should not be used as arrays. `span` is a bounds-checked, safe alternative to using pointers to access arrays.

##### Example, bad

    void g(int* p, size_t length);

    void f()
    {
        int a[5];
        g(a, 5);        // BAD
        g(&a[0], 1);    // OK
    }

##### Example, good

    void g(int* p, size_t length);
    void g1(span<int> av); // BETTER: get g() changed.

    void f()
    {
        int a[5];
        span<int> av = a;

        g(av.data(), av.length());   // OK, if you have no choice
        g1(a);                       // OK -- no decay here, instead use implicit span ctor
    }

##### Enforcement

Issue a diagnostic for any expression that would rely on implicit conversion of an array type to a pointer type.

### <a name="Pro-bounds-stdlib"></a>Bounds.4: Don't use standard library functions and types that are not bounds-checked.

##### Reason

These functions all have bounds-safe overloads that take `span`. Standard types such as `vector` can be modified to perform bounds-checks under the bounds profile (in a compatible way, such as by adding contracts), or used with `at()`.

##### Example, bad

    void f()
    {
        array<int, 10> a, b;
        memset(a.data(), 0, 10);         // BAD, and contains a length error (length = 10 * sizeof(int))
        memcmp(a.data(), b.data(), 10);  // BAD, and contains a length error (length = 10 * sizeof(int))
    }

Also, `std::array<>::fill()` or `std::fill()` or even an empty initializer are better candidate than `memset()`.

##### Example, good

    void f()
    {
        array<int, 10> a, b, c{};       // c is initialized to zero
        a.fill(0);
        fill(b.begin(), b.end(), 0);    // std::fill()
        fill(b, 0);                     // std::fill() + Ranges TS

        if ( a == b ) {
          // ...
        }
    }

##### Example

If code is using an unmodified standard library, then there are still workarounds that enable use of `std::array` and `std::vector` in a bounds-safe manner. Code can call the `.at()` member function on each class, which will result in an `std::out_of_range` exception being thrown. Alternatively, code can call the `at()` free function, which will result in fail-fast (or a customized action) on a bounds violation.

    void f(std::vector<int>& v, std::array<int, 12> a, int i)
    {
        v[0] = a[0];        // BAD
        v.at(0) = a[0];     // OK (alternative 1)
        at(v, 0) = a[0];    // OK (alternative 2)

        v.at(0) = a[i];     // BAD
        v.at(0) = a.at(i);  // OK (alternative 1)
        v.at(0) = at(a, i); // OK (alternative 2)
    }

##### Enforcement

* Issue a diagnostic for any call to a standard library function that is not bounds-checked. ??? insert link to a list of banned functions

**TODO Notes**:

* Impact on the standard library will require close coordination with WG21, if only to ensure compatibility even if never standardized.
* We are considering specifying bounds-safe overloads for stdlib (especially C stdlib) functions like `memcmp` and shipping them in the GSL.
* For existing stdlib functions and types like `vector` that are not fully bounds-checked, the goal is for these features to be bounds-checked when called from code with the bounds profile on, and unchecked when called from legacy code, possibly using contracts (concurrently being proposed by several WG21 members).

## <a name="SS-lifetime"></a>Pro.lifetime: Lifetime safety profile

???

# <a name="S-gsl"></a>GSL: Guideline support library

The GSL is a small library of facilities designed to support this set of guidelines.
Without these facilities, the guidelines would have to be far more restrictive on language details.

The Core Guidelines support library is defined in namespace `gsl` and the names may be aliases for standard library or other well-known library names. Using the (compile-time) indirection through the `gsl` namespace allows for experimentation and for local variants of the support facilities.

The GSL is header only, and can be found at [GSL: Guideline support library](https://github.com/Microsoft/GSL).
The support library facilities are designed to be extremely lightweight (zero-overhead) so that they impose no overhead compared to using conventional alternatives.
Where desirable, they can be "instrumented" with additional functionality (e.g., checks) for tasks such as debugging.

These Guidelines assume a `variant` type, but this is not currently in GSL.
Eventually, use [the one voted into C++17](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0088r3.html).

Summary of GSL components:

* [GSL.view: Views](#SS-views)
* [GSL.owner](#SS-ownership)
* [GSL.assert: Assertions](#SS-assertions)
* [GSL.util: Utilities](#SS-utilities)
* [GSL.concept: Concepts](#SS-gsl-concepts)

We plan for a "ISO C++ standard style" semi-formal specification of the GSL.

We rely on the ISO C++ standard library and hope for parts of the GSL to be absorbed into the standard library.

## <a name="SS-views"></a>GSL.view: Views

These types allow the user to distinguish between owning and non-owning pointers and between pointers to a single object and pointers to the first element of a sequence.

These "views" are never owners.

References are never owners.

The names are mostly ISO standard-library style (lower case and underscore):

* `T*`      // The `T*` is not an owner, may be null; assumed to be pointing to a single element.
* `T&`      // The `T&` is not an owner and can never be a "null reference"; references are always bound to objects.

The "raw-pointer" notation (e.g. `int*`) is assumed to have its most common meaning; that is, a pointer points to an object, but does not own it.
Owners should be converted to resource handles (e.g., `unique_ptr` or `vector<T>`) or marked `owner<T*>`

* `owner<T*>`   // a `T*`that owns the object pointed/referred to; may be `nullptr`.
* `owner<T&>`   // a `T&` that owns the object pointed/referred to.

`owner` is used to mark owning pointers in code that cannot be upgraded to use proper resource handles.
Reasons for that include:

* Cost of conversion.
* The pointer is used with an ABI.
* The pointer is part of the implementation of a resource handle.

An `owner<T>` differs from a resource handle for a `T` by still requiring an explicit `delete`.

An `owner<T>` is assumed to refer to an object on the free store (heap).

If something is not supposed to be `nullptr`, say so:

* `not_null<T>`   // `T` is usually a pointer type (e.g., `not_null<int*>` and `not_null<owner<Foo*>>`) that may not be `nullptr`.
  `T` can be any type for which `==nullptr` is meaningful.

* `span<T>`       // `[`p`:`p+n`), constructor from `{p, q}` and `{p, n}`; `T` is the pointer type
* `span_p<T>`     // `{p, predicate}` \[`p`:`q`) where `q` is the first element for which `predicate(*p)` is true
* `string_span`   // `span<char>`
* `cstring_span`  // `span<const char>`

A `span<T>` refers to zero or more mutable `T`s unless `T` is a `const` type.

"Pointer arithmetic" is best done within `span`s.
A `char*` that points to more than one `char` but is not a C-style string (e.g., a pointer into an input buffer) should be represented by a `span`.

* `zstring`    // a `char*` supposed to be a C-style string; that is, a zero-terminated sequence of `char` or `nullptr`
* `czstring`   // a `const char*` supposed to be a C-style string; that is, a zero-terminated sequence of `const` `char` or `nullptr`

Logically, those last two aliases are not needed, but we are not always logical, and they make the distinction between a pointer to one `char` and a pointer to a C-style string explicit.
A sequence of characters that is not assumed to be zero-terminated should be a `char*`, rather than a `zstring`.
French accent optional.

Use `not_null<zstring>` for C-style strings that cannot be `nullptr`. ??? Do we need a name for `not_null<zstring>`? or is its ugliness a feature?

## <a name="SS-ownership"></a>GSL.owner: Ownership pointers

* `unique_ptr<T>`     // unique ownership: `std::unique_ptr<T>`
* `shared_ptr<T>`     // shared ownership: `std::shared_ptr<T>` (a counted pointer)
* `stack_array<T>`    // A stack-allocated array. The number of elements are determined at construction and fixed thereafter. The elements are mutable unless `T` is a `const` type.
* `dyn_array<T>`      // ??? needed ??? A heap-allocated array. The number of elements are determined at construction and fixed thereafter.
  The elements are mutable unless `T` is a `const` type. Basically a `span` that allocates and owns its elements.

## <a name="SS-assertions"></a>GSL.assert: Assertions

* `Expects`     // precondition assertion. Currently placed in function bodies. Later, should be moved to declarations.
                // `Expects(p)` terminates the program unless `p == true`
                // `Expect` in under control of some options (enforcement, error message, alternatives to terminate)
* `Ensures`     // postcondition assertion. Currently placed in function bodies. Later, should be moved to declarations.

These assertions is currently macros (yuck!) and must appear in function definitions (only)
pending standard commission decisions on contracts and assertion syntax.
See [the contract proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf) uses the attribute syntax,
for example, `Expects(p!=nullptr)` will become`[[expects: p!=nullptr]]`.

## <a name="SS-utilities"></a>GSL.util: Utilities

* `finally`       // `finally(f)` makes a `final_action{f}` with a destructor that invokes `f`
* `narrow_cast`   // `narrow_cast<T>(x)` is `static_cast<T>(x)`
* `narrow`        // `narrow<T>(x)` is `static_cast<T>(x)` if `static_cast<T>(x) == x` or it throws `narrowing_error`
* `[[implicit]]`  // "Marker" to put on single-argument constructors to explicitly make them non-explicit.
* `move_owner`    // `p = move_owner(q)` means `p = q` but ???

## <a name="SS-gsl-concepts"></a>GSL.concept: Concepts

These concepts (type predicates) are borrowed from
Andrew Sutton's Origin library,
the Range proposal,
and the ISO WG21 Palo Alto TR.
They are likely to be very similar to what will become part of the ISO C++ standard.
The notation is that of the ISO WG21 [Concepts TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
Most of the concepts below are defined in [the Ranges TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf).

* `Range`
* `String`   // ???
* `Number`   // ???
* `Sortable`
* `Pointer`  // A type with `*`, `->`, `==`, and default construction (default construction is assumed to set the singular "null" value); see [smart pointers](#Rr-smartptrconcepts)
* `Unique_ptr`  // A type that matches `Pointer`, has move (not copy), and matches the Lifetime profile criteria for a `unique` owner type; see [smart pointers](#Rr-smartptrconcepts)
* `Shared_ptr`   // A type that matches `Pointer`, has copy, and matches the Lifetime profile criteria for a `shared` owner type; see [smart pointers](#Rr-smartptrconcepts)
* `EqualityComparable`   // ???Must we suffer CaMelcAse???
* `Convertible`
* `Common`
* `Boolean`
* `Integral`
* `SignedIntegral`
* `SemiRegular` // ??? Copyable?
* `Regular`
* `TotallyOrdered`
* `Function`
* `RegularFunction`
* `Predicate`
* `Relation`
* ...

### <a name="SS-gsl-smartptrconcepts"></a>Smart pointer concepts

Described in [Lifetimes paper](https://github.com/isocpp/CppCoreGuidelines/blob/master/docs/Lifetimes%20I%20and%20II%20-%20v0.9.1.pdf).

# <a name="S-naming"></a>NL: Naming and layout rules

Consistent naming and layout are helpful.
If for no other reason because it minimizes "my style is better than your style" arguments.
However, there are many, many, different styles around and people are passionate about them (pro and con).
Also, most real-world projects includes code from many sources, so standardizing on a single style for all code is often impossible.
We present a set of rules that you might use if you have no better ideas, but the real aim is consistency, rather than any particular rule set.
IDEs and tools can help (as well as hinder).

Naming and layout rules:

* [NL 1: Don't say in comments what can be clearly stated in code](#Rl-comments)
* [NL.2: State intent in comments](#Rl-comments-intent)
* [NL.3: Keep comments crisp](#Rl-comments-crisp)
* [NL.4: Maintain a consistent indentation style](#Rl-indent)
* [NL.5: Don't encode type information in names](#Rl-name-type)
* [NL.7: Make the length of a name roughly proportional to the length of its scope](#Rl-name-length)
* [NL.8: Use a consistent naming style](#Rl-name)
* [NL 9: Use `ALL_CAPS` for macro names only](#Rl-all-caps)
* [NL.10: Avoid CamelCase](#Rl-camel)
* [NL.15: Use spaces sparingly](#Rl-space)
* [NL.16: Use a conventional class member declaration order](#Rl-order)
* [NL.17: Use K&R-derived layout](#Rl-knr)
* [NL.18: Use C++-style declarator layout](#Rl-ptr)
* [NL.19: Avoid names that are easily misread](#Rl-misread)
* [NL.20: Don't place two statements on the same line](#Rl-stmt)
* [NL.21: Declare one name (only) per declaration](#Rl-dcl)
* [NL.25: Don't use `void` as an argument type](#Rl-void)
* [NL.26: Use conventional `const` notation](#Rl-const)

Most of these rules are aesthetic and programmers hold strong opinions.
IDEs also tend to have defaults and a range of alternatives.
These rules are suggested defaults to follow unless you have reasons not to.

We have had comments to the effect that naming and layout are so personal and/or arbitrary that we should not try to "legislate" them.
We are not "legislating" (see the previous paragraph).
However, we have had many requests for a set of naming and layout conventions to use when there are no external constraints.

More specific and detailed rules are easier to enforce.

These rules bear a strong resemblance to the recommendations in the [PPP Style Guide](http://www.stroustrup.com/Programming/PPP-style.pdf)
written in support of Stroustrup's [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html).

### <a name="Rl-comments"></a>NL.1: Don't say in comments what can be clearly stated in code

##### Reason

Compilers do not read comments.
Comments are less precise than code.
Comments are not updated as consistently as code.

##### Example, bad

    auto x = m * v1 + vv;   // multiply m with v1 and add the result to vv

##### Enforcement

Build an AI program that interprets colloquial English text and see if what is said could be better expressed in C++.

### <a name="Rl-comments-intent"></a>NL.2: State intent in comments

##### Reason

Code says what is done, not what is supposed to be done. Often intent can be stated more clearly and concisely than the implementation.

##### Example

    void stable_sort(Sortable& c)
        // sort c in the order determined by <, keep equal elements (as defined by ==) in
        // their original relative order
    {
        // ... quite a few lines of non-trivial code ...
    }

##### Note

If the comment and the code disagrees, both are likely to be wrong.

### <a name="Rl-comments-crisp"></a>NL.3: Keep comments crisp

##### Reason

Verbosity slows down understanding and makes the code harder to read by spreading it around in the source file.

##### Note

Use intelligible English.
I may be fluent in Danish, but most programmers are not; the maintainers of my code may not be.
Avoid SMS lingo and watch your grammar, punctuation, and capitalization.
Aim for professionalism, not "cool."

##### Enforcement

not possible.

### <a name="Rl-indent"></a>NL.4: Maintain a consistent indentation style

##### Reason

Readability. Avoidance of "silly mistakes."

##### Example, bad

    int i;
    for (i = 0; i < max; ++i); // bug waiting to happen
    if (i == j)
        return i;

##### Note

Always indenting the statement after `if (...)`, `for (...)`, and `while (...)` is usually a good idea:

    if (i < 0) error("negative argument");

    if (i < 0)
        error("negative argument");

##### Enforcement

Use a tool.

### <a name="Rl-name-type"></a>NL.5 Don't encode type information in names

##### Rationale

If names reflects type rather than functionality, it becomes hard to change the types used to provide that functionality.
Also, if the type of a variable is changed, code using it will have to be modified.
Minimize unintentional conversions.

##### Example, bad

    void print_int(int i);
    void print_string(const char*);

    print_int(1);   // OK
    print_int(x);   // conversion to int if x is a double

##### Note

Names with types encoded are either verbose or cryptic.

    printS  // print a std::string
    prints  // print a C-style string
    printi  // print an int

PS. Hungarian notation is evil (at least in a strongly statically-typed language).

##### Note

Some styles distinguishes members from local variable, and/or from global variable.

    struct S {
        int m_;
        S(int m) :m_{abs(m)} { }
    };

This is not evil.

##### Note

Like C++, some styles distinguishes types from non-types.
For example, by capitalizing type names, but not the names of functions and variables.

    typename<typename T>
    class Hash_tbl {   // maps string to T
        // ...
    };

    Hash_tbl<int> index;

This is not evil.

### <a name="Rl-name-length"></a>NL.7: Make the length of a name roughly proportional to the length of its scope

**Rationale**: The larger the scope the greater the chance of confusion and of an unintended name clash.

##### Example

    double sqrt(double x);   // return the square root of x; x must be non-negative

    int length(const char* p);  // return the number of characters in a zero-terminated C-style string

    int length_of_string(const char zero_terminated_array_of_char[])    // bad: verbose

    int g;      // bad: global variable with a cryptic name

    int open;   // bad: global variable with a short, popular name

The use of `p` for pointer and `x` for a floating-point variable is conventional and non-confusing in a restricted scope.

##### Enforcement

???

### <a name="Rl-name"></a>NL.8: Use a consistent naming style

**Rationale**: Consistence in naming and naming style increases readability.

##### Note

Where are many styles and when you use multiple libraries, you can't follow all their differences conventions.
Choose a "house style", but leave "imported" libraries with their original style.

##### Example

ISO Standard, use lower case only and digits, separate words with underscores:

* `int`
* `vector`
* `my_map`

Avoid double underscores `__`.

##### Example

[Stroustrup](http://www.stroustrup.com/Programming/PPP-style.pdf):
ISO Standard, but with upper case used for your own types and concepts:

* `int`
* `vector`
* `My_map`

##### Example

CamelCase: capitalize each word in a multi-word identifier:

* `int`
* `vector`
* `MyMap`
* `myMap`

Some conventions capitalize the first letter, some don't.

##### Note

Try to be consistent in your use of acronyms and lengths of identifiers:

    int mtbf {12};
    int mean_time_between_failures {12}; // make up your mind

##### Enforcement

Would be possible except for the use of libraries with varying conventions.

### <a name="Rl-all-caps"></a>NL 9: Use `ALL_CAPS` for macro names only

##### Reason

To avoid confusing macros from names that obeys scope and type rules.

##### Example

    void f()
    {
        const int SIZE{1000};  // Bad, use 'size' instead
        int v[SIZE];
    }

##### Note

This rule applies to non-macro symbolic constants:

    enum bad { BAD, WORSE, HORRIBLE }; // BAD

##### Enforcement

* Flag macros with lower-case letters
* Flag `ALL_CAPS` non-macro names

### <a name="Rl-camel"></a>NL.10: Avoid CamelCase

##### Reason

The use of underscores to separate parts of a name is the original C and C++ style and used in the C++ standard library.
If you prefer CamelCase, you have to choose among different flavors of camelCase.

##### Note

This rule is a default to use only if you have a choice.
Often, you don't have a choice and must follow an established style for [consistency](#Rl-name).
The need for consistency beats personal taste.

##### Example

[Stroustrup](http://www.stroustrup.com/Programming/PPP-style.pdf):
ISO Standard, but with upper case used for your own types and concepts:

* `int`
* `vector`
* `My_map`

##### Enforcement

Impossible.

### <a name="Rl-space"></a>NL.15: Use spaces sparingly

##### Reason

Too much space makes the text larger and distracts.

##### Example, bad

    #include < map >

    int main(int argc, char * argv [ ])
    {
        // ...
    }

##### Example

    #include<map>

    int main(int argc, char* argv[])
    {
        // ...
    }

##### Note

Some IDEs have their own opinions and add distracting space.

##### Note

We value well-placed whitespace as a significant help for readability. Just don't overdo it.

### <a name="Rl-order"></a>NL.16: Use a conventional class member declaration order

##### Reason

A conventional order of members improves readability.

When declaring a class use the following order

* types: classes, enums, and aliases (`using`)
* constructors, assignments, destructor
* functions
* data

Use the `public` before `protected` before `private` order.

Private types and functions can be placed with private data.

Avoid multiple blocks of declarations of one access (e.g., `public`) dispersed among blocks of declarations with different access (e.g. `private`).

##### Example

    class X {
    public:
        // interface
    protected:
        // unchecked function for use by derived class implementations
    private:
        // implementation details
    };

##### Note

The use of macros to declare groups of members often violates any ordering rules.
However, macros obscures what is being expressed anyway.

##### Enforcement

Flag departures from the suggested order. There will be a lot of old code that doesn't follow this rule.

### <a name="Rl-knr"></a>NL.17: Use K&R-derived layout

##### Reason

This is the original C and C++ layout. It preserves vertical space well. It distinguishes different language constructs (such as functions and classes) well.

##### Note

In the context of C++, this style is often called "Stroustrup".

##### Example

    struct Cable {
        int x;
        // ...
    };

    double foo(int x)
    {
        if (0 < x) {
            // ...
        }

        switch (x) {
            case 0:
                // ...
                break;
            case amazing:
                // ...
                break;
            default:
                // ...
                break;
        }

        if (0 < x)
            ++x;

        if (x < 0)
            something();
        else
            something_else();

        return some_value;
    }

Note the space between `if` and `(`

##### Note

Use separate lines for each statement, the branches of an `if`, and the body of a `for`.

##### Note

The `{` for a `class` and a `struct` in *not* on a separate line, but the `{` for a function is.

##### Note

Capitalize the names of your user-defined types to distinguish them from standards-library types.

##### Note

Do not capitalize function names.

##### Enforcement

If you want enforcement, use an IDE to reformat.

### <a name="Rl-ptr"></a>NL.18: Use C++-style declarator layout

##### Reason

The C-style layout emphasizes use in expressions and grammar, whereas the C++-style emphasizes types.
The use in expressions argument doesn't hold for references.

##### Example

    T& operator[](size_t);   // OK
    T &operator[](size_t);   // just strange
    T & operator[](size_t);   // undecided

##### Enforcement

Impossible in the face of history.


### <a name="Rl-misread"></a>NL.19: Avoid names that are easily misread

##### Reason

Readability.
Not everyone has screens and printers that makes it easy to distinguish all characters.
We easily confuse similarly spelled and slightly misspelled words.

##### Example

    int oO01lL = 6; // bad

    int splunk = 7;
    int splonk = 8; // bad: splunk and splonk are easily confused

##### Enforcement

???

### <a name="Rl-stmt"></a>NL.20: Don't place two statements on the same line

##### Reason

Readability.
It is really easy to overlook a statement when there is more on a line.

##### Example

    int x = 7; char* p = 29;    // don't
    int x = 7; f(x);  ++x;      // don't

##### Enforcement

Easy.

### <a name="Rl-dcl"></a>NL.21: Declare one name (only) per declaration

##### Reason

Readability.
Minimizing confusion with the declarator syntax.

##### Note

For details, see [ES.10](#Res-name-one).


### <a name="Rl-void"></a>NL.25: Don't use `void` as an argument type

##### Reason

It's verbose and only needed where C compatibility matters.

##### Example

    void f(void);   // bad

    void g();       // better

##### Note

Even Dennis Ritchie deemed `void f(void)` an abomination.
You can make an argument for that abomination in C when function prototypes were rare so that banning:

    int f();
    f(1, 2, "weird but valid C89");   // hope that f() is defined int f(a, b, c) char* c; { /* ... */ }

would have caused major problems, but not in the 21st century and in C++.

### <a name="Rl-const"></a>NL.26: Use conventional `const` notation

##### Reason

Conventional notation is more familiar to more programmers.
Consistency in large code bases.

##### Example

    const int x = 7;    // OK
    int const y = 9;    // bad

    const int *const p = nullptr;   // OK, constant pointer to constant int
    int const *const p = nullptr;   // bad, constant pointer to constant int

##### Note

We are well aware that you could claim the "bad" examples more logical than the ones marked "OK",
but they also confuse more people, especially novices relying on teaching material using the far more common, conventional OK style.

As ever, remember that the aim of these naming and layout rules is consistency and that aesthetics vary immensely.

##### Enforcement

Flag `const` used as a suffix for a type.

# <a name="S-faq"></a>FAQ: Answers to frequently asked questions

This section covers answers to frequently asked questions about these guidelines.

### <a name="Faq-aims"></a>FAQ.1: What do these guidelines aim to achieve?

See the <a href="#S-abstract">top of this page</a>. This is an open source project to maintain modern authoritative guidelines for writing C++ code using the current C++ Standard (as of this writing, C++14). The guidelines are designed to be modern, machine-enforceable wherever possible, and open to contributions and forking so that organizations can easily incorporate them into their own corporate coding guidelines.

### <a name="Faq-announced"></a>FAQ.2: When and where was this work first announced?

It was announced by [Bjarne Stroustrup in his CppCon 2015 opening keynote, "Writing Good C++14"](https://isocpp.org/blog/2015/09/stroustrup-cppcon15-keynote). See also the [accompanying isocpp.org blog post](https://isocpp.org/blog/2015/09/bjarne-stroustrup-announces-cpp-core-guidelines), and for the rationale of the type and memory safety guidelines see [Herb Sutter's follow-up CppCon 2015 talk, "Writing Good C++14 ... By Default"](https://isocpp.org/blog/2015/09/sutter-cppcon15-day2plenary).

### <a name="Faq-maintainers"></a>FAQ.3: Who are the authors and maintainers of these guidelines?

The initial primary authors and maintainers are Bjarne Stroustrup and Herb Sutter, and the guidelines so far were developed with contributions from experts at CERN, Microsoft, Morgan Stanley, and several other organizations. At the time of their release, the guidelines are in a "0.6" state, and contributions are welcome. As Stroustrup said in his announcement: "We need help!"

### <a name="Faq-contribute"></a>FAQ.4: How can I contribute?

See [CONTRIBUTING.md](https://github.com/isocpp/CppCoreGuidelines/blob/master/CONTRIBUTING.md). We appreciate volunteer help!

### <a name="Faq-maintainer"></a>FAQ.5: How can I become an editor/maintainer?

By contributing a lot first and having the consistent quality of your contributions recognized. See [CONTRIBUTING.md](https://github.com/isocpp/CppCoreGuidelines/blob/master/CONTRIBUTING.md). We appreciate volunteer help!

### <a name="Faq-iso"></a>FAQ.6: Have these guidelines been approved by the ISO C++ standards committee? Do they represent the consensus of the committee?

No. These guidelines are outside the standard. They are intended to serve the standard, and be maintained as current guidelines about how to use the current Standard C++ effectively. We aim to keep them in sync with the standard as that is evolved by the committee.

### <a name="Faq-isocpp"></a>FAQ.7: If these guidelines are not approved by the committee, why are they under `github.com/isocpp`?

Because `isocpp` is the Standard C++ Foundation; the committee's repositories are under [github.com/*cplusplus*](https://github.com/cplusplus). Some neutral organization has to own the copyright and license to make it clear this is not being dominated by any one person or vendor. The natural entity is the Foundation, which exists to promote the use and up-to-date understanding of modern Standard C++ and the work of the committee. This follows the same pattern that isocpp.org did for the [C++ FAQ](https://isocpp.org/faq), which was initially the work of Bjarne Stroustrup, Marshall Cline, and Herb Sutter and contributed to the open project in the same way.

### <a name="Faq-cpp98"></a>FAQ.8: Will there be a C++98 version of these Guidelines? a C++11 version?

No. These guidelines are about how to best use Standard C++14 (and, if you have an implementation available, the Concepts Lite Technical Specification) and write code assuming you have a modern conforming compiler.

### <a name="Faq-language-extensions"></a>FAQ.9: Do these guidelines propose new language features?

No. These guidelines are about how to best use Standard C++14 + the Concepts Lite Technical Specification, and they limit themselves to recommending only those features.

### <a name="Faq-markdown"></a>FAQ.10: What version of Markdown do these guidelines use?

These coding standards are written using [CommonMark](http://commonmark.org), and `<a>` HTML anchors.

We are considering the following extensions from [GitHub Flavored Markdown (GFM)](https://help.github.com/articles/github-flavored-markdown/):

* fenced code blocks (consistently using indented vs. fenced is under discussion)
* tables (none yet but we'll likely need them, and this is a GFM extension)

Avoid other HTML tags and other extensions.

Note: We are not yet consistent with this style.

### <a name="Faq-gsl"></a>FAQ.50: What is the GSL (guideline support library)?

The GSL is the small set of types and aliases specified in these guidelines. As of this writing, their specification herein is too sparse; we plan to add a WG21-style interface specification to ensure that different implementations agree, and to propose as a contribution for possible standardization, subject as usual to whatever the committee decides to accept/improve/alter/reject.

### <a name="Faq-msgsl"></a>FAQ.51: Is [github.com/Microsoft/GSL](https://github.com/Microsoft/GSL) the GSL?

No. That is just a first implementation contributed by Microsoft. Other implementations by other vendors are encouraged, as are forks of and contributions to that implementation. As of this writing one week into the public project, at least one GPLv3 open source implementation already exists. We plan to produce a WG21-style interface specification to ensure that different implementations agree.

### <a name="Faq-gsl-implementation"></a>FAQ.52: Why not supply an actual GSL implementation in/with these guidelines?

We are reluctant to bless one particular implementation because we do not want to make people think there is only one, and inadvertently stifle parallel implementations. And if these guidelines included an actual implementation, then whoever contributed it could be mistakenly seen as too influential. We prefer to follow the long-standing approach of the committee, namely to specify interfaces, not implementations. But at the same time we want at least one implementation available; we hope for many.

### <a name="Faq-boost"></a>FAQ.53: Why weren't the GSL types proposed through Boost?

Because we want to use them immediately, and because they are temporary in that we want to retire them as soon as types that fill the same needs exist in the standard library.

### <a name="Faq-gsl-iso"></a>FAQ.54: Has the GSL (guideline support library) been approved by the ISO C++ standards committee?

No. The GSL exists only to supply a few types and aliases that are not currently in the standard library. If the committee decides on standardized versions (of these or other types that fill the same need) then they can be removed from the GSL.

### <a name="Faq-gsl-string-view"></a>FAQ.55: If you're using the standard types where available, why is the GSL `string_span` different from the `string_view` in the Library Fundamentals 1 Technical Specification and C++17 Working Paper? Why not just use the committee-approved `string_view`?

The consensus on the taxonomy of views for the C++ standard library was that "view" means "read-only", and "span" means "read/write". The read-only `string_view` was the first such component to complete the standardization process, while `span` and `string_span` are currently being considered for standardization.

### <a name="Faq-gsl-owner"></a>FAQ.56: Is `owner` the same as the proposed `observer_ptr`?

No. `owner` owns, is an alias, and can be applied to any indirection type. The main intent of `observer_ptr` is to signify a *non*-owning pointer.

### <a name="Faq-gsl-stack-array"></a>FAQ.57: Is `stack_array` the same as the standard `array`?

No. `stack_array` is guaranteed to be allocated on the stack. Although a `std::array` contains its storage directly inside itself, the `array` object can be put anywhere, including the heap.

### <a name="Faq-gsl-dyn-array"></a>FAQ.58: Is `dyn_array` the same as `vector` or the proposed `dynarray`?

No. `dyn_array` is not resizable, and is a safe way to refer to a heap-allocated fixed-size array. Unlike `vector`, it is intended to replace array-`new[]`. Unlike the `dynarray` that has been proposed in the committee, this does not anticipate compiler/language magic to somehow allocate it on the stack when it is a member of an object that is allocated on the stack; it simply refers to a "dynamic" or heap-based array.

### <a name="Faq-gsl-expects"></a>FAQ.59: Is `Expects` the same as `assert`?

No. It is a placeholder for language support for contract preconditions.

### <a name="Faq-gsl-ensures"></a>FAQ.60: Is `Ensures` the same as `assert`?

No. It is a placeholder for language support for contract postconditions.

# <a name="S-libraries"></a>부록 A: 라이브러리

이 절에서는 추천할만한 라이브러리를 나열하고 명시적으로 몇 가지 라이브러리를 추천하고자 한다.

??? 일반적인 가이드로 적합할까? 나는 그렇게 생각하지 않는다 ???

# <a name="S-modernizing"></a>Appendix B: Modernizing code

Ideally, we follow all rules in all code.
Realistically, we have to deal with a lot of old code:

* application code written before the guidelines were formulated or known
* libraries written to older/different standards
* code written under "unusual" constraints
* code that we just haven't gotten around to modernizing

If we have a million lines of new code, the idea of "just changing it all at once" is typically unrealistic.
Thus, we need a way of gradually modernizing a code base.

Upgrading older code to modern style can be a daunting task.
Often, the old code is both a mess (hard to understand) and working correctly (for the current range of uses).
Typically, the original programmer is not around and the test cases incomplete.
The fact that the code is a mess dramatically increases the effort needed to make any change and the risk of introducing errors.
Often, messy old code runs unnecessarily slowly because it requires outdated compilers and cannot take advantage of modern hardware.
In many cases, automated "modernizer"-style tool support would be required for major upgrade efforts.

The purpose of modernizing code is to simplify adding new functionality, to ease maintenance, and to increase performance (throughput or latency), and to better utilize modern hardware.
Making code "look pretty" or "follow modern style" are not by themselves reasons for change.
There are risks implied by every change and costs (including the cost of lost opportunities) implied by having an outdated code base.
The cost reductions must outweigh the risks.

But how?

There is no one approach to modernizing code.
How best to do it depends on the code, the pressure for updates, the backgrounds of the developers, and the available tool.
Here are some (very general) ideas:

* The ideal is "just upgrade everything." That gives the most benefits for the shortest total time.
  In most circumstances, it is also impossible.
* We could convert a code base module for module, but any rules that affects interfaces (especially ABIs), such as [use `span`](#SS-views), cannot be done on a per-module basis.
* We could convert code "bottom up" starting with the rules we estimate will give the greatest benefits and/or the least trouble in a given code base.
* We could start by focusing on the interfaces, e.g., make sure that no resources are lost and no pointer is misused.
  This would be a set of changes across the whole code base, but would most likely have huge benefits.
  Afterwards, code hidden behind those interfaces can be gradually modernized without affecting other code.

Whichever way you choose, please note that the most advantages come with the highest conformance to the guidelines.
The guidelines are not a random set of unrelated rules where you can randomly pick and choose with an expectation of success.

We would dearly love to hear about experience and about tools used.
Modernization can be much faster, simpler, and safer when supported with analysis tools and even code transformation tools.

# <a name="S-discussion"></a>Appendix C: Discussion

This section contains follow-up material on rules and sets of rules.
In particular, here we present further rationale, longer examples, and discussions of alternatives.

### <a name="Sd-order"></a>Discussion: Define and initialize member variables in the order of member declaration

Member variables are always initialized in the order they are declared in the class definition, so write them in that order in the constructor initialization list. Writing them in a different order just makes the code confusing because it won't run in the order you see, and that can make it hard to see order-dependent bugs.

    class Employee {
        string email, first, last;
    public:
        Employee(const char* firstName, const char* lastName);
        // ...
    };

    Employee::Employee(const char* firstName, const char* lastName)
      : first(firstName),
        last(lastName),
        // BAD: first and last not yet constructed
        email(first + "." + last + "@acme.com")
    {}

In this example, `email` will be constructed before `first` and `last` because it is declared first. That means its constructor will attempt to use `first` and `last` too soon -- not just before they are set to the desired values, but before they are constructed at all.

If the class definition and the constructor body are in separate files, the long-distance influence that the order of member variable declarations has over the constructor's correctness will be even harder to spot.

**References**:

[\[Cline99\]](#Cline99) §22.03-11, [\[Dewhurst03\]](Dewhurst03) §52-53, [\[Koenig97\]](#Koenig97) §4, [\[Lakos96\]](#Lakos96) §10.3.5, [\[Meyers97\]](#Meyers97) §13, [\[Murray93\]](#Murray93) §2.1.3, [\[Sutter00\]](#Sutter00) §47

### <a name="TBD"></a>Use of `=`, `{}`, and `()` as initializers

???

### <a name="Sd-factory"></a>Discussion: Use a factory function if you need "virtual behavior" during initialization

If your design wants virtual dispatch into a derived class from a base class constructor or destructor for functions like `f` and `g`, you need other techniques, such as a post-constructor -- a separate member function the caller must invoke to complete initialization, which can safely call `f` and `g` because in member functions virtual calls behave normally. Some techniques for this are shown in the References. Here's a non-exhaustive list of options:

* *Pass the buck:* Just document that user code must call the post-initialization function right after constructing an object.
* *Post-initialize lazily:* Do it during the first call of a member function. A Boolean flag in the base class tells whether or not post-construction has taken place yet.
* *Use virtual base class semantics:* Language rules dictate that the constructor most-derived class decides which base constructor will be invoked; you can use that to your advantage. (See [\[Taligent94\]](#Taligent94).)
* *Use a factory function:* This way, you can easily force a mandatory invocation of a post-constructor function.

Here is an example of the last option:

    class B {
    public:
        B() { /* ... */ f(); /* ... */ }   // BAD: see Item 49.1

        virtual void f() = 0;

        // ...
    };

    class B {
    protected:
        B() { /* ... */ }
        virtual void PostInitialize()    // called right after construction
            { /* ... */ f(); /* ... */ }   // GOOD: virtual dispatch is safe
    public:
        virtual void f() = 0;

        template<class T>
        static shared_ptr<T> Create()    // interface for creating objects
        {
            auto p = make_shared<T>();
            p->PostInitialize();
            return p;
        }
    };


    class D : public B {                 // some derived class
    public:
        void f() override { /* ...  */ };

    protected:
        D() {}

        template<class T>
        friend shared_ptr<T> B::Create();
    };

    shared_ptr<D> p = D::Create<D>();    // creating a D object

This design requires the following discipline:

* Derived classes such as `D` must not expose a public constructor. Otherwise, `D`'s users could create `D` objects that don't invoke `PostInitialize`.
* Allocation is limited to `operator new`. `B` can, however, override `new` (see Items 45 and 46).
* `D` must define a constructor with the same parameters that `B` selected. Defining several overloads of `Create` can assuage this problem, however; and the overloads can even be templated on the argument types.

If the requirements above are met, the design guarantees that `PostInitialize` has been called for any fully constructed `B`-derived object. `PostInitialize` doesn't need to be virtual; it can, however, invoke virtual functions freely.

In summary, no post-construction technique is perfect. The worst techniques dodge the whole issue by simply asking the caller to invoke the post-constructor manually. Even the best require a different syntax for constructing objects (easy to check at compile time) and/or cooperation from derived class authors (impossible to check at compile time).

**References**: [\[Alexandrescu01\]](#Alexandrescu01) §3, [\[Boost\]](#Boost), [\[Dewhurst03\]](#Dewhurst03) §75, [\[Meyers97\]](#Meyers97) §46, [\[Stroustrup00\]](#Stroustrup00) §15.4.3, [\[Taligent94\]](#Taligent94)

### <a name="Sd-dtor"></a>Discussion: Make base class destructors public and virtual, or protected and nonvirtual

Should destruction behave virtually? That is, should destruction through a pointer to a `base` class be allowed? If yes, then `base`'s destructor must be public in order to be callable, and virtual otherwise calling it results in undefined behavior. Otherwise, it should be protected so that only derived classes can invoke it in their own destructors, and nonvirtual since it doesn't need to behave virtually virtual.

##### Example

The common case for a base class is that it's intended to have publicly derived classes, and so calling code is just about sure to use something like a `shared_ptr<base>`:

    class Base {
    public:
        ~Base();                   // BAD, not virtual
        virtual ~Base();           // GOOD
        // ...
    };

    class Derived : public Base { /* ... */ };

    {
        unique_ptr<Base> pb = make_unique<Derived>();
        // ...
    } // ~pb invokes correct destructor only when ~Base is virtual

In rarer cases, such as policy classes, the class is used as a base class for convenience, not for polymorphic behavior. It is recommended to make those destructors protected and nonvirtual:

    class My_policy {
    public:
        virtual ~My_policy();      // BAD, public and virtual
    protected:
        ~My_policy();              // GOOD
        // ...
    };

    template<class Policy>
    class customizable : Policy { /* ... */ }; // note: private inheritance

##### Note

This simple guideline illustrates a subtle issue and reflects modern uses of inheritance and object-oriented design principles.

For a base class `Base`, calling code might try to destroy derived objects through pointers to `Base`, such as when using a `unique_ptr<Base>`. If `Base`'s destructor is public and nonvirtual (the default), it can be accidentally called on a pointer that actually points to a derived object, in which case the behavior of the attempted deletion is undefined. This state of affairs has led older coding standards to impose a blanket requirement that all base class destructors must be virtual. This is overkill (even if it is the common case); instead, the rule should be to make base class destructors virtual if and only if they are public.

To write a base class is to define an abstraction (see Items 35 through 37). Recall that for each member function participating in that abstraction, you need to decide:

* Whether it should behave virtually or not.
* Whether it should be publicly available to all callers using a pointer to `Base` or else be a hidden internal implementation detail.

As described in Item 39, for a normal member function, the choice is between allowing it to be called via a pointer to `Base` nonvirtually (but possibly with virtual behavior if it invokes virtual functions, such as in the NVI or Template Method patterns), virtually, or not at all. The NVI pattern is a technique to avoid public virtual functions.

Destruction can be viewed as just another operation, albeit with special semantics that make nonvirtual calls dangerous or wrong. For a base class destructor, therefore, the choice is between allowing it to be called via a pointer to `Base` virtually or not at all; "nonvirtually" is not an option. Hence, a base class destructor is virtual if it can be called (i.e., is public), and nonvirtual otherwise.

Note that the NVI pattern cannot be applied to the destructor because constructors and destructors cannot make deep virtual calls. (See Items 39 and 55.)

Corollary: When writing a base class, always write a destructor explicitly, because the implicitly generated one is public and nonvirtual. You can always `=default` the implementation if the default body is fine and you're just writing the function to give it the proper visibility and virtuality.

##### Exception

Some component architectures (e.g., COM and CORBA) don't use a standard deletion mechanism, and foster different protocols for object disposal. Follow the local patterns and idioms, and adapt this guideline as appropriate.

Consider also this rare case:

* `B` is both a base class and a concrete class that can be instantiated by itself, and so the destructor must be public for `B` objects to be created and destroyed.
* Yet `B` also has no virtual functions and is not meant to be used polymorphically, and so although the destructor is public it does not need to be virtual.

Then, even though the destructor has to be public, there can be great pressure to not make it virtual because as the first virtual function it would incur all the run-time type overhead when the added functionality should never be needed.

In this rare case, you could make the destructor public and nonvirtual but clearly document that further-derived objects must not be used polymorphically as `B`'s. This is what was done with `std::unary_function`.

In general, however, avoid concrete base classes (see Item 35). For example, `unary_function` is a bundle-of-typedefs that was never intended to be instantiated standalone. It really makes no sense to give it a public destructor; a better design would be to follow this Item's advice and give it a protected nonvirtual destructor.

**References**: [\[C++CS\]](#C++CS) Item 50, [\[Cargill92\]](#Cargill92) pp. 77-79, 207, [\[Cline99\]](#Cline99) §21.06, 21.12-13, [\[Henricson97\]](#Henricson97) pp. 110-114, [\[Koenig97\]](#Koenig97) Chapters 4, 11, [\[Meyers97\]](#Meyers97) §14, [\[Stroustrup00\]](#Stroustrup00) §12.4.2, [\[Sutter02\]](#Sutter02) §27, [\[Sutter04\]](#Sutter04) §18

### <a name="Sd-noexcept"></a>Discussion: Usage of noexcept

???

### <a name="Sd-never-fail"></a>Discussion: Destructors, deallocation, and swap must never fail

Never allow an error to be reported from a destructor, a resource deallocation function (e.g., `operator delete`), or a `swap` function using `throw`. It is nearly impossible to write useful code if these operations can fail, and even if something does go wrong it nearly never makes any sense to retry. Specifically, types whose destructors may throw an exception are flatly forbidden from use with the C++ standard library. Most destructors are now implicitly `noexcept` by default.

##### Example

    class Nefarious {
    public:
        Nefarious()  { /* code that could throw */ }   // ok
        ~Nefarious() { /* code that could throw */ }   // BAD, should not throw
        // ...
    };

1. `Nefarious` objects are hard to use safely even as local variables:


        void test(string& s)
        {
            Nefarious n;          // trouble brewing
            string copy = s;      // copy the string
        } // destroy copy and then n

    Here, copying `s` could throw, and if that throws and if `n`'s destructor then also throws, the program will exit via `std::terminate` because two exceptions can't be propagated simultaneously.

2. Classes with `Nefarious` members or bases are also hard to use safely, because their destructors must invoke `Nefarious`' destructor, and are similarly poisoned by its poor behavior:


        class Innocent_bystander {
            Nefarious member;     // oops, poisons the enclosing class's destructor
            // ...
        };

        void test(string& s)
        {
            Innocent_bystander i; // more trouble brewing
            string copy2 = s;      // copy the string
        } // destroy copy and then i

    Here, if constructing `copy2` throws, we have the same problem because `i`'s destructor now also can throw, and if so we'll invoke `std::terminate`.

3. You can't reliably create global or static `Nefarious` objects either:


        static Nefarious n;       // oops, any destructor exception can't be caught

4. You can't reliably create arrays of `Nefarious`:


        void test()
        {
            std::array<Nefarious, 10> arr; // this line can std::terminate(!)
        }

    The behavior of arrays is undefined in the presence of destructors that throw because there is no reasonable rollback behavior that could ever be devised. Just think: What code can the compiler generate for constructing an `arr` where, if the fourth object's constructor throws, the code has to give up and in its cleanup mode tries to call the destructors of the already-constructed objects ... and one or more of those destructors throws? There is no satisfactory answer.

5. You can't use `Nefarious` objects in standard containers:


        std::vector<Nefarious> vec(10);   // this line can std::terminate()

    The standard library forbids all destructors used with it from throwing. You can't store `Nefarious` objects in standard containers or use them with any other part of the standard library.

##### Note

These are key functions that must not fail because they are necessary for the two key operations in transactional programming: to back out work if problems are encountered during processing, and to commit work if no problems occur. If there's no way to safely back out using no-fail operations, then no-fail rollback is impossible to implement. If there's no way to safely commit state changes using a no-fail operation (notably, but not limited to, `swap`), then no-fail commit is impossible to implement.

Consider the following advice and requirements found in the C++ Standard:

> If a destructor called during stack unwinding exits with an exception, terminate is called (15.5.1). So destructors should generally catch exceptions and not let them propagate out of the destructor. --[\[C++03\]](#C++03) §15.2(3)
>
> No destructor operation defined in the C++ Standard Library (including the destructor of any type that is used to instantiate a standard library template) will throw an exception. --[\[C++03\]](#C++03) §17.4.4.8(3)

Deallocation functions, including specifically overloaded `operator delete` and `operator delete[]`, fall into the same category, because they too are used during cleanup in general, and during exception handling in particular, to back out of partial work that needs to be undone.
Besides destructors and deallocation functions, common error-safety techniques rely also on `swap` operations never failing -- in this case, not because they are used to implement a guaranteed rollback, but because they are used to implement a guaranteed commit. For example, here is an idiomatic implementation of `operator=` for a type `T` that performs copy construction followed by a call to a no-fail `swap`:

    T& T::operator=(const T& other) {
        auto temp = other;
        swap(temp);
    }

(See also Item 56. ???)

Fortunately, when releasing a resource, the scope for failure is definitely smaller. If using exceptions as the error reporting mechanism, make sure such functions handle all exceptions and other errors that their internal processing might generate. (For exceptions, simply wrap everything sensitive that your destructor does in a `try/catch(...)` block.) This is particularly important because a destructor might be called in a crisis situation, such as failure to allocate a system resource (e.g., memory, files, locks, ports, windows, or other system objects).

When using exceptions as your error handling mechanism, always document this behavior by declaring these functions `noexcept`. (See Item 75.)

**References**: [\[C++CS\]](#C++CS) Item 51; [\[C++03\]](#C++03) §15.2(3), §17.4.4.8(3), [\[Meyers96\]](#Meyers96) §11, [\[Stroustrup00\]](#Stroustrup00) §14.4.7, §E.2-4, [\[Sutter00\]](#Sutter00) §8, §16, [\[Sutter02\]](#Sutter02) §18-19

## <a name="Sd-consistent"></a>Define Copy, move, and destroy consistently

##### Reason

 ???

##### Note

If you define a copy constructor, you must also define a copy assignment operator.

##### Note

If you define a move constructor, you must also define a move assignment operator.

##### Example

    class X {
        // ...
    public:
        X(const X&) { /* stuff */ }

        // BAD: failed to also define a copy assignment operator

        X(x&&) { /* stuff */ }

        // BAD: failed to also define a move assignment operator
    };

    X x1;
    X x2 = x1; // ok
    x2 = x1;   // pitfall: either fails to compile, or does something suspicious

If you define a destructor, you should not use the compiler-generated copy or move operation; you probably need to define or suppress copy and/or move.

    class X {
        HANDLE hnd;
        // ...
    public:
        ~X() { /* custom stuff, such as closing hnd */ }
        // suspicious: no mention of copying or moving -- what happens to hnd?
    };

    X x1;
    X x2 = x1; // pitfall: either fails to compile, or does something suspicious
    x2 = x1;   // pitfall: either fails to compile, or does something suspicious

If you define copying, and any base or member has a type that defines a move operation, you should also define a move operation.

    class X {
        string s; // defines more efficient move operations
        // ... other data members ...
    public:
        X(const X&) { /* stuff */ }
        X& operator=(const X&) { /* stuff */ }

        // BAD: failed to also define a move construction and move assignment
        // (why wasn't the custom "stuff" repeated here?)
    };

    X test()
    {
        X local;
        // ...
        return local;  // pitfall: will be inefficient and/or do the wrong thing
    }

If you define any of the copy constructor, copy assignment operator, or destructor, you probably should define the others.

##### Note

If you need to define any of these five functions, it means you need it to do more than its default behavior -- and the five are asymmetrically interrelated. Here's how:

* If you write/disable either of the copy constructor or the copy assignment operator, you probably need to do the same for the other: If one does "special" work, probably so should the other because the two functions should have similar effects. (See Item 53, which expands on this point in isolation.)
* If you explicitly write the copying functions, you probably need to write the destructor: If the "special" work in the copy constructor is to allocate or duplicate some resource (e.g., memory, file, socket), you need to deallocate it in the destructor.
* If you explicitly write the destructor, you probably need to explicitly write or disable copying: If you have to write a nontrivial destructor, it's often because you need to manually release a resource that the object held. If so, it is likely that those resources require careful duplication, and then you need to pay attention to the way objects are copied and assigned, or disable copying completely.

In many cases, holding properly encapsulated resources using RAII "owning" objects can eliminate the need to write these operations yourself. (See Item 13.)

Prefer compiler-generated (including `=default`) special members; only these can be classified as "trivial", and at least one major standard library vendor heavily optimizes for classes having trivial special members. This is likely to become common practice.

**Exceptions**: When any of the special functions are declared only to make them nonpublic or virtual, but without special semantics, it doesn't imply that the others are needed.
In rare cases, classes that have members of strange types (such as reference members) are an exception because they have peculiar copy semantics.
In a class holding a reference, you likely need to write the copy constructor and the assignment operator, but the default destructor already does the right thing. (Note that using a reference member is almost always wrong.)

**References**: [\[C++CS\]](#C++CS) Item 52; [\[Cline99\]](#Cline99) §30.01-14, [\[Koenig97\]](#Koenig97) §4, [\[Stroustrup00\]](#Stroustrup00) §5.5, §10.4, [\[SuttHysl04b\]](#SuttHysl04b)

Resource management rule summary:

* [Provide strong resource safety; that is, never leak anything that you think of as a resource](#Cr-safety)
* [Never throw while holding a resource not owned by a handle](#Cr-never)
* [A "raw" pointer or reference is never a resource handle](#Cr-raw)
* [Never let a pointer outlive the object it points to](#Cr-outlive)
* [Use templates to express containers (and other resource handles)](#Cr-templates)
* [Return containers by value (relying on move or copy elision for efficiency)](#Cr-value-return)
* [If a class is a resource handle, it needs a constructor, a destructor, and copy and/or move operations](#Cr-handle)
* [If a class is a container, give it an initializer-list constructor](#Cr-list)

### <a name="Cr-safety"></a>Provide strong resource safety; that is, never leak anything that you think of as a resource

##### Reason

Prevent leaks. Leaks can lead to performance degradation, mysterious error, system crashes, and security violations.

**Alternative formulation**: Have every resource represented as an object of some class managing its lifetime.

##### Example

    template<class T>
    class Vector {
    // ...
    private:
        T* elem;   // sz elements on the free store, owned by the class object
        int sz;
    };

This class is a resource handle. It manages the lifetime of the `T`s. To do so, `Vector` must define or delete [the set of special operations](???) (constructors, a destructor, etc.).

##### Example

    ??? "odd" non-memory resource ???

##### Enforcement

The basic technique for preventing leaks is to have every resource owned by a resource handle with a suitable destructor. A checker can find "naked `new`s". Given a list of C-style allocation functions (e.g., `fopen()`), a checker can also find uses that are not managed by a resource handle. In general, "naked pointers" can be viewed with suspicion, flagged, and/or analyzed. A complete list of resources cannot be generated without human input (the definition of "a resource" is necessarily too general), but a tool can be "parameterized" with a resource list.

### <a name="Cr-never"></a>Never throw while holding a resource not owned by a handle

##### Reason

That would be a leak.

##### Example

    void f(int i)
    {
        FILE* f = fopen("a file", "r");
        ifstream is { "another file" };
        // ...
        if (i == 0) return;
        // ...
        fclose(f);
    }

If `i == 0` the file handle for `a file` is leaked. On the other hand, the `ifstream` for `another file` will correctly close its file (upon destruction). If you must use an explicit pointer, rather than a resource handle with specific semantics, use a `unique_ptr` or a `shared_ptr` with a custom deleter:

    void f(int i)
    {
        unique_ptr<FILE, int(*)(FILE*)> f(fopen("a file", "r"), fclose);
        // ...
        if (i == 0) return;
        // ...
    }

Better:

    void f(int i)
    {
        ifstream input {"a file"};
        // ...
        if (i == 0) return;
        // ...
    }

##### Enforcement

A checker must consider all "naked pointers" suspicious.
A checker probably must rely on a human-provided list of resources.
For starters, we know about the standard-library containers, `string`, and smart pointers.
The use of `span` and `string_span` should help a lot (they are not resource handles).

### <a name="Cr-raw"></a>A "raw" pointer or reference is never a resource handle

##### Reason

To be able to distinguish owners from views.

##### Note

This is independent of how you "spell" pointer: `T*`, `T&`, `Ptr<T>` and `Range<T>` are not owners.

### <a name="Cr-outlive"></a>Never let a pointer outlive the object it points to

##### Reason

To avoid extremely hard-to-find errors. Dereferencing such a pointer is undefined behavior and could lead to violations of the type system.

##### Example

    string* bad()   // really bad
    {
        vector<string> v = { "this", "will", "cause" "trouble" };
        // leaking a pointer into a destroyed member of a destroyed object (v)
        return &v[0];
    }

    void use()
    {
        string* p = bad();
        vector<int> xx = {7, 8, 9};
        // undefined behavior: x may not be "this"
        string x = *p;
        // undefined behavior: we don't know what (if anything) is allocated a location p
        *p = "Evil!";
    }

The `string`s of `v` are destroyed upon exit from `bad()` and so is `v` itself. The returned pointer points to unallocated memory on the free store. This memory (pointed into by `p`) may have been reallocated by the time `*p` is executed. There may be no `string` to read and a write through `p` could easily corrupt objects of unrelated types.

##### Enforcement

Most compilers already warn about simple cases and has the information to do more. Consider any pointer returned from a function suspect. Use containers, resource handles, and views (e.g., `span` known not to be resource handles) to lower the number of cases to be examined. For starters, consider every class with a destructor as resource handle.

### <a name="Cr-templates"></a>Use templates to express containers (and other resource handles)

##### Reason

To provide statically type-safe manipulation of elements.

##### Example

    template<typename T> class Vector {
        // ...
        T* elem;   // point to sz elements of type T
        int sz;
    };

### <a name="Cr-value-return"></a>Return containers by value (relying on move or copy elision for efficiency)

##### Reason

To simplify code and eliminate a need for explicit memory management. To bring an object into a surrounding scope, thereby extending its lifetime. See also [F.20, the general item about "out" output values](#Rf-out).

##### Example

    vector<int> get_large_vector()
    {
        return ...;
    }

    auto v = get_large_vector(); //  return by value is ok, most modern compilers will do copy elision

##### Exception

See the Exceptions in [F.20](#Rf-out).

##### Enforcement

Check for pointers and references returned from functions and see if they are assigned to resource handles (e.g., to a `unique_ptr`).

### <a name="Cr-handle"></a>If a class is a resource handle, it needs a constructor, a destructor, and copy and/or move operations

##### Reason

To provide complete control of the lifetime of the resource. To provide a coherent set of operations on the resource.

##### Example

    ??? Messing with pointers

##### Note

If all members are resource handles, rely on the default special operations where possible.

    template<typename T> struct Named {
        string name;
        T value;
    };

Now `Named` has a default constructor, a destructor, and efficient copy and move operations, provided `T` has.

##### Enforcement

In general, a tool cannot know if a class is a resource handle. However, if a class has some of [the default operations](#SS-ctor), it should have all, and if a class has a member that is a resource handle, it should be considered as resource handle.

### <a name="Cr-list"></a>If a class is a container, give it an initializer-list constructor

##### Reason

It is common to need an initial set of elements.

##### Example

    template<typename T> class Vector {
    public:
        Vector(std::initializer_list<T>);
        // ...
    };

    Vector<string> vs { "Nygaard", "Ritchie" };

##### Enforcement

When is a class a container? ???

# <a name="S-glossary"></a>Glossary

A relatively informal definition of terms used in the guidelines
(based of the glossary in [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html))

* *abstract class*: a class that cannot be directly used to create objects; often used to define an interface to derived classes.
  A class is made abstract by having a pure virtual function or only protected constructors.
* *abstraction*: a description of something that selectively and deliberately ignores (hides) details (e.g., implementation details); selective ignorance.
* *address*: a value that allows us to find an object in a computer's memory.
* *algorithm*: a procedure or formula for solving a problem; a finite series of computational steps to produce a result.
* *alias*: an alternative way of referring to an object; often a name, pointer, or reference.
* *application*: a program or a collection of programs that is considered an entity by its users.
* *approximation*: something (e.g., a value or a design) that is close to the perfect or ideal (value or design).
  Often an approximation is a result of trade-offs among ideals.
* *argument*: a value passed to a function or a template, in which it is accessed through a parameter.
* *array*: a homogeneous sequence of elements, usually numbered, e.g., \[0:max).
* *assertion*: a statement inserted into a program to state (assert) that something must always be true at this point in the program.
* *base class*: a class used as the base of a class hierarchy. Typically a base class has one or more virtual functions.
* *bit*: the basic unit of information in a computer. A bit can have the value 0 or the value 1.
* *bug*: an error in a program.
* *byte*: the basic unit of addressing in most computers. Typically, a byte holds 8 bits.
* *class*: a user-defined type that may contain data members, function members, and member types.
* *code*: a program or a part of a program; ambiguously used for both source code and object code.
* *compiler*: a program that turns source code into object code.
* *complexity*: a hard-to-precisely-define notion or measure of the difficulty of constructing a solution to a problem or of the solution itself.
  Sometimes complexity is used to (simply) mean an estimate of the number of operations needed to execute an algorithm.
* *computation*: the execution of some code, usually taking some input and producing some output.
* *concept*: (1) a notion, and idea; (2) a set of requirements, usually for a template argument.
* *concrete class*: class for which objects can be created.
* *constant*: a value that cannot be changed (in a given scope); not mutable.
* *constructor*: an operation that initializes ("constructs") an object.
  Typically a constructor establishes an invariant and often acquires resources needed for an object to be used (which are then typically released by a destructor).
* *container*: an object that holds elements (other objects).
* *copy*: an operation that makes two object have values that compare equal. See also move.
* *correctness*: a program or a piece of a program is correct if it meets its specification.
  Unfortunately, a specification can be incomplete or inconsistent, or can fail to meet users' reasonable expectations.
  Thus, to produce acceptable code, we sometimes have to do more than just follow the formal specification.
* *cost*: the expense (e.g., in programmer time, run time, or space) of producing a program or of executing it.
  Ideally, cost should be a function of complexity.
* *customization point*: ???
* *data*: values used in a computation.
* *debugging*: the act of searching for and removing errors from a program; usually far less systematic than testing.
* *declaration*: the specification of a name with its type in a program.
* *definition*: a declaration of an entity that supplies all information necessary to complete a program using the entity.
  Simplified definition: a declaration that allocates memory.
* *derived class*: a class derived from one or more base classes.
* *design*: an overall description of how a piece of software should operate to meet its specification.
* *destructor*: an operation that is implicitly invoked (called) when an object is destroyed (e.g., at the end of a scope). Often, it releases resources.
* *encapsulation*: protecting something meant to be private (e.g., implementation details) from unauthorized access.
* *error*: a mismatch between reasonable expectations of program behavior (often expressed as a requirement or a users' guide) and what a program actually does.
* *executable*: a program ready to be run (executed) on a computer.
* *feature creep*: a tendency to add excess functionality to a program "just in case."
* *file*: a container of permanent information in a computer.
* *floating-point number*: a computer's approximation of a real number, such as 7.93 and 10.78e-3.
* *function*: a named unit of code that can be invoked (called) from different parts of a program; a logical unit of computation.
* *generic programming*: a style of programming focused on the design and efficient implementation of algorithms.
  A generic algorithm will work for all argument types that meet its requirements. In C++, generic programming typically uses templates.
* *global variable*: technically, a named object in namespace scope.
* *handle*: a class that allows access to another through a member pointer or reference. See also resource, copy, move.
* *header*: a file containing declarations used to share interfaces between parts of a program.
* *hiding*: the act of preventing a piece of information from being directly seen or accessed.
  For example, a name from a nested (inner) scope can prevent that same name from an outer (enclosing) scope from being directly used.
* *ideal*: the perfect version of something we are striving for. Usually we have to make trade-offs and settle for an approximation.
* *implementation*: (1) the act of writing and testing code; (2) the code that implements a program.
* *infinite loop*: a loop where the termination condition never becomes true. See iteration.
* *infinite recursion*: a recursion that doesn't end until the machine runs out of memory to hold the calls.
  In reality, such recursion is never infinite but is terminated by some hardware error.
* *information hiding*: the act of separating interface and implementation, thus hiding implementation details not meant for the user's attention and providing an abstraction.
* *initialize*: giving an object its first (initial) value.
* *input*: values used by a computation (e.g., function arguments and characters typed on a keyboard).
* *integer*: a whole number, such as 42 and -99.
* *interface*: a declaration or a set of declarations specifying how a piece of code (such as a function or a class) can be called.
* *invariant*: something that must be always true at a given point (or points) of a program; typically used to describe the state (set of values) of an object or the state of a loop before entry into the repeated statement.
* *iteration*: the act of repeatedly executing a piece of code; see recursion.
* *iterator*: an object that identifies an element of a sequence.
* *library*: a collection of types, functions, classes, etc. implementing a set of facilities (abstractions) meant to be potentially used as part of more that one program.
* *lifetime*: the time from the initialization of an object until it becomes unusable (goes out of scope, is deleted, or the program terminates).
* *linker*: a program that combines object code files and libraries into an executable program.
* *literal*: a notation that directly specifies a value, such as 12 specifying the integer value "twelve."
* *loop*: a piece of code executed repeatedly; in C++, typically a for-statement or a while-statement.
* *move*: an operation that transfers a value from one object to another leaving behind a value representing "empty." See also copy.
* *mutable*: changeable; the opposite of immutable, constant, and invariable.
* *object*: (1) an initialized region of memory of a known type which holds a value of that type; (2) a region of memory.
* *object code*: output from a compiler intended as input for a linker (for the linker to produce executable code).
* *object file*: a file containing object code.
* *object-oriented programming*: (OOP) a style of programming focused on the design and use of classes and class hierarchies.
* *operation*: something that can perform some action, such as a function and an operator.
* *output*: values produced by a computation (e.g., a function result or lines of characters written on a screen).
* *overflow*: producing a value that cannot be stored in its intended target.
* *overload*: defining two functions or operators with the same name but different argument (operand) types.
* *override*: defining a function in a derived class with the same name and argument types as a virtual function in the base class, thus making the function callable through the interface defined by the base class.
* *owner*: an object responsible for releasing a resource.
* *paradigm*: a somewhat pretentious term for design or programming style; often used with the (erroneous) implication that there exists a paradigm that is superior to all others.
* *parameter*: a declaration of an explicit input to a function or a template. When called, a function can access the arguments passed through the names of its parameters.
* *pointer*: (1) a value used to identify a typed object in memory; (2) a variable holding such a value.
* *post-condition*: a condition that must hold upon exit from a piece of code, such as a function or a loop.
* *pre-condition*: a condition that must hold upon entry into a piece of code, such as a function or a loop.
* *program*: code (possibly with associated data) that is sufficiently complete to be executed by a computer.
* *programming*: the art of expressing solutions to problems as code.
* *programming language*: a language for expressing programs.
* *pseudo code*: a description of a computation written in an informal notation rather than a programming language.
* *pure virtual function*: a virtual function that must be overridden in a derived class.
* *RAII*: ("Resource Acquisition Is Initialization") a basic technique for resource management based on scopes.
* *range*: a sequence of values that can be described by a start point and an end point. For example, \[0:5) means the values 0, 1, 2, 3, and 4.
* *recursion*: the act of a function calling itself; see also iteration.
* *reference*: (1) a value describing the location of a typed value in memory; (2) a variable holding such a value.
* *regular expression*: a notation for patterns in character strings.
* *requirement*: (1) a description of the desired behavior of a program or part of a program; (2) a description of the assumptions a function or template makes of its arguments.
* *resource*: something that is acquired and must later be released, such as a file handle, a lock, or memory. See also handle, owner.
* *rounding*: conversion of a value to the mathematically nearest value of a less precise type.
* *RTTI*: Run-Time Type Information. ???
* *scope*: the region of program text (source code) in which a name can be referred to.
* *sequence*: elements that can be visited in a linear order.
* *software*: a collection of pieces of code and associated data; often used interchangeably with program.
* *source code*: code as produced by a programmer and (in principle) readable by other programmers.
* *source file*: a file containing source code.
* *specification*: a description of what a piece of code should do.
* *standard*: an officially agreed upon definition of something, such as a programming language.
* *state*: a set of values.
* *STL*: the containers, iterators, and algorithms part of the standard library.
* *string*: a sequence of characters.
* *style*: a set of techniques for programming leading to a consistent use of language features; sometimes used in a very restricted sense to refer just to low-level rules for naming and appearance of code.
* *subtype*: derived type; a type that has all the properties of a type and possibly more.
* *supertype*: base type; a type that has a subset of the properties of a type.
* *system*: (1) a program or a set of programs for performing a task on a computer; (2) a shorthand for "operating system", that is, the fundamental execution environment and tools for a computer.
* *template*: a class or a function parameterized by one or more types or (compile-time) values; the basic C++ language construct supporting generic programming.
* *testing*: a systematic search for errors in a program.
* *trade-off*: the result of balancing several design and implementation criteria.
* *truncation*: loss of information in a conversion from a type into another that cannot exactly represent the value to be converted.
* *type*: something that defines a set of possible values and a set of operations for an object.
* *uninitialized*: the (undefined) state of an object before it is initialized.
* *unit*: (1) a standard measure that gives meaning to a value (e.g., km for a distance); (2) a distinguished (e.g., named) part of a larger whole.
* *use case*: a specific (typically simple) use of a program meant to test its functionality and demonstrate its purpose.
* *value*: a set of bits in memory interpreted according to a type.
* *variable*: a named object of a given type; contains a value unless uninitialized.
* *virtual function*: a member function that can be overridden in a derived class.
* *word*: a basic unit of memory in a computer, often the unit used to hold an integer.

# <a name="S-unclassified"></a>To-do: Unclassified proto-rules

This is our to-do list.
Eventually, the entries will become rules or parts of rules.
Alternatively, we will decide that no change is needed and delete the entry.

* No long-distance friendship
* Should physical design (what's in a file) and large-scale design (libraries, groups of libraries) be addressed?
* Namespaces
* How granular should namespaces be? All classes/functions designed to work together and released together (as defined in Sutter/Alexandrescu) or something narrower or wider?
* Should there be inline namespaces (à la `std::literals::*_literals`)?
* Avoid implicit conversions
* Const member functions should be thread safe ... aka, but I don't really change the variable, just assign it a value the first time it's called ... argh
* Always initialize variables, use initialization lists for member variables.
* Anyone writing a public interface which takes or returns `void*` should have their toes set on fire. That one has been a personal favorite of mine for a number of years. :)
* Use `const`-ness wherever possible: member functions, variables and (yippee) `const_iterators`
* Use `auto`
* `(size)` vs. `{initializers}` vs. `{Extent{size}}`
* Don't overabstract
* Never pass a pointer down the call stack
* falling through a function bottom
* Should there be guidelines to choose between polymorphisms? YES. classic (virtual functions, reference semantics) vs. Sean Parent style (value semantics, type-erased, kind of like `std::function`)  vs. CRTP/static? YES Perhaps even vs. tag dispatch?
* Speaking of virtual functions, should non-virtual interface be promoted? YES. (public non-virtual `foo()` calling private/protected `do_foo()`)? Not a new thing, seeing as locales/streams use it, but it seems to be under-emphasized.
* should virtual calls be banned from ctors/dtors in your guidelines? YES. A lot of people ban them, even though I think it's a big strength of C++ that they are ??? -preserving (D disappointed me so much when it went the Java way). WHAT WOULD BE A GOOD EXAMPLE?
* Speaking of lambdas, what would weigh in on the decision between lambdas and (local?) classes in algorithm calls and other callback scenarios?
* And speaking of `std::bind`, Stephen T. Lavavej criticizes it so much I'm starting to wonder if it is indeed going to fade away in future. Should lambdas be recommended instead?
* What to do with leaks out of temporaries? : `p = (s1 + s2).c_str();`
* pointer/iterator invalidation leading to dangling pointers:

        void bad()
        {
            int* p = new int[700];
            int* q = &p[7];
            delete p;

            vector<int> v(700);
            int* q2 = &v[7];
            v.resize(900);

            // ... use q and q2 ...
        }

* LSP
* private inheritance vs/and membership
* avoid static class members variables (race conditions, almost-global variables)

* Use RAII lock guards (`lock_guard`, `unique_lock`, `shared_lock`), never call `mutex.lock` and `mutex.unlock` directly (RAII)
* Prefer non-recursive locks (often used to work around bad reasoning, overhead)
* Join your threads! (because of `std::terminate` in destructor if not joined or detached ... is there a good reason to detach threads?) -- ??? could support library provide a RAII wrapper for `std::thread`?
* If two or more mutexes must be acquired at the same time, use `std::lock` (or another deadlock avoidance algorithm?)
* When using a `condition_variable`, always protect the condition by a mutex (atomic bool whose value is set outside of the mutex is wrong!), and use the same mutex for the condition variable itself.
* Never use `atomic_compare_exchange_strong` with `std::atomic<user-defined-struct>` (differences in padding matter, while `compare_exchange_weak` in a loop converges to stable padding)
* individual `shared_future` objects are not thread-safe: two threads cannot wait on the same `shared_future` object (they can wait on copies of a `shared_future` that refer to the same shared state)
* individual `shared_ptr` objects are not thread-safe: different threads can call non-`const` member functions on *different* `shared_ptr`s that refer to the same shared object, but one thread cannot call a non-`const` member function of a `shared_ptr` object while another thread accesses that same `shared_ptr` object (if you need that, consider `atomic_shared_ptr` instead)

* rules for arithmetic

# Bibliography

* <a name="Alexandrescu01"></a>
  \[Alexandrescu01]:  A. Alexandrescu. Modern C++ Design (Addison-Wesley, 2001).
* <a name="Cplusplus03"></a>
  \[C++03]:           ISO/IEC 14882:2003(E), Programming Languages — C++ (updated ISO and ANSI C++ Standard including the contents of (C++98) plus errata corrections).
* <a name="CplusplusCS"></a>
  \[C++CS]:
* <a name="Cargill92"></a>
  \[Cargill92]:       T. Cargill. C++ Programming Style (Addison-Wesley, 1992).
* <a name="Cline99"></a>
  \[Cline99]:         M. Cline, G. Lomow, and M. Girou. C++ FAQs (2ndEdition) (Addison-Wesley, 1999).
* <a name="Dewhurst03"></a>
  \[Dewhurst03]:      S. Dewhurst. C++ Gotchas (Addison-Wesley, 2003).
* <a name="Henricson97"></a>
  \[Henricson97]:     M. Henricson and E. Nyquist. Industrial Strength C++ (Prentice Hall, 1997).
* <a name="Koenig97"></a>
  \[Koenig97]:        A. Koenig and B. Moo. Ruminations on C++ (Addison-Wesley, 1997).
* <a name="Lakos96"></a>
  \[Lakos96]:         J. Lakos. Large-Scale C++ Software Design (Addison-Wesley, 1996).
* <a name="Meyers96"></a>
  \[Meyers96]:        S. Meyers. More Effective C++ (Addison-Wesley, 1996).
* <a name="Meyers97"></a>
  \[Meyers97]:        S. Meyers. Effective C++ (2nd Edition) (Addison-Wesley, 1997).
* <a name="Meyers15"></a>
  \[Meyers15]:        S. Meyers. Effective Modern C++ (O'Reilly, 2015).
* <a name="Murray93"></a>
  \[Murray93]:        R. Murray. C++ Strategies and Tactics (Addison-Wesley, 1993).
* <a name="Stroustrup00"></a>
  \[Stroustrup00]:    B. Stroustrup. The C++ Programming Language (Special 3rdEdition) (Addison-Wesley, 2000).
* <a name="Stroustrup05"></a>
  \[Stroustrup05]:    B. Stroustrup. [A rationale for semantically enhanced library languages](http://www.stroustrup.com/SELLrationale.pdf).
* <a name="Stroustrup13"></a>
  \[Stroustrup13]:    B. Stroustrup. [The C++ Programming Language (4th Edition)](http://www.stroustrup.com/4th.html). Addison Wesley 2013.
* <a name="Stroustrup14"></a>
  \[Stroustrup14]:    B. Stroustrup. [A Tour of C++](http://www.stroustrup.com/Tour.html).
  Addison Wesley 2014.
* <a name="SuttHysl04b"></a>
  \[SuttHysl04b]:     H. Sutter and J. Hyslop. "Collecting Shared Objects" (C/C++ Users Journal, 22(8), August 2004).
* <a name="SuttAlex05"></a>
  \[SuttAlex05]:      H. Sutter and  A. Alexandrescu. C++ Coding Standards. Addison-Wesley 2005.
* <a name="Sutter00"></a>
  \[Sutter00]:        H. Sutter. Exceptional C++ (Addison-Wesley, 2000).
* <a name="Sutter02"></a>
  \[Sutter02]:        H. Sutter. More Exceptional C++ (Addison-Wesley, 2002).
* <a name="Sutter04"></a>
  \[Sutter04]:        H. Sutter. Exceptional C++ Style (Addison-Wesley, 2004).
* <a name="Taligent94"></a>
  \[Taligent94]: Taligent's Guide to Designing Programs (Addison-Wesley, 1994).
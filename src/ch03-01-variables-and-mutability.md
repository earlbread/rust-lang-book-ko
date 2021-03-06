## 변수와 가변성

2장에서 언급했듯이, 변수는 기본적으로 불변입니다. 이것은
러스트가 제공하는 안정성과 쉬운 동시성이라는 이점을 얻을 수 있는 방향으로 코드를
쓰게 하는 강제사항(nudge)중 하나입니다. 하지만 당신은 여전히
변수를 가변으로 만들 수 있습니다. 어떻게 하는지 살펴보고 왜
러스트가 불변성을 권하는지와 어떨 때 가변성을 써야 하는지
알아봅시다.

변수가 불변일 때, 한번 값이 묶이면 그 값은 바꿀 수
없습니다. 이를 표현하기 위해, `cargo new variables`로
*projects* 디렉터리 안에 *variables*라는 프로젝트를 만들어 봅시다.

그리고, 새 *variables* 폴더의 *src/main.rc* 파일을 열어서
아직은 컴파일되지 않는 다음의 코드로 교체하세요:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

저장하고 `cargo run`으로 프로그램을 실행해보세요. 다음의 출력처럼
에러 메시지를 받을 것입니다:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

이 예시는 컴파일러가 프로그램의 에러 찾기를 어떻게 도와주는지 보여줍니다.
컴파일러 에러가 실망스러울 수도 있겠지만, 컴파일러는 그저 당신의 프로그램이 아직은 
원하는 대로 안전하게 동작하지 않는다고 할 뿐입니다. 컴파일러는 당신이 좋은 프로그래머가
아니라고 한 적이 *없습니다*! 경험이 많은 러스트인들조차 컴파일러 에러가 발생합니다.

에러 메시지는 에러의 원인이 `불변 변수 x에
두 번 값을 할당할 수 없다`는 것을 알려줍니다. 불변 변수 `x`에 두 번째 값을 할당하려고
했기 때문이죠.

불변으로 지정한 값을 변경하려고 하는
바로 이 상황이 버그로 이어질 수 있기 때문에,
컴파일-타임 에러가 발생하는 것은 중요합니다. 만약 코드의 한 부분이 변수 값은 변하지 않는다는
전제 하에 작동하고 코드의 다른 부분이 그 값을 바꾼다면, 첫
부분의 코드는 원래 지정된 일을 못할 가능성이 생깁니다.
이런 류의 버그는 발생 후 추적하는 것이 어려운데,
특히 코드의 두 번째 부분이 값을 *가끔 바꿀 때* 그렇습니다.

러스트에서는, 당신이 변수가 변하지 않는다고 지정하면
컴파일러가 이를 보증합니다. 이 말은 코드를 읽고 쓸 때 값이 어디서 어떻게 변할 지
쫓을 필요가 없다는 것입니다. 따라서 당신의 코드는
흐름을 따라가기 쉬워집니다.

하지만 가변성은 아주 유용할 수 있습니다. 변수는 단지 기본적으로 불변일 뿐입니다.
2장에서 한 것처럼, 변수명 앞에 `mut`을 붙여서 가변으로
만들 수 있습니다. 이 변수에게 값이 변할 수 있도록 하는 것에 더해,
다른 부분의 코드에서 이 변수의 값이 변할 것이라는
의미를 미래의 독자들에게 전달합니다.

예를 들어, *src/main.rs*를 다음과 같이 바꿉시다.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

지금 이 프로그램을 실행하면, 다음의 출력을 얻습니다.

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

우리는 `mut`를 사용해 `x`의 값을 `5`에서 `6`으로 바꿀 수
있었습니다. 몇몇 경우에는 변수를 가변으로 만들고 싶을 텐데,
불변 변수만 있을 때보다 코드가 더 간편해지기 때문입니다.

버그를 방지하는 것 외에도 고려해야 할 비용이
있습니다. 예를 들어, 큰 데이터 구조를 사용할 때, 인스턴스를 알맞게 가변으로 설정하는 것은 
새로 인스턴스를 할당하고 복사해서 돌려주는 것보다 빠를 수
있습니다. 작은 데이터 구조라면, 새 인스턴스를 만들고 더 함수형 프로그래밍 스타일
로 작성하는 것이 더 흐름을 따라가기 쉽기 때문에, 퍼포먼스가
느려지더라도 명확성을 얻는 것에 대한 패널티로 받아들이는 것이 좋을 수 있습니다.

### 변수와 상수의 차이

변수의 값을 바꿀 수 없게 하는 것이 대부분의 다른 프로그래밍 언어가 가지고
있는 *상수*를 떠올리게 했을 수도 있겠습니다. 불변 변수와
마찬가지로 상수는 이름에 묶인 값이며
바꿀 수 없지만, 상수와 변수는 몇가지 차이가
있습니다.

먼저, `mut`와 상수를 함께 사용할 수 없습니다. 
상수는 기본적으로 불변일 뿐만 아니라 항상 불변입니다.

상수는 `let` 키워드 대신 `const` 키워드로 선언하며,
값의 타입은 *반드시* 어노테이션이 달려야 합니다. 다음 절에서 타입과 타입 
어노테이션을 다음 절인 [“Data Types,”][data-types]<!-- ignore -->에서 다룰 예정이므로,
자세한 사항은 아직 걱정하지 않아도 됩니다. 항상 어노테이션을
달아야 한다는 것만 알아두세요.

상수는 전역 스코프를 포함한 어느 스코프에서도 선언 가능하며, 전역 스코프도 되는데,
이는 코드의 많은 부분에서 알 필요가 있는 값들에 유용합니다.

마지막 차이점은, 상수는 반드시 상수 표현식이어야 하고
함수의 결과값이나 런타임에 결정되는 그 어떤 값이어도
안된다는 것입니다.

여기 상수의 선언 예시로 `MAX_POINTS`라는 이름의
상수를 선언하고 그 값을 100,000으로 정했습니다. (상수를 위한 러스트의 작명 관례로,
단어 사이에는 언더스코어를 사용하고 모든 글자를 대문자로 하며,
가독성을 위해 숫자 상수에는 언더바를 사용할 수 있습니다.)

```rust
const MAX_POINTS: u32 = 100_000;
```

상수는 선언된 스코프 내에서 프로그램이 동작하는 전체
시간동안 유효하며, 플레이어가 얻을 수 있는
점수의 최고값이나 빛의 속도같이
프로그램의 여러 부분이
알 필요가 있는 값들에 유용합니다.

전체 프로그램에 하드코드된 값에 상수로써 이름을 붙이는 것은
미래의 코드 관리자에게 그 값의 의미를 전달하는데 유용합니다.
또한 나중에 업데이트될 하드코드된 값을
단 한 군데에서 변경할 수 있게 해줍니다.

### 덮어쓰기

2장 추측 게임의 [“Comparing the Guess to the Secret
Number”][comparing-the-guess-to-the-secret-number]<!-- 
ignore --> 섹션에서 보았듯이, 새 변수를 이전 변
수명과 같은 이름으로 선언할 수 있고, 새 변수는 이전의 변수를 덮어씁니다.
러스트인들은 첫 번째 변수가 두 번째 변수에 의해 *덮어쓰였다*라고 표현하며,
이는 두 번째 변수의 값이 그 변수가 앞으로 사용될 때의
값이라는 것을 의미합니다. 우리는 다음과 같이 같은 변수명과 `let` 키워드의
반복으로 덮어쓸 수 있습니다.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

이 프로그램은 1차로 `x`에 `5`를 바인드(대입)합니다. 그리고 `let x = `을 반복해 `x`의 값을 가려
원래 값에 `1`을 더한 값을 대입해 `x`의 값은 `6`이 됩니다.
세 번째 `let` 구문 또한 `x`를 덮어 씌우며,
이전 값에 `2`를 곱해 `x`에 할당해서 `x`의 최종값은 `12`가 됩니다. 우리가 이 프로그램을 실행하면
다음과 같이 출력될 것입니다.

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

덮어쓰기는 변수를 `mut`로 표시하는 것과는 다릅니다.
`let` 키워드 없이 값을 재할당 하려고 한다면
컴파일-타임 에러가 발생하기 때문입니다.
`let`을 사용하면, 값을 이전하면서 불변으로
유지할 수 있습니다.

`mut`과 덮어쓰기의 또다른 차이점은,
같은 변수명으로 다른 타입의 값을 저장할 수 있다는 것입니다.
예를 들어, 프로그램이 사용자에게 글 사이에
몇 개의 공백을 넣고 싶은지 입력하도록 하는데,
우리는 이 값을 숫자로써 보관하고 싶습니다.

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

이 구조는 숫자 타입인 두 번째 `spaces` 변수가 문자열 타입인
첫 번째 `spaces` 변수를 덮어쓰기에 가능합니다.
따라서 덮어쓰기는 우리가 `spaces_str`과 `spaces_num`같은
변수명을 쓸 필요가 없게 해줍니다.
대신에, 우리는 단순히 `spaces`라는 이름을 재사용할 수 있습니다.
하지만, 여기에 `mut`을 사용하려 한다면, 보시다시피 컴파일-타임 에러가 발생합니다.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

에러는 변수의 타입을 바꿀 수 없다고 알려줍니다.

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

변수가 어떻게 작동하는지 알아보았으니, 변수가 가질 수 있는 더 많은
타입들에 대해 알아봅시다.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types

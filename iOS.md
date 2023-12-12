## Deployment Target -> 실행 가능한 iOS최소 버전

## Assistant editor -> storyboard와 소스를 연결하는 에디터

## @IBOutlet, action -> Attribute inspector를 통해 만드는거
아웃렛은 프로퍼티, 액선은 메서드선언

## Connections Inspector -> 아웃렛 연결에 대한거 노란삼각형이 나오면 끊어준다

## StroyBoard Entry Point -> 첫화면에 나오는 왼쪽화살표. 진입점임. 이게 없으면 검은화면나옴

## Action으로 만든 함수들을 보면, (_ sender: UIButton) 이렇게 나오는데 이거뭐냐. 함수 전에 나왔던거
외부 매개변수를 생략한거다(4주차)

##    @IBOutlet weak var webView: WKWebView! 마지막꺼는 자료형이고, 느낌표는 옵셔널임. 옵셔널푸는거아님 이거의 장점?


클래스상속, 오버로드, 슈퍼, class ViewController: UIViewController 이거뭐냐

path 를 통해서 
let file:String? = Bundle.main.path(forResource:"bmi", ofType: "mp4")
이거 리턴형이 옵셔널인이유는 아래 코드
let url = NSURL(fileURLWithPath: file!)
옵셔널이니까 느낌표로 푸는거임


https://developer.apple.com/documentation/foundation/bundle/1409670-path

func path(
    forResource name: String?,
    ofType ext: String?,
    inDirectory subpath: String?
) -> String?
옵셔널 밭이다.


guard let


URL에 대해서
Summary

Creates a URL instance from the provided string.
Declaration

init?(string: String)   이거 생성자인데 뒤에 물음표가 있는 이유? failable initioalizer 이거 다음에 가드랫으로 푸는거 연계해서 보셈
Discussion

This initializer returns nil if the string doesn’t represent a valid URL. For example, an empty string or one containing characters that are illegal in a URL produces nil.
Parameters

string	
A URL location.



forced unwrapping 에서 if x!=nil 이런식으로 하면안됌.

옵셔널 바인딩보기

var x : Int?
x = 10 if let x = x 라고 써도 됨
if let xx = x { //옵셔널 변수 x가 값(10)이 있으므로 언래핑해서 일반 상수 xx에 대입하고 if문 실행 } print(x,xx)
else {
} print("nil")
var x1 : Int?
if let xx = x1 { //옵셔널 변수 x1이 값이 없어서 if문의 조건이 거짓이 되어 if문 실행하지 않고 else로 감 } print(xx)
else {
} print("nil")

## 여러개 옵셔널 언래핑.

## 느낌표로 언래핑
implicitly Unwrapping optional
자동으로 언래핑을 하는 옵셔널 자료형 ->!

3주차 15페이지 예제
약간 위험할수있는데 왜 엑스코드에서는 !를 쓰냐? 확신이 있다고 함..
!? 차이

argument, parameter 차이

함수는 걍 다보기
매우중요라고 써있는거 보기

failable initializer 로 만든 프로퍼티는 옵셔널로나옴,. 그래서 그걸 풀어야함.
guard let url = URL(string:urlString) else {
    print("invaild URL");
}

클래스도.
..
그냥 다하셈

여기는 10- 14주차자료
![스크린샷 2023-12-12 오후 12 14 40](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/466ebbab-6fff-48b8-b9f8-262bd01727ba)
이거 가드문으로 해보기
뜬금없이 !가 나오는 부분 부자연스럽기떄문에..

그리고 31페이지

더블은 구조체인데 그 안에 스트링이 들어가면 nil이 들어가야하기 떄문에 더블형에 문자가 들어가면 옵셔널로 튀어나옴.
그래서 !가 두번쓰임. forced unwrapping을 2번한거임.

let weight = Double(txtWeight.text!)!
let height = Double(txtHeight.text!)!
let bmi = weight / (heightheight0.0001) // kg/m*m 이걸 보다 안전한 옵셔널 바인딩 두가지방법으로

이상민의 어시스턴트
GPT-4
옵셔널 바인딩을 사용하여 안전하게 값을 얻어올 수 있습니다. 다음은 두 가지 방법입니다.

if let을 사용한 옵셔널 바인딩:
swift
if let weight = Double(txtWeight.text!), let height = Double(txtHeight.text!) {
    let bmi = weight / (height * height * 0.0001)
    // 여기서 bmi를 사용합니다.
} else {
    // 변환에 실패했을 때의 처리를 작성합니다.
}
guard let을 사용한 옵셔널 바인딩:
swift
guard let weight = Double(txtWeight.text!), let height = Double(txtHeight.text!) else {
    // 변환에 실패했을 때의 처리를 작성하고 함수나 메소드를 빠져나갑니다.
    return
}
let bmi = weight / (height * height * 0.0001)
// 여기서 bmi를 사용합니다.


if let 보다 guard let을 더 많이쓰는이유? -> 가독성면에서 좋음. 다른건 찾아보기

# 1207_day2
## 함수
### 함수를 만들때 규칙
1. 최대한 "작게" 만들어라
   - if문, guard문, while문 등의 내용은 1줄로 작성, 중첩은 1번에서 2번으로 최대한 적게 들어가는것이 좋다.
2. 한 가지의 일만 하게 해라
- - -
ex)
bad
- if문의 내용이 두줄 이상으로 너무 길다.
- 하나의 함수에서 다양한 일을 하고있다.
```
func renderPageWithSetupsAndTeardowns(_ page: Page) -> String {
    let isTestPage = page.contains("Test")
    if isTestPage {
        let testPage = page.getWikiPage()
        var newPageContent = PageContent()
        includeSetup(newPageContent, to: testPage)
        newPageContent.append(page.getContent())
        includeTeardown(newPageContent, to: testPage)
        page.setContent(newPageContent)
    }
    return page.getHtml()
}
```
good
- if문의 내용이 한줄이다.
- 한가지 일만 하고있다.
```
func renderPageWithSetupsAndTeardowns(_ page: Page) -> String {
    if isTestPage(page) {
        includeSetupAndTeardownPages(to: page)
    }
    return page.getHtml()
}
```
3. 추상화 수준을 똑같이 정하자
- 사실 리펙토링한 코드 또한 3가지의 일을 하고 있다
  - 1. 페이지가 테스트 페이지인지 확인
  - 2. 맞다면 페이지를 추가한다.
  - 3. 페이지를 HTML로 렌더링한다.
- 하나의 함수에 한가지 일을 한다는 의미는 즉
  - 함수의 이름 아래에서 추상화 수준이 하나라는 뜻
- 어렵지만 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
- #### 위에서 아래로 코드 읽기
  - 문단을 읽어 내려가듯 코드를 구현하면 추상화 수준을 일관되게 만들기 쉬워진다.

4. 술적인 이름을 사용하라
- 내용을 유추하기 쉽게 이름을 길게 적어도 괜찮다.
- 오히려 짧고 함축적이라 의미가 모호한것보다 훨씬 좋은 방법이다.

5. 함수 인자
- 함수에서 인수의 개수는 0~2개가 가장 적절하다.
- 3개이상은 읽기가 어려워지기 때문에 피하는게 좋다.
- 필요한 경우엔 객체로 만들어 인자로 넘겨주는게 훨씬 읽기 편함

bad
```
func makeCircle(x: Double, y: Double, radius: Double) -> Circle
```
good
```
func makeCircle(center: Point, radius: Double) -> Circle
```

6. 부수효과(side Effect)가 없는 함수를 만들어라
- 부수효과란 값을 반환하는 함수가 외부의 내용을 변환하는 경우를 말한다.
- ex) userNamerhk password를 확인하고 두 인수가 올바르면 session을 초기화하고 true를 반환, 아니면 false를 반환하는 함수이다.
  - 여기서 부수 효과는 Sesstion.initalize()에서 함수 외부에 있는 session을 초기화하는 일이 발생한다.
```
class UserValidator {
    private let cryptographer: Cryptographer
    init (cryptographer: Cryptographer) {
        self.cryptographer = cryptographer
    }
    
    func checkPassword(userName: String, password: String) -> Bool {
        let user = UserGateway.findByName(userName)
        guard let user = user else { return false }
        let codedPhase = user.getPhraseEncodedByPassword()
        let phrase = cryptographer.decrypt(codedPhase, password)
        if ("Valid Password".contains(phrase)) {
            Session.initalize()
            return true
        }
    }
}
```
- 에초에 한가지일을 하는 함수가 아니게 되기때문에 좋지 않은 함수이지만 최소한
- 함수 이름을 "checkPasswordAndInitializeSession" 로 변경하여 함수에 하는 일과 부수효과를 알수 있게 명시해 주는게 필요하다

7. 오류 코드보다 예외를 사용하라
- 오류 코드보다 try-catch를 사용해라
- 오류시 처리되는 코드와 원래코드가 분류되어 좀더 깔끔한 코드가 완성된다.
bad
```
if deletePage(page) == E_OK {
    if registry.deleteReference(page.name) == E_OK {
        if configKeys.deleteKey(page.name.makeKey()) == E_OK {
            logger.log("page deleted")
        } else {
            logger.log("configKey not deleted")
        }
    } else {
        logger.log("deleteReference from registry failed")
    }
} else {
    logger.log("delete failed")
    return E_ERROR
}
```
good
```
try {
    deletePage(page)
    registry.deleteReference(page.name)
    configKeys.deleteKey(page.name.makeKey())
} catch {
    logger.log(error)
}

func deletePageAndAllReference(page: Page) throws {
    deletePage(page)
    registry.deleteReference(page.name)
    configKeys.deleteKey(page.name.makeKey())
}
```

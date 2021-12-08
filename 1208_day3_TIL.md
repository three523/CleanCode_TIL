# 1208_day3
## 클린코드 ch.3 주석
### 주석
- 주석을 안달고 이해시키는게 제일 베스트!

bad
```
// 직원에게 복지 혜택을 받을 자격이 있는지 검사
if (employee.flags & HOURLY_FLAG) && (employee.age > 65)
```
good
```
if employee.isEligibleForFullBenefits()
```

### 사용해야 할 경우의 주석

1. 정보를 제공하는 주석
ex : 정규 표현식을 표현한 코드
```
// kk:mm:ss EEE, MMM dd, yyyy 형식
let timeMatcher = Pattenrn.compile("\\d*:\\d*: ...")
```
2. TODO, FIXME 주석
- TODO: 지금은 해결하지 않지만 나중에 해할 일을 미리 적어둘떼
- FiXME: 문제가 있지만, 당장 수정할 필요는 없을떼
![image](https://user-images.githubusercontent.com/71269216/145208068-0de9c519-ccf6-4b9b-a117-e592cf8a3dd7.png)   
swift에서 //TODO: - 할일, //FIXME: - 버그 라고 적으면 보이는 부분
3. 결과를 경고하는 주석
- 특정 테스트 케이스의 시간이 오래걸려 경고 메세지를 남기는 주석
```
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
func testWithReallyBigFile() {
  writeLineToFile(1000000000)
  ...
}
```
4. 중요성을 강조하는 주석
- 자칫 대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서 주석을 사용
```
let listItemContent = match.group(3).trim()
// 여기서 teim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
```
### 번외 xcode의 퀵헬프 사용하기
- 퀵헬프는 함수나 변수위에 마크다운 형식으로 주석을 적으면 나중에 option + 클릭으로 문서화된 형식으로 설명을 볼 수 있게해주는 기능
```
 # Lists
 
 You can apply *italic*, **bold**, or `code` inline styles.
 
 ## Unordered Lists
 
 - Unordered item
 - Unordered item with sublist
   - Unordered subitem
 
 ## Ordered Lists
 
 1. Ordered item
    1. Ordered subitem
    2. Ordered subitem
 2. Ordered item
 */
func unlockDoor(with password: String) -> Bool {
    guard password != "1234" else {
        return false
    }
    
    return true
}

unlockDoor(with: "1234")
```
![image](https://user-images.githubusercontent.com/71269216/145208788-c79e278e-8eec-4d1e-8320-6ff26c248140.png)
   option + 클릭시 모습
![image](https://user-images.githubusercontent.com/71269216/145208895-88e6aa7f-50a5-49f2-83fa-2e9642698749.png)
퀵헬프 클릭시 보이는 모습
- 퀵헬프를 숨기기/보이기는: 쉬프트 + 컨트롤 + 커맨드 + <- or

#EditText 키보드의 기능 변경하기#

_ _ _


- EditText에 입력할 때 회원가입을 한다거나 하는 경우에는 '엔터' 대신에 '다음'의 기능으로 되어있는 것이 편함.

- 이러한 기능은 `imeOptions 속성`을 추가해주면 됨.

- xml에서 간단하게 해당 edittext에 아래 속성을 추가해주면 됨.

		android:imeOptions="actionNext"
        android:inputType="text"
        
- 처음에 imeOptions만 추가해주었을때는 적용되지 않았으나, inputType을 설정해주고 나니 적용이 되었다.

- inputType을 숫자로 한다면 숫자키보드로 바뀌지 않을까 생각은 하였으나 아직 적용히보지는 않음.

- imeOptions 에는 다양한 속성이 존재한다.

		android:imeOptions="normal"     // 특별한 의미 없음
		android:imeOptions="actionUnspecified"     // 특별한 의미 없음
		android:imeOptions="actionNone"     // 특별한 의미 없음
		android:imeOptions="actionGo"     // '이동'의 의미 (예 : 웹 브라우져에서 사용)
		android:imeOptions="actionSearch"     // '검색'의 의미 (예 : 네이버 검색창)
		android:imeOptions="actionSend"     // '보내기'의 의미 (예 : 메세지 작성시 사용)
		android:imeOptions="actionNext"     // '다음'의 의미 (예 : 회원가입시 다음 필드로 이동시)
		android:imeOptions="actionDone"     // '완료'의 의미 (예 : 정보 입력창)
		android:imeOptions="actionPrevious"     // '이전'의 의미 (예 : 회원가입시 이전 필드로 이동시) - API11부터 가능
        

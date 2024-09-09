# 1. 주제
----------
+ DooIn: 모임 중개 서비스
# 2. 역할 분담
----------
+ 요구사항 영역별 정리
  + 회원가입/ 로그인 / 아이디,비밀번호 찾기 : 양웅열
  + 마이페이지기능(현재 자기 상태창(회원정보) 표시및 개인정보 수정, 회원탈퇴) : 김도영
  + 친구페이지(친구 상태 확인, 친구 쪽지 발송, 친구 추가 알림 표시) : 양웅열
  + 메인페이지(현재 올라와 있는 모임 표시) : 김도영
  + 사이드바(회원 프로필 사진 표시, 현재 가입된 모임 표시, 모임 가입 알림 표시): 양웅열
  + 모임 개설 페이지 : 양웅열
  + 모임 상세 페이지 : 양웅열, 김동환
  + 게시판(게시글 출력, 카테고리 별 게시글 출력, 카테고리 별 게시글 검색(제목, 작성자), 내가 쓴 글 조회) :김동환
  + 게시글 쓰기 페이지: 김동환
  + 게시글 상세 페이지(해당 게시글 상세페이지 출력, 게시글 삭제 기능, 수정 기능 ,댓글 창 출력, 댓글 작성, 자기가 쓴 글이면 삭제 및 수정 버튼 활성화): 김동환
  + 카카오맵 : 김동환
+ ERD 및 테이블/UI 구성
  + 상세 기능 설계: 김동환, 양웅열, 김도영
  + 테이블 제작 : 김동환, 양웅열
  + ERD 제작: 김도영
  + 피그마 제작: 김도영, 김동환, 양웅
  + 쿼리 작성
    + controller
      + FriendController: 양웅열
      + LetterController: 양웅열
      + LoginController: 양웅열
      + MeetingController: 양웅영
      + MyController: 양웅열
      + NotBoController: 김동환
    + dao
      + FriendDAO: 양웅열
      + LetterDAO: 양웅열
      + LoginDAO: 양웅열
      + MeetingDAO: 양웅열
      + NotBoDao: 김동환
      + MyDAO: 양웅열
    + utils
      + Common : 양웅열
    + vo
       + chatVo: 양웅열
       + CommentVO: 양웅열
       + LetterVO: 양웅열
       + FriendVO: 양웅열
       + MeetingMemberVO: 양웅열
       + MeetingVO: 양웅열
       + NotBoVo: 김동환
       + ScheduleVO: 양웅열
       + UserInfoVO: 양웅열
+ 프론트엔드
    + 로그인 페이지: 양웅열
    + 메인페이지 : 양웅열
    + 모임 상세페이지: 양웅열, 김동환
    + 친구 창: 양웅열
    + 쪽지 페이지: 양웅열
    + 게시판: 김동환
    + 상세 게시판: 김동환
    + 글쓰기: 김동환
    + 네비 바: 양웅열
    + 반응형 쿼리: 김동환
+ PPT 협업
  https://www.canva.com/design/DAGGm-QEqYo/NzWNEgjP6CiYXJpcSQ-zLg/edit?utm_content=DAGGm-QEqYo&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton
  + PPT 제작: 김동환
# 3. DB테이블 구성
![스크린샷 2024-09-09 115405](https://github.com/user-attachments/assets/255767da-3951-47c9-ba73-3b799455c19f)
+ 스토리 보
![스크린샷 2024-09-09 115535](https://github.com/user-attachments/assets/b8004977-f445-4b51-a874-1d647e300529)


## 1. **유저정보 (USER_INFO_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
| 아이디 | USER_ID | VARCHAR2 | PK |
| 비밀번호 | USER_PW | VARCHAR2 | |
| 생년월일 | USER_ BIRTH | DATE | |	
| 닉네임 | USER_NICK | VARCHAR2 | |	
| 이메일 | USER_EMAIL | VARCHAR2 | |	
| 성별 | USER_GENDER | VARCHAR2 | |	
| 자기소개 | USER_INTRODUTION | VARCHAR2 | |	
| 프로필 | USER_PROFILE | VARCHAR2 | |

```sql
CREATE TABLE USER_INFO_TB(
    USER_ID VARCHAR2(255) PRIMARY KEY,
    USER_PW VARCHAR2(255),
    USER_BIRTH DATE, 
    USER_NICK VARCHAR2(255),
    USER_EMAIL VARCHAR2(255),
    USER_GENDER VARCHAR2(255),
    USER_INTRODUTION VARCHAR2(255),
    USER_PROFILE VARCHAR2(500)
);
```
+ 모임 (MEETING_TB)
  이름	컬럼명	자료형	제약 조건
모임번호	MEETING_NO	NUMBER	PK
모집제목	MEETING_TITLE	VARCHAR2	NOT NULL
모임명	MEETING_NAME	VARCHAR2	NOT NULL
위치	MEETING_LOCATION	VARCHAR2	NOT NULL
기간	MEETING_DURATION	DATE	NOT NULL
기간2	MEETING_DURATION2	DATE	NOT NULL
인원	MEETING_PERSONNEL	NUMBER	NOT NULL
모임장	USER_ID	VARCHAR2	FK USER_INFO_TB(USER_ID)
카테고리	MEETING_CATEGORY	VARCHAR2	NOT NULL
세부내용	MEETING_DETAILS	VARCHAR2
```sql
CREATE TABLE MEETING_TB(
    MEETING_NO VARCHAR2(4) PRIMARY KEY ,
    MEETING_TITLE VARCHAR2(255),
    MEETING_NAME VARCHAR2(255), 
    MEETING_LOCATION VARCHAR2(255),
    MEETING_DURATION DATE,
    MEETING_DURATION2 DATE
    MEETING_PERSONNEL NUMBER(4),
    USER_ID VARCHAR2() REFERENCES USER_INFO_TB(USER_ID),
    MEETING_CATEGORY VARCHAR2(255),
    MEETING_DETAILS VARCHAR2(255),
);

CREATE SEQUENCE MEETING_SEQ
 INCREMENT BY 1  
 START WITH 1    
 MAXVALUE 999    
 NOCYCLE;
```

## 2. **모임 참가 (MEETING_MEMBER_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|모임번호|MEETING_NO|NUMBER|FK MEETING_TB(MEETING_NO)|
|참여자|USER_ID|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|모임장|MASTER|VARCHAR2| |	
|수락|ACCEPT|VARCHAR2| |
|추가소개|DETAIL|VARCHAR2| |
```sql
CREATE TABLE MEETING_MEMBER_TB(
    MEETING_NO NUMBER(4),
    USER_ID VARCHAR2(255),
    MASTER VARCHAR2(5),
    ACCEPT VARCHAR2(5),
    DETAIL VARCHAR2(3000)
);
```
## 3. **편지함 (LETTER_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|쪽지번호|LETTER_NO|NUMBER|PK|
|작성자|LETTER_SENDER|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|작성자|닉네임|LETTER_SENDERNICK|VARCHAR2|FK USER_INFO_TB(USER_NICK)|
|수신자|LETTER_RECEIVER|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|수신자 닉네임|LETTER_RECEIVERNICK|VARCHAR2|FK USER_INFO_TB(USER_NICK)|
|제목|LETTER_TITLE|VARCHAR2| |	
|내용|LETTER_CONTENTS|VARCHAR2| |	
|작성일|LETTER_DATE|DATE| |
|수신여부|LETTER_VIEW|BOOL| |	
```sql
CREATE TABLE LETTER_TB(
    LETTER_NO NUMBER(4) PRIMARY KEY,
    LETTER_SENDER VARCHAR2(255),
    LETTER_SENDERNick VARCHAR2(255),
    LETTER_RECEIVER VARCHAR2(255),
    LETTER_RECEIVERNick VARCHAR2(255),
    LETTER_TITLE VARCHAR2(255), 
    LETTER_CONTENTS VARCHAR2(3000),
    LETTER_DATE DATE,
    LETTER_VIEW VARCHAR2(10)
);

CREATE SEQUENCE LETTER_SEQ
 INCREMENT BY 1  
 START WITH 1    
 MAXVALUE 9999    
 NOCYCLE;
```

## 4. **게시판(BOARD_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|게시판번호|BOARD_NO|NUMBER|PK|
|카테고리|BORD_CATEGORY|VARCHAR2|NOT NULL|
|글 제목|BOARD_TITLE|VARCHAR2|NOT NULL|
|글 내용|BOARD_DE|VARCHAR2|NOT NULL|
|작성자|USER_ID|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|작성일|BOARD_DATE|DATE| |
|조회수|BOARD_VIEW|NUMBER|NOT NULL|
|이미지URL|IMAGEURL|VARCHAR2| |
```sql
CREATE TABLE BOARD_TB(
    BOARD_NO NUMBER PRIMARY KEY,
    BOARD_TITLE VARCHAR2(255) NOT NULL,
    BOARD_CATEGORY VARCHAR2(255) NOT NULL ,
    BOARD_DE VARCHAR2(4000) NOT NULL,
    USER_ID VARCHAR2(255) REFERENCES USER_INFO_TB(USER_ID),
    BOARD_DATE DATE,
    BOARD_VIEW NUMBER NOT NULL
);

CREATE SEQUENCE BOARD_SEQ
 INCREMENT BY 1  
 START WITH 1    
 MAXVALUE 9999    
 NOCYCLE;        
```

## 4. **친구 (FRIEND_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|보낸 친구|FRIEND_SEND_ID|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|받은 친구|FRIEND_RECEIVE_ID |VARCHAR2|FK USER_INFO_TB(USER_ID)|
|수락여부|FRIEND_ACCEPT|VARCHAR2| |
```sql
CREATE TABLE FRIEND_TB(
	  FRIEND_SEND_ID VARCHAR2(255),
	  FRIEND_RECEIVE_ID VARCHAR2(255),
	  FRIEND_ACCEPT VARCHAR2(10)
);
```
## 5. **채팅 (CHAT_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|모임번호|MEETING_NO|NUMBER|FK MEETING_TB(MEETING_NO)|
|작성자|USER_ID|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|닉네임|USER_NICK|VARCHAR2|FK USER_INFO_TB(USER_NICK)|
|내용|CHAT_DE|VARCHAR2| |
|작성일|CHAT_DATE|DATE| |
```sql
CREATE TABLE CHAT_TB(
	   MEETING_NO NUMBER(4) REFERENCES MEETING_TB(MEETING_NO),
     USER_ID VARCHAR2(255) REFERENCES USER_INFO_TB(USER_ID),
     CHAT_DE VARCHAR2(4000),
     CHAT_DATE DATE,
);
```
## 6. **댓글(COMMENTLIST_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|댓글 번호|COMMENT_NO|NUMBER|PK|
|댓글 내용|COMMENT_DETAILL|VARCHAR2|NOT NULL|
|댓글 달린 페이지|BOARD_NO|NUMBER|FK BOARD_TB(BOARD_NO)|
|유저 정보|COMMENT_ID|VARCHAR2|NOT NULL|
|댓글작성일|COMMENT_DATE|DATE|NOT NULL|
```sql
CREATE TABLE COMMENTLIST (
    COMMENT_NO NUMBER PRIMARY KEY,
    COMMENT_DETAILL VARCHAR2(4000) NOT NULL,
    BOARD_NO NUMBER NOT NULL,
    COMMENT_DATE DATE NOT NULL,
    CONSTRAINT fk_board FOREIGN KEY (BOARD_NO) REFERENCES BOARD_TB (BOARD_NO)
);

CREATE SEQUENCE COMMENT_SEQ
 INCREMENT BY 1  
 START WITH 1    
 MAXVALUE 999    
 NOCYCLE;
```
## 7. **스케쥴(SCHEDULE_TB)**
이름|컬럼명|자료형|제약 조건|
| --- | --- | --- | --- |
|모임번호|MEETING_NO|NUMBER|FK MEETING_TB(MEETING_NO)|
|게시판번호|SCHEDULE_NO|NUMBER|PK|
|글 제목|SCHEDULE_TITLE|VARCHAR2| |	
|글 내용|SCHEDULE_CONTENTS|VARCHAR2| |	
|작성자|USER_ID|VARCHAR2|FK USER_INFO_TB(USER_ID)|
|작성일|BOARD_DATE|DATE| |
|일정|SCHEDULE_DATE|DATE| |
```sql
  CREATE TABLE SCHEDULE_TB(
    MEETING_NO	NUMBER(4),	
		SCHEDULE_NO	NUMBER(4),	
		SCHEDULE_TITLE	VARCHAR2(250),	
		SCHEDULE_CONTENTS	VARCHAR2(250),	
		USER_ID	VARCHAR2(250),
		BOARD_DATE	DATE,
		SCHEDULE_DATE	DATE	
);

CREATE SEQUENCE SCHEDULE_SEQ
 INCREMENT BY 1  
 START WITH 1    
 MAXVALUE 9999    
 NOCYCLE;
```
# 9. 프로젝트 진행

https://docs.google.com/spreadsheets/d/1Fkr2t1Z2e-jvBHXsHzg7cwaRmYuG7w661nOFxeGN4Ss/edit?gid=0#gid=0


# 10. 참고자료

## GitHub url

## Team
|<img src="![image](https://github.com/user-attachments/assets/9bff88fd-bc20-4372-97d6-b415031a63dd)
" width="150" height="150"/>|<img src="![image](https://github.com/user-attachments/assets/5b660abb-8f42-4960-9bb7-e7fbee18d69f)
" width="150" height="150"/>|<img src="[https://avatars.githubusercontent.com/u/49334905?v=4](https://avatars.githubusercontent.com/u/161570931?v=4)" width="150" height="150"/>|
|:-:|:-:|:-:|:-:|
kimfjd<br/>[@kimfjd](https://github.com/kimfjd)|ungyeolyang<br/>[@ungyeolyang](https://github.com/ungyeolyang)|KimDoyoung<br/>[@KimDoyoung](https://github.com/KDoZero)

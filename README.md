# 마구마구 도감 (수정중)

도감을 이용하여 자신만의 가상 라인업을 만들어보세요.

### 사용방법

1. 마구마구 게임 내에 존재하는 <도감>에서 자신이 만들기 원하는 덱을 스크린샷하여 저장
2. load 폴더에 옮기고 Ma9Ma9.ipyhb 에서 '#.png' 부분을 수정하여 실행
(현재는 예시로 국가대표 카드만 저장되어 있음)
3. main 폴더에 있는 index.html을 실행
4. 검색엔진을 사용하여 카드 검색 후 자신이 원하는 타순을 꾸린다.
(3,4번 항목은 업데이트 예정)

### 업데이트 예정
- 국가대표팀 외에 모든 팀 카드 불러오기
- main 폴더에 존재 할 도감 웹사이트 만들기 (html, css, js)
- 선수 정보 표기

### Dependency

- python
- openCV
- numpy
- pytesseract
- PIL

## 코드 살펴보기

```python
x_start = [509, 633, 757, 881, 1005, 1129, 1253]
y_start = [205, 373, 541, 709]
img = Image.open('#.png')
char = 1
area2 = (20, 141, 100, 160)
```
게임 내 스크린샷을 불러와 카드가 있는 위치의 좌표 및 범위 저장

```
for i in x_start:
    for j in y_start:
        try:
            area = (i,j,i+118,j+162)
            cropped_img = img.crop(area)
            dst = cropped_img.crop(area2)
            result = pytesseract.image_to_string(dst, lang='kor')
```
for 문을 통해 최대 28장의 카드를 모두 잘라내어 카드 선수 연도 및 이름으로 저장을 시도
(openCV를 통해 한글 파일명으로 저장이 불가능해 복잡한 과정을 거침)

```
            if result != '':
                name = '%s.png' % result
                cropped_img.save(name)
                
            else:
                name = '06 '+str(char)+'.png'
                dst.save(name)
                cv2_img = cv2.imread(name)
                gray = cv2.cvtColor(cv2_img, cv2.COLOR_BGR2GRAY)
                result = pytesseract.image_to_string(gray, lang='kor')
```
tesseract OCR을 통해 카드에 쓰여진 연도와 선수 이름을 인식 시도, 인식률을 높임

```
                    if result != '':
                        name = '%s.png' % result
                        cropped_img.save(name)
                        
                    else:
                        cropped_img.save(name)
                        char = char + 1
                        
       except:
            cropped_img.save('06 '+str(char)+'.png')
            char = char + 1
```
인식을 모두 실패하거나 오류가 발생할 경우, 연도로만 자동 저장 후 수동으로 이름을 설정

> Copyright 2020 Hipass7 all rights reserved.

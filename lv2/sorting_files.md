# 문제

세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 `ver-10.zip`이 `ver-9.zip`보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 ["img12.png", "img10.png", "img2.png", "img1.png"]일 경우, 일반적인 정렬은 ["img1.png", "img10.png", "img12.png", "img2.png"] 순이 되지만, 숫자 순으로 정렬된 ["img1.png", "img2.png", "img10.png", img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. `0`부터 `99999` 사이의 숫자로, `00000`이나 `0101` 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

| 파일명 | head | number | tail |
| - | - | - | - |
| foo9.txt | foo | 9 | .txt |
| foo010bar020.zip | foo | 010 | bar020.zip |
| F-15 | F- | 15 | (빈 문자열) |

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. `MUZI`와 `muzi`, `MuZi`는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.

## 제한사항

입력으로 배열 `files`가 주어진다.

- `files`는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (`muzi1.txt`, `MUZI1.txt`, `muzi001.txt`, `muzi1.TXT`는 함께 입력으로 주어질 수 있다.)

## 예제

| 입력 | 출력 | 
| - | - | 
| ["img12.png", "img10.png", "img02.png", "img1.png", "IMG01.GIF", "img2.JPG"] | ["img1.png", "IMG01.GIF", "img02.png", "img2.JPG", "img10.png", "img12.png"] |
| ["F-5 Freedom Fighter", "B-50 Superfortress", "A-10 Thunderbolt II", "F-14 Tomcat"] | ["A-10 Thunderbolt II", "B-50 Superfortress", "F-5 Freedom Fighter", "F-14 Tomcat"] |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 완전 탐색을 사용합니다.
    - 처음으로 등장한 숫자가 시작하는 지점, 끝나는 지점을 체크하면 다음 3가지 경우로 나눌 수 있습니다.
        - head, number, tail 존재
        - head, number 존재
        - head 존재
    - 결과적으로 (head, number, tail) 형식의 튜플을 생성합니다.
    - 정렬 기준은 1) head - 대소문자 상관 없음, 2) number - 앞에 붙는 00 상관 없음이므로 이에 맞게 각각 소문자, 정수형으로 변환 후 2) → 1) 순으로 정렬합니다(정렬 기준이 여러 개인 경우 역순으로 정렬해야 함).

```python
def separate(file):
    num_start = -1
    num_end = -1
    
    for i in range(len(file)):
        if num_start == -1 and file[i] in [str(x) for x in range(10)]:
            num_start = i
            continue
        if num_start != -1 and num_end == -1 and file[i] not in [str(x) for x in range(10)]:
            num_end = i
            break
    
    if num_start != -1 and num_end != -1: # head, number, tail이 있음
        return file[:num_start], file[num_start:num_end], file[num_end:]
    elif num_end == -1: # head, number만 있음
        return file[:num_start], file[num_start:], ''
    else: # head만 있음
        return file, '', ''
    
def solution(files):
    return list(map(lambda x: x[0]+x[1]+x[2], sorted(sorted(list(map(separate, files)), key=lambda x: int(x[1])), key=lambda x: x[0].lower())))
```
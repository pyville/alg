# 방금그곡

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/17683)

[돌아가기](/../)

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 “`(None)`”을 반환한다.

## 제한 사항

입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 `1`개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 `100`개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 '`,`'로 구분된 문자열이다.
    - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
    - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
    - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.

## 예제

| m | musicinfos | answer |
| - | - | - |
| "ABCDEFG" | ["12:00,12:14,HELLO,CDEFGAB", "13:00,13:05,WORLD,ABCDEF"] | "HELLO" |
| "CC#BCC#BCC#BCC#B" | ["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"] | "FOO" |
| "ABC" | ["12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"] | "WORLD" |

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.

세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다.

## 풀이

python을 이용한 풀이는 아래와 같습니다.

- 문자열 조작을 사용합니다.
    - hash_replace 함수를 통해 '#'이 붙은 문자열은 오류 발생의 가능성이 있어 A~G에 해당되지 않는 문자열로 치환합니다.
    - duration 함수를 통해 음악이 실제로 재생된 길이를 계산합니다.
    - musicinfos 리스트는 (길이, 제목, 실제로 재생된 음악) 의 튜플로 변환하고, 길이가 긴 순으로 정렬합니다.

```python
def hash_replace(s):
    return s.replace('C#', 'H').replace('D#', 'I').replace('F#', 'J').replace('G#', 'K').replace('A#', 'M')

def duration(t1, t2):
    if t1[:2] == t2[:2]: # 시 부분이 같은 경우
        return int(t2[3:]) - int(t1[3:]) # 분의 차이 반환
    else: # 시 부분이 다른 경우
        return 60 * (int(t2[:2]) - int(t1[:2])) + int(t2[3:]) - int(t1[3:]) # 시, 분의 차이 반환
    pass

def solution(m, musicinfos):
    m = hash_replace(m)
    musicinfos = [(duration(musicinfo[0], musicinfo[1]), musicinfo[2], hash_replace(musicinfo[3])) for musicinfo in list(map(lambda x: x.split(','), musicinfos))]
    new_musicinfos = []
    for musicinfo in musicinfos:
        dur, title, melody = musicinfo
        tmp = ''
        for i in range(dur):
            tmp += melody[i % len(melody)]
        new_musicinfos.append((dur, title, tmp))
        
    new_musicinfos.sort(key=lambda x: x[0], reverse=True)
    
    for musicinfo in new_musicinfos:
        _, title, melody = musicinfo
        if melody.find(m) != -1: return title
        
    return "(None)"
```
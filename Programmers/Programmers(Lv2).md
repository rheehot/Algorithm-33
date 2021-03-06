# Programmers(Lv2)

### 124나라의 숫자

```python
def solution(n):
    answer = ''
    while n > 0:
        if n%3 == 1:
            answer += '1'
        elif n%3 == 2:
            answer += '2'
        else:
            answer += '4'
        n = (n-1)//3
    return answer[::-1]
```



### 기능개발

```python
def solution(progresses, speeds):
    answer = []
    #candidate - 일을 마치기까지 소요되는 시간을 담을 배열
    candidate = []
    #100% - progresses(%) // speeds 계산으로 일을 마칠 때 까지 걸리는 시간을 구한다.
    for i in range(len(progresses)):
        candidate.append((100-progresses[i])//speeds[i])
	#첫 번째 값부터 비교 - pop(0)을 할 경우 생겨나는 비효율적인 메로리사용, 시간복잡도를 
    #줄이기 위해서 idx를 이용
    num = candidate[0]
    #num의 숫자가 있을 때 같이 배포할 수 있는 작업의 수를 구하기위한 변수
    cnt = 1
    #1번부터 마지막까지 탐색
    for i in range(1,len(candidate)):
        #num이 더 크면 동시에 배포할 수 있으므로 cnt += 1
        if num >= candidate[i]:
            cnt += 1
        #num이 더 작으면 동시 배포가 불가능하므로 cnt는 answer에 넣어주고
        #cnt값과 num을 갱신
        else:
            answer.append(cnt)
            cnt = 1
            num = candidate[i]
    #반복문을 끝마치고 남은 cnt를 answer에 추가
    answer.append(cnt)
    return answer
```



### 탑

```python
def solution(heights):
    N = len(heights)
    answer = []
    re_heights = heights[::-1]
    for send in range(N):
        for receive in range(send+1,N):
            if re_heights[send] < re_heights[receive]:
                answer.append(N-receive)
                break
        else:
            answer.append(0)
    answer = answer[::-1]
    return answer
```



### 전화번호 목록

```python
def solution(phone_book):
    for i in range(len(phone_book)-1):
        for j in range(i+1, len(phone_book)):
            if phone_book[i] == phone_book[j][:len(phone_book[i])]:
                return False
            if phone_book[j] == phone_book[i][:len(phone_book[j])]:
                return False
    return True
```



### 최댓값과 최솟값

```python
def solution(s):
    answer = ''
    numbers = list(map(int,s.split()))
    print(numbers)
    answer += str(min(numbers))+' '+str(max(numbers))
    return answer
```



### 영어 끝말잇기

```python
def solution(n, words):
    first = words[0]
    said = [first]
    for i in range(1,len(words)):
        if first[-1] == words[i][0] and words[i] not in said:
            said.append(words[i])
            first = words[i]
        else:
            answer = [len(said)%n+1, len(said)//n+1]
            break
    else:
        answer = [0,0]
    return answer
```



### 멀쩡한 사각형

```python
#수학적접근의 중요성!
def solution(w,h):
    big = max(w,h)
    small = min(w,h)
    for i in range(small,0,-1):
        if not big %i and not small%i:
            gcd = i
            break
    print(gcd)
    return w*h-(w+h-gcd)
```



### 스킬트리

```python
def solution(skill, skill_trees):
    answer = 0
    for sample in skill_trees:
        check = [0] * len(skill) #스킬 이수여부 확인
        for course in sample:
            if course not in skill: #관련없는 스킬 continue
                continue
            x = skill.index(course) #몇 번째 스킬인지 x변수에 담기
            if not x and not sum(check[1:]): #첫 번째 스킬(index=0)이면서 뒤의 스킬을 배우지 않았으면
                check[x] = 1                 #첫 번째 스킬 이수
            elif x and sum(check[:x]) == x:  #n번째 스킬일 때 자기 이전에 배운스킬을 다 이수 했으면(선행스킬을 다 이수했으면 x와 같은값이 되야 함.)
                check[x] = 1                 #n번째 스킬이수
            else:break                       #위 두 사건에 해당안되면 break
        else:answer+=1                       #break문 없이 for문을 통과하였다면 answer += 1
  return(answer)
```



### 쇠막대기

```python
def solution(arrangement):
    answer = 0
    #이전에 들어온 값이 무엇인지 확인하는 변수 before
    #지금까지의 "("[막대기]수를 세는 변수 count
    count = 0
    for v in arrangement:
        #막대기 시작 or 레이저시작
        if v == "(":
            count += 1
            before = "("
        #레이저 끝(레이저 발사)
        elif v == ")" and before == "(":
            count -= 1
            before = ")"
            answer += count
        #막대기 끝
        else:
            count -= 1
            before = ")"
            answer += 1
    return answer

"""
다른방법)
새로운 배열을 만든 후 주어진 arrangement에서 레이저에 해당하는 '()'을 미리 표현하고 '('와 ')'로 막대기만 세주는 방법을 생각함.
→ 또 다른 배열을 만들어 반복문으로 찾을필요 없이 replace 함수를 이용하여arrangement.replace('()','0') 다음과 같이 표현해주면 레이저 발사구간을 쉽게 바꿔줄 수 있다.
"""
```



### 프린트

문제 좀 제대로 이해하자...후...

```python
def solution(priorities, location):
    from collections import deque
    value = priorities[location] #찾으려는 문서의 중요도
    more = [] #찾으려는 문서의 중요도보다 높은 문서를 담을 리스트
    queue = deque() #사용할 queue
    #priorities를 큐로 만드는 파트
    for i in range(len(priorities)):
        if priorities[i] > value:more.append(i)
        queue.append((i, priorities[i]))
    #찾으려는 문서의 중요도보다 높은 문서를 담는 파트
    for i in priorities:
        if i > value:more.append(i)
    more.sort(reverse=True)
    answer = len(more)
    #찾으려는 중요도보다 높은 문서를 빼내는 파트
    idx = 0
    while idx < len(more):
        test = queue.popleft()
        if test[1] == more[idx]:
            idx += 1
        else:
            queue.append(test)
    #자신과 같은 우선순위가 있는지 확인하는 파트
    flag = True
    while flag:
        test = queue.popleft()
        if test[1] == value:
            answer += 1
        if test[0] == location:
            flag = False
        else:
            queue.append(test)
    return answer
```



### 가장 큰 수

```python
Solution 1 : 재귀
샘플 테스트 케이스의 경우 input의 크기가 작기 때문에 통과하였으나
테스트 케이스의 경우 문자열의 길이가 매우크면 시간초과 및 런타임에러가 발생...
# def perm(n, k, numbers):
#     global answer
#     if n == k:
#         if int(''.join(numbers)) > answer:
#             answer = int(''.join(numbers))
#     else:
#         for i in range(k, n):
#             numbers[k], numbers[i] = numbers[i], numbers[k]
#             perm(n, k+1, numbers)
#             numbers[k], numbers[i] = numbers[i], numbers[k]
# 
# def solution(numbers):
#     global answer
#     numbers = list(map(str, numbers))
#     perm(len(numbers), 0, numbers)
#     print(answer)
#     return None
# answer = 0
# numbers = [1000, 555, 464, 489, 239, 193, 134, 125, 154, 542, 75, 978, 67, 456]
# solution(numbers)

Solution 2 : 문자열 접근
    
def solution(numbers):
    answer = '' #결과를 담을 변수
    strnum = list(map(str, numbers)) #리스트의 int형을 string형으로 변환
    same_length = [[i, i * (12//len(i)) ] for i in strnum] #문자로 변환후 대소비교하기위한 세팅
    #1~100까지숫자를 문자로 나타냈을 대 길이가 1~4이므로 길이 통일을 위해 12(최소공배수)값
  
    same_length.sort(key=lambda x:x[1], reverse=True) #1번째 인덱스값을 기준으로 정렬
    for i in same_length:answer += i[0] #정렬후 원래 값인 0번째 인덱스를 answer에 더해준다.
    answer = str(int(answer)) #만약, 숫자가 0만 나올 경우 [0, 0, 0, 0] 0000으로 출력되면 안되므로 형변환이 필요하다.(11번 테스트케이스...)
    return answer

	#+) 세자리까지만 비교해도 충분하다... 1000은 100보다 낮은 우선순위 
    #따라서, same_length 리스트를 만들필요 없이 strnum.sort(key=lambda x:x*3, reverse=True)를 해주면 된다.
```



### 다리를 지나는 트럭

```python
def solution(bridge_length, weight, truck_weights):
    answer = 0

    from collections import deque
    trucks = deque(truck_weights)
    onbridge = deque()
    while trucks:
        if len(onbridge) == bridge_length:
            onbridge.popleft()
        if trucks[0] + sum(onbridge) <= weight:
            onbridge.append(trucks.popleft())
            answer += 1
        else:
            while len(onbridge) < bridge_length:
                onbridge.append(0)
                answer += 1
    return answer+bridge_length
```



### 문자열 압축

```python
def check(num, word, answer):
    res = ''
    now = word[:num]
    cnt = 1
    for i in range(num, len(word), num):
        if now == word[i:i + num]:
            cnt += 1
        else:
            if cnt == 1:
                res += now
                now = word[i:i+num]
            else:
                res += str(cnt) + now
                now = word[i:i + num]
                cnt = 1
        if len(res) >= answer:
            return answer
    if cnt == 1:
        res += now
    else:
        res += str(cnt) + now
    return len(res)

def solution(s):
    answer = len(s)
    for i in range(1, len(s)//2 + 1):
        result = check(i, s, answer)
        if answer > result:
            answer = result
    return answer
```



### 삼각 달팽이

```python
def solution(n):
    answer = []
    candidates = [[0]*n for _ in range(n)]

    direction = 'd'

    row = 0
    col = 0
    num = 1
    cnt = n
    while(cnt):
        if direction == 'd':
            for i in range(cnt):
                if not candidates[row][col]:
                    candidates[row][col] = num
                    row += 1
                    num += 1
                else:
                    break
            row -= 1
            col += 1
            direction = 'r'
            cnt -= 1
        elif direction == 'r':
            for i in range(cnt):
                if not candidates[row][col]:
                    candidates[row][col] = num
                    col += 1
                    num += 1
                else:
                    break
            row -= 1
            col -= 2
            direction = 'u'
            cnt -= 1
        else:
            for i in range(cnt):
                if not candidates[row][col]:
                    candidates[row][col] = num
                    row -= 1
                    col -= 1
                    num += 1
                else:
                    break
            row += 2
            col += 1
            direction = 'd'
            cnt -= 1
            
    for li in candidates:
        for num in li:
            if num:
                answer.append(num)
    return answer
```



### 구명보트

```python
def solution(people, limit):
    answer = 0
    people.sort(reverse=True)
    front, back = 0, len(people)-1
    while front <= back:
        if people[front] + people[back] <= limit:
            front += 1
            back -= 1
        else:
            front += 1
        answer += 1
    return answer
```



### H-index

```python
def solution(citations):
    answer = 0
    candidates = [0] * (max(citations)+1)
    for i in citations:
        candidates[i] += 1
    for j in range(len(candidates)-1, -1, -1):
        if sum(candidates[j:]) >= j:
            answer = j
            break
    return answer
```



### 타겟 넘버

> DFS/BFS라는 힌트를 받았지만 어떻게 풀어야할지 막막했던 문제
>
> DFS 방법 - 재귀를 이용하여 풀었다.
>
> 또한 함수 안의 함수 형태에서 변수의 사용범위에 대해서 고민이 많았는데
>
> 굳이 인자로 넘기지 않고 **nonlocal**로 처리할 수 있음을 배웠다.

```python
def solution(numbers, target):
    answer = 0
    def dfs(step, total):
        if step == len(numbers):
            if total == target:
                nonlocal  answer
                answer += 1
        else:
            dfs(step+1, total+numbers[step])
            dfs(step+1, total-numbers[step])
    dfs(0, 0)
    print(answer)
```



### JadenCase 문자열 만들기

```python
def solution(s):
    answer = ''
    empty = ''
    for word in s:
        if not empty and word.isalpha():
            empty += word.upper()
        elif word == ' ':
            answer += empty + ' '
            empty = ''
        else:
            empty += word.lower()
    return answer + empty
```



### 올바른 괄호

```python
def solution(s):
    left = 0
    for i in s:
        if i == '(':
            left += 1
        else:
            if left == 0:
                return False
            else:
                left -= 1
    return True if not left else False 
```



### 더 맵게

> 풀이 1

```python
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    while len(scoville) > 1:
        min_1 = heapq.heappop(scoville)
        if min_1 < K:
            min_2 = heapq.heappop(scoville)
            heapq.heappush(scoville, min_1 + min_2*2)
            answer += 1
        else:
            heapq.heappush(scoville, min_1)
            break
    return answer if scoville[0] >= K else -1
```

> 풀이 2

```python
import heapq
def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    while True:
        min_1 = heapq.heappop(scoville)
        if min_1 < K:
            if not len(scoville):
                return -1
            min_2 = heapq.heappop(scoville)
            heapq.heappush(scoville, min_1 + min_2*2)
            answer += 1
        else:
            heapq.heappush(scoville, min_1)
            break
    return answer
```



### 위장

```python
def solution(clothes):
    answer = 1
    total = dict()
    for clothe in clothes:
        if clothe[1] not in total:
            total[clothe[1]] = 1
        else:
            total[clothe[1]] += 1
    if len(total) == 1:
        return len(clothes)
    else:
        for i in total.values():
            answer *= i+1
    return answer - 1
```



### 카펫

```python
def solution(brown, yellow):
    total = brown + yellow # 카펫의 총 면적
    for i in range(3, brown): # 최소 3부터 시작
        if not total % i: # 면적으로 만들 수 있는 행, 열이 존재
            col = max(i, total//i)
            row = min(i, total//i)
            if (col * 2 + row * 2) - 4 == brown: # brown 개수 일치여부
                return [col, row]
```



### 피보나치 수

```python
def solution(n):
    a, b = 0, 1
    for i in range(n):
        a, b = b, a + b
    return a % 1234567
```



### 소수 찾기

>파이썬의 내장 라이브러리인 itertools를 활용

```python
from itertools import permutations
def check(num):
    if num < 2:
        return False
    else:
        for i in range(2, int(num**(0.5))+1):
            if not num%i:
                return False
    return True

def solution(numbers):
    answer = 0
    candidates = []
    for i in range(1, len(numbers)+1):
        for n in list(map(''.join, permutations(numbers, i))):
            if int(n) not in candidates:
                candidates.append(int(n))            
                if check(int(n)):
                    answer += 1
    return answer
             
```



### 오픈채팅방

```python
def solution(record):
    users = dict()
    userid = []
    answer = []
    for re in record:
        info = re.split()
        if info[0] == "Enter":
            users[info[1]] = info[2]
            userid.append(info[1])
            answer.append("님이 들어왔습니다.")
        elif info[0] == "Change":
            users[info[1]] = info[2]
        else:
            answer.append("님이 나갔습니다.")
            userid.append(info[1])
    for i in range(len(userid)):
        answer[i] = users[userid[i]] + answer[i]
    return answer
```



### 최솟값 만들기

```python
def solution(A,B):
    answer = 0
    A.sort()
    B.sort(reverse=True)
    for i in range(len(A)):
        answer += A[i] * B[i]
    return answer

solution([1, 4, 2], [5, 4, 4])
```



> 다른사람의 풀이 - zip을 사용!!!

```python
def getMinSum(A,B):
    return sum(a*b for a, b in zip(sorted(A), sorted(B, reverse = True)))
```



### 큰 수 만들기

```python
def solution(number, k):
    answer = ''
    stack = [number[0]]
    while k:
        for i in range(1, len(number)):
            if int(stack[-1]) >= int(number[i]):
                stack.append(number[i])
            else:
                while len(stack) and k and int(stack[-1]) < int(number[i]):
                    stack.pop()
                    k -= 1
                stack.append(number[i])
        else:
            return ''.join(stack[:len(number)-k])
    return ''.join(stack)
```



### 다음 큰 숫자

```python
def solution(n):
    binary = bin(n)[2:]
    cnt = binary.count('1')
    if len(binary) == cnt:
        return int('0b' + '10'+'1'*(cnt-1),2)
    else:
        if binary[-1] == '1':
            for i in range(len(binary)-1, 0, -1):
                if binary[i] == '0':
                    return int('0b' + binary[:i] + '10' + binary[i+2:], 2)
        else:
            flag = False
            chk = 0
            for i in range(len(binary)-1, 0, -1):
                if binary[i] == '0':
                    flag = True
                if binary[i] == '1' and flag and chk == 0:
                    chk = 1
                    flag = False
                if binary[i] == '0' and chk == 1:
                    idx = i
                    count_1 = binary[idx+2:].count('1')
                    count_0 = len(binary[idx+2:]) - count_1
                    return int('0b' + binary[:idx] + '10' + '0' * count_0 + '1' * count_1, 2)

            else:
                count_1 = binary.count('1')
                count_0 = binary.count('0')
                return int('0b' + '10' + '0'* (count_0) + '1' * (count_1-1), 2)
```



> 다른사람의 풀이

```python
def solution(n):
    cnt = bin(n).count('1')
    for i in range(n+1, 1000001):
        if bin(i).count('1') == cnt:
            return i
```



### 괄호 변환

```python
def divide(p):
    last = 0
    flag = True
    left, right = 0, 0
    for i in range(len(p)):
        if p[i] == '(':
            left += 1
        else:
            right += 1
            if right > left:
                flag = False
        if left == right:
            last = i
            break
    else:
        last = len(p)-1
    return p[:last+1], p[last+1:], flag

def recursive(p):
    if p == '':
        return ''
    u, v, flag = divide(p)
    if flag:
        return u + recursive(v)
    else:
        return '(' + recursive(v) + ')' + ''.join([')' if i == '(' else '(' for i in u[1:-1]])

def solution(p):
    return recursive(p)
```



### 숫자의 표현

```python
def solution(n):
    answer = 1
    numbers = [i for i in range(1, n)]
    print(numbers)
    for i in range(0, len(numbers)-1):
        for j in range(i+1, len(numbers)):
            res = sum(numbers[i:j+1])
            if res == n:
                answer += 1
            if res > n:
                break
    return answer
```



### N개의 최소공배수

```python
def divide(num, n):
    cnt = 0
    while True:
        if n % num:
            return cnt, n
        else:
            n //= num
            cnt += 1
            
def solution(arr):
    answer = 1
    num = 2
    while sum(arr) != len(arr):
        count = 0
        for i in range(len(arr)):
            if not arr[i] % num:
                cnt, res = divide(num, arr[i])
                arr[i] = res
                count = max(count, cnt)
        answer *= (num ** count)
        num += 1
    return answer
```


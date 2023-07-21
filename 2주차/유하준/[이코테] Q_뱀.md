### 문제 풀이 핵심 아이디어 :
    1. 먼저 그래프(맵)을 모두 0으로 채워준다.

    2. 사과 위치는 모두 2로 채워준다.

    3. 앞으로 뱀이 차지하고 있는 부분은 1로 채워줄 것이다.

    4. 뱀이 이동할 때 마다 머리와 꼬리는 한 칸씩 전진한다. (즉, 몸의 길이는 그대로이다.)

    5. 이동했을 때 사과를 먹으면 머리는 전진하지만 꼬리는 그대로이다. (즉, 몸의 길이가 한 칸 늘어난다.)

    6. 방향 전환을 해야 하는 타이밍에 맞춰 L이면 왼쪽, D이면 오른쪽으로 방향전환을 한다.

    
### 코드의 시간복잡도와 그 이유 :
    O(n^2) : While 반복문 안에 덱을 사용한 루프가 존재


### 코드 :
```python
from collections import deque
 
n = int(input())
graph = [[0] * n for _ in range(n)]
graph[0][0] = 1

k = int(input())
for _ in range(k):
    a, b = map(int, input().split())
    graph[a - 1][b - 1] = 2

l = int(input())
turns = deque()
for _ in range(l):
    x, c = input().split()
    x = int(x)
    turns.append((x, c))


directions = [(0, 1), (-1, 0), (0, -1), (1, 0)]
direction_index = 0

def turn(c, direction_index):
    if c == 'D':
        direction_index -= 1
    else:
        direction_index += 1
    
    return (direction_index % 4)

# 'L':+1 , 'D':-1
res = 0
x, y = 0, 0
queue = deque()
queue.append((0, 0))

while True:
    res += 1
    dx, dy = directions[direction_index]
    
    nx = x + dx
    ny = y + dy
    
    if nx < 0 or nx >= n or 0 or ny < 0 or ny >= n:
        break
        
    # 꼬리가 지나간 부분 deque를 통해 처리 필요
    if graph[nx][ny] == 2:
        graph[nx][ny] = 1
        queue.append((nx, ny))
    
    elif graph[nx][ny] == 0:
        graph[nx][ny] = 1
        queue.append((nx, ny))
        tx, ty = queue.popleft()
        graph[tx][ty] = 0
        
    else:
        break
    
    if turns:
        if turns[0][0] == res:
            t, c = turns.popleft()
            direction_index = turn(c, direction_index)
    
    x, y = nx, ny
    

print(res)
```
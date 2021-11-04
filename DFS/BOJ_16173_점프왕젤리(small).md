```python
def dfs(row, col):
    global ans
    #   아래  오른쪽
    dr = [1, 0]
    dc = [0, 1]
	# 맵의 끝까지 갔고, 방문까지 완료했다면 harharu
    if row == N -1 and col == N -1 and visited[row][col] == 1:
        ans = "HaruHaru"
        return ans

    for i in range(2):
        # MAP 에 주어진 한 번에 이동가능한 칸 수 만큼 곱해서 더해주기
        next_dr = row + dr[i] * MAP[row][col]
        next_dc = col + dc[i] * MAP[row][col]
        if 0 <= next_dr < N and 0 <= next_dc < N:
            if visited[next_dr][next_dc] == 1:
                continue
            else:
                visited[next_dr][next_dc] = 1
                dfs(next_dr, next_dc)

N = int(input())
MAP = [list(map(int, input().split())) for _ in range(N)]
visited = [[0] * N for _ in range(N)]
ans = "Hing"
visited[0][0] = 1
dfs(0,0)

print(ans)
```

next_dr, next_dc 범위 정할 때 0을 포함안시켜서 hing만 나와서 맞왜틀만 무한 시전..
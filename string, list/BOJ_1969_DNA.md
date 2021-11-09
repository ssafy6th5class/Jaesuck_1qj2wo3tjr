```python
N , M = map(int, input().split())
dna_list = [input() for _ in range(N)]
new_dna_list = []   # 세로로 저장하기 위한 새로운 배열 선언
first_letter = ['A', 'C', 'G', 'T'] # 알파벳 순서대로 저장하면 추후 사전적 순서를 신경쓸 필요 없다
many_letter = [0] * 4 # 알파벳의 count 저장하기위한 배열
ans_list = []
Hamming_distance = 0
# 주어진 input을 세로로 읽어서 new_dna_list 에 추가한다.
for i in range(M):
    str = ''
    for j in range(N):
        str += dna_list[j][i]
    new_dna_list.append(str)

# 세로로 저장한 새로운 배열을 순회하며 문자열마다 중복되는 알파벳의 숫자를 구해서 many_letter 에 갱신한다
for i in range(M):
    for j in range(4):
        many_letter[j] = new_dna_list[i].count(first_letter[j])
    # A C G T 각 알파벳의 숫자를 저장한 배열에서 max 값을 구하고, 해당 index 를 구한다.
    many_letter_count = max(many_letter)
    many_idx = many_letter.index(many_letter_count)
    # 위에서 구한 idx를 통해 문자열을 하나씩 구해온다.
    ans_list.append(first_letter[many_idx])

# 입력받은 문자열과 새롭게 구한 dna 문자열 사이의 Hamming_distance 구하기
for i in range(N):
    for j in range(M):
        if dna_list[i][j] != ans_list[j]:
            Hamming_distance += 1

print(''.join(ans_list))
print(Hamming_distance)
```


# baekgeonwoo.github.io
# 모각코

#한국외국어대학교 신찬수교수님의 유튜브 영상과 구름을 통해 알고리즘 학습 및 다음학기 예습이 목표입니다.

# 2021 07 07

오늘은 재귀 알고리즘에 대해서 배웠습니다. a부터 b까지 더하는 함수를 만드는데 함수 이름을 sum이라 하고 sum(a,b)에서 return sum(a,b-1)+b 형식으로 재귀를 하나만 사용하는 방법과
sum(a,b)에서 m = (a+b)//2 라고 한 뒤 sum(a,m)+sum(m+!,b) 하는 방법 두가지를 배웠는데 두 가지 방법 모두 시간복잡도는 Big-O 방식으로 O(n)이었습니다.
그리고 재귀는 return 부분 즉, 재귀함수를 호충하는 부분과 재귀를 끝내는 부분이 중요하다는 것을 배웠습니다.
그 다음 백준문제 1769번을 풀었는데 숫자로 이루어진 문자열을 받아서 3의 배수인지 확인하는 문제였습니다. 문자열을 받아서 1자리가 될때까지 각 자리수를 더한 뒤
이러한 변환을 한 횟수와 3의 배수인지 아닌지를 출력하는 문제였습니다. 처음에는 문자열만을 함수의 매개변수로 사용해서 try-finally 문을 사용하여 두 결과의 순서가 반대로 나와서 고생했지만
재귀함수의 매개변수로 변환횟수를 추가하여 해결할 수 있었습니다. 총 5번 재출만에 통과했습니다. 아래는 제가 작성한 문제코드입니다.

def rec(string, count):
    if len(string) > 1:
        a = map(int, list(string))
        s = 0
        for n in a:
            s += n
        return rec(str(s), count+1)
    else:
        return int(string), count
string = input()
s, c = rec(string, 0)
print(c)
if s%3==0:
    print("YES")
else:
    print("NO")

# 2021 07 14

오늘은 quick select 알고리즘에 대해서 공부했습니다. 선택문제는 특수한 경우와 일반적인 경우로 나눌 수 있는데 특수한 경우는 n개의 값 중에서 최댓값과 최솟값을 구하는 경우와 두번째로 작은값, 큰값을 구하는 것입니다. 이 경우 n-1번 비교를 하여 최댓값, 최솟값을 구할 수 있고, 다시 n-2번 비교하여 그 다음 작거나 큰 값을 구할 수 있습니다. 하지만 이 경우 시간이 오래 걸리고 특수한 경우이기 때문에 일반적이지 않다는 단점이 있습니다. 그 다음으로 일반적인 경우는 n개의 값 중에서 k번째로 작은 값, 큰 값을 구하는 경우인데 k번째로 큰 값을 구하는 경우 작은 값을 구하는 경우와 반대로 하면 됩니다. 먼저 n개의 값들이 L이라는 리스트에 있다고 하고, pivot이라고 기준이 되는 값 p를 정하고(아무거나 정해도 되는데 저는 맨 왼쪽에 있는 값으로 정했습니다) 이 값보다 작은 값들은 A라는 리스트에, p와 같은 값들은 M이라는 리스트에, p보다 큰 값들은 B라는 리스트를 만들어서 넣습니다. 그 다음 k가 A의 크기보다 작거나 같으면 A안에 k번째로 작은 수가 있는 것이므로 재귀적으로 L에 A를 넣고 계속 진행합니다. k가 len(A)+len(M)보다 큰 경우 k번째로 작은 값은 B에 있는 것이므로 재귀적으로 L을 B로 하여 같은 과정을 진행합니다. 앞의 두 경우가 아니면 k번쨰로 값은 M에 있는 것이므로 p를 반환합니다.
저는 n개의 값 중에서 k번쨰로 작은 값을 구하는 문제를 풀었는데 문제의 첫 번째 줄에 n과 k가 주어지고 두 번째 줄에 n개의 값들이 주어집니다. 앞에서 설명한 알고리즘을 코드로 구현하여 문제를 해결했고, 아래는 그 코드입니다.
def quick_select(L,k):
	p = L[0]
	A, B, M = [], [], []
	for x in L:
		if p>x: A.append(x)
		elif p<x: B.append(x)
		else: M.append(x)
	if len(A) >= k: return quick_select(A,k)
	elif len(A) + len(M) < k: return quick_select(B, k-len(A)-len(M))
	else: return p

n, k = [int(e) for e in input().split()]
L = [int(e) for e in input().split()]
print(quick_select(L,k))

# 2021 07 21

오늘은 MoM(Median of Medians) 알고리즘에 대해서 배우고 MoM 알고리즘의 수행시간 분석을 귀납법을 통해 해보았습니다. MoM 알고리즘은 quick select 알고리즘이 worst case일때 수행시간이 O(n^2)이 되는 것을 보완한 알고리즘으로 quick select 알고리즘에서 pivot을 랜덤하게 정해서 수행시간이 길어지는 문제를 pivot을 k번째 값을 찾는데 좀더 효율적이도록 고름으로써 수행시간을 줄인 알고리즘입니다. 먼저 값들을 5개씩 자른 뒤 5개씩 자른 값들의 중간값들의 중간값을 구합니다.(이래서 알고리즘 이름이 median of medians입니다.) 이 중간값을 m이라고 부르겠습니다. 즉, m이 pivot이 되는것입니다. m보다 작은 값들을 A라는 리스트에 m보다 큰 값들을 B라는 리스트에 저장한 뒤 quick select 알고리즘처럼 k번째 값을 구합니다. 이러면 값의 개수를 n이라고 했을 떄 A, B의 크기가 n/4이상 3n/4이하가 되어 A또는 B에 대부분의 값이 몰려 수행시간이 길어지는 경우를 방지할 수 있게 됩니다. 수행시간을 분석한 결과 MoM알고리즘의 값이 n개일 때의 수행시간을 T(n) 이라 하면 T(n) = T(3n/4) + T(n/5) + 11n/5라는 식을 구할 수 있었습니다. T(1) = 1이므로 x<n 일때 T(x) <= cx이 성립한다고 가정하면 T(n) = T(3n/4) + T(n/5) + 11n/5 <= 3cn/4 + cn/5 + 11n/5 <= cn 에서 44<=c 임을 구핤할 수 있으며 T(n) <= 44n이 1일때 성립하고 x<n일때 성립한다고 가정했을 때 x=n일때 성립하는 것이 증명되었으므로 T(n) <= 44n이 성립합니다. 이로써 값이 n개일때 MoM 알고리즘의 수행시간이 O(n)이라는 것을 증명할 수 있었습니다. 아래는 제가 구현한 MoM 알고리즘 코드입니다.

def find_median_five(A):
	n = len(A)
	if n in [1,2]:
		return A[0]
	elif n in [3,4]:
		A.remove(max(A))
		return max(A)
	else:
		if A[0] > A[1]:
			s1, l1 = A[1], A[0]
		else:
			s1, l1 = A[0], A[1]
		if A[2] > A[3]:
			s2, l2 = A[3], A[2]
		else:
			s2, l2 = A[2], A[3]
		r = A[4]
		if l1 > l2:
			if s1 > r:
				if s1 > l2:
					if l2 > r:
						return r
					else:
						return l2
				else:
					return s1
			else:
				if l2 > r:
					return r
				else:
					if l2 > s1:
						return l2
					else:
						return s1
		else:
			if s2 > r:
				if s2 > l1:
					if l1 > r:
						return r
					else:
						return l1
				else:
					return s2
			else:
				if l1 > r:
					return r
				else:
					if l1 > s2:
						return l1
					else:
						return s2
	
def MoM(A, k): # L의 값 중에서 k번째로 작은 수 리턴
	if len(A) == 1: # no more recursion
		return A[0]
	i = 0
	S, M, L, medians = [], [], [], []
	while i+4 < len(A):
		medians.append(find_median_five(A[i: i+5]))
		i += 5
		
	if i < len(A) and i+4 >= len(A): # 마지막 그룹으로 5개 미만의 값으로 구성
		medians.append(find_median_five(A[i:]))
	
	mom = MoM(medians, len(medians)//2)
	for v in A:
		if v < mom:
			S.append(v)
		elif v > mom:
			L.append(v)
		else:
			M.append(v)

	if len(S) >= k : return MoM(S, k)
	elif len(S) + len(M) < k: return MoM(L, k - len(S) - len(M))
	else: return mom

n, k = map(int, input().split())
A = list(map(int, input().split()))
print(MoM(A, k))

# 2021 07 28

오늘은 분할정복법과 분할정복법을 이용한 피보나치수 계산, 카라츠바 알고리즘에 대해서 배웠습니다. 분할정복법이란 큰 문제를 작은 문제로 분할해서 재귀적으로 해결하는 방법을 말합니다. 간단한 예시를 들면 리스트에서 최댓값을 찾을 때 리스트를 절반으로 잘라 2개의 리스트를 만들고 각각의 리스트의 최댓값을 구한 뒤 구한 값들을 비교하여 전체 리스트의 최댓값을 구하는 방법입니다. 이를 이용하여 n번째와 n-1번째 피보나치수를 구해보면 [fn fn-1]T = [1 1/1 0] [fn-1 fn-2]T인 것을 이용하여 [fn fn-1]T = [1 1/1 0]^n-1 [f1 f0]T 임을 구할 수 있고 n번의 행렬곱셈만으로 n번째와 n-1번째 피보나치수를 구할 수 있음을 배웠습니다. 마지막으로 카라츠바 알고리즘은 카라츠바라는 사람이 고안한 알고리즘으로 큰수의 곱셈을 할 때 수들을 절반씩 나누어(자리수 유지를 위해 나중에 10^n을 곱해줍니다. 여기서 n은 수의 길이의 절반) 수들을 u, v라 하면 u = 10^n*x + y, v = 10^n*z + w라 할 수 있고 u*v = x*z*10^2n + (x*w+y*z)*10^n + y*w = x*z*10^2n + ((x+y)(z+w)-x*z-y*w)*10^n + y*w임을 이용해서 곱셈의 쵯수를 x*z*10^2n + (x*w+y*z)*10^n + y*w = x*z*10^2n에서 x*z, x*w, y*z, y*w로 4번이었던 것을 x*z, (x+y)*(z+w), y*w로 3번으로 줄여 수행시간을 줄이는 방법입니다. 이런식으로 u또는 v의 길이가 1이 될 때까지 이 알고리즘을 재귀적으로 반복하면 O(n^2)에서 O(n^log23)(로그2의 3)로 수행시간을 줄일 수 있습니다.
아래는 제가 작성한 코드입니다.
def mult(u, v):
	n1, n2 = len(str(u)), len(str(v))
	if n1 == 1 or n2 == 1:
		return u*v
	n = min(n1,n2)//2
	x, y = int(str(u)[:-n]), int(str(u)[-n:])
	z, w = int(str(v)[:-n]), int(str(v)[-n:])
	xz, yw = mult(x,z), mult(y,w)
	return 10**(2*n)*xz + 10**n*(mult(x+y,z+w)-xz-yw) + yw
	
u = int(input())
v = int(input())
print(mult(u,v))

# 2021 08 04
이번에는 정렬 알고리즘에 대해서 배웠습니다. 기본적인 정렬 알고리즘(쉽고 느린)은 3종류가 있는데 selection, bubble 그리고 insertion이 있습니다. selection 정렬 알고리즘은 리스트에서 [:]에서 최댓값을 구해 맨 뒤에 넣고 [:-1]에서 최댓값을 구해 인덱스 -2에 넣는 것을 반복하는데 이 과정을 n-1번 반복함으로써 리스트를 정렬할 수 있습니다. 그 다음 bubble 정렬 알고리즘은 앞에서부터 2개씩 비교하는데 인덱스로 0과 1을 비교하여 0이 더 크면 1에 두고 그 다음 1과 2를 비교하여 큰 것을 뒤에 놓는 것을 반복하여 최종적으로 맨 뒤에 가장 큰 값을 넣고 [:-1]에서 이를 반복하고, [:-2] ... 식으로 이 과정을 n-1번 반복하여 리스트를 정렬합니다. insertion 정렬 알고리즘은 A라는 리스트가 있다고 하면 A[0]으로 새로운 리스트를 만든 뒤(이를 B라 합니다) A[1]과 B의 요소들을 비교하여 B에서 A[1]이 들어갈 위치를 B의 뒤에서부터 찾아 A[1]을 넣습니다. A[2], A[3]...에 대해서 이를 반복하면 A를 오름차순 정렬한 리스트가 B가 됩니다. 이 세 알고리즘은 모두 수행시간이 O(n^2)입니다.
그 다음으로 가장 빠른 정렬 알고리즘인 quick sort 알고리즘에 대해서 배웠습니다. worst case의 수행시간은 앞의 세 정렬알고리즘과 같이 O(n^2)이지만 평균적으로 가장 빠른 알고리즘입니다. quick sort 알고리즘은 앞서 배웠던 quick select알고리즘을 이용한 알고리즘인데 리스트 A가 있다고 하면 A[0]을 pivot으로 잡고 pivot p보다 작은 값들을 S에 같은 값들을 M에 큰 값들을 L에 넣고,  quick sort 알고리즘을 S와 L에 적용하고 M과 더하여 반환합니다. 즉, return quick_sort(S)+M+quick_sort(L)합니다. 그리고 만약 A의 크기가 1 이하면 A를 반환합니다. 이를 반복하면 A가 정렬됩니다.
아래는 실습겸 사용자로부터 리스트의 요소 개수 n을 받고 리스트에 들어갈 값을 n번 입력받아 리스트 L을 만들고 L을 quick sort 알고리즘으로 정렬하여 반환하는 코드를 작성한 것입니다.
def quick_sort(L):
	if len(L) <= 1:
		return L
	p = L[0]
	A, B, M = [], [], []
	for x in L:
		if p>x: A.append(x)
		elif p<x: B.append(x)
		else: M.append(x)
	return quick_sort(A) + M + quick_sort(B)

n = int(input())
L = []
for _ in range(n):
	L.append(int(input()))
print(quick_sort(L))

# 2021 08 18
이번주 모각코에서는 Merge sort와 Tim sort에 대해서 배웠습니다. Merge sort는 리스트를 작게 쪼개고 쪼갠 부분들을 2개씩 비교하여 정렬하는 방식으로 예를 들어 A라는 리스트에서 쪼개진 B, C라는 리스트가 있고 B=[1,4,7,9], C=[-1,3,5] 라 하면 새로운 빈 리스트 D를 생성하고 B,C의 요소들을 앞에서부터 비교하여 D에 순서대로 -1,1,3,4,5를 넣고 두 리스트중 하나가 끝요소까지 D에 들어가면 for문을 이용해 나머지 아직 끝 요소까지 비교하지 않은 리스트는 당연히 더 크므로 D에 넣습니다. 그런데 실제로는 둘중 어느 리스트의 요소가 더 먼저 끝나는지 알 수 없으므로 for문을 두번 사용하여 두 리스트 모두 남은 부분을 넣어줍니다.(둘중 한 for문만 작동합니다) 그 다음 D에 있는 요소들을 순서대로 A의 위치에 넣어주고 이를 재귀를 통해 반복하면 Merge sort가 끝납니다.
Tim sort는 insertion sort와 Merge sort를 활용한 정렬알고리즘으로 리스트를 적당한 크기로 쪼개고(이 쪼갠 것을 run이라 합니다) 쪼갠 리스트들을 insertion sort 알고리즘을 활용해 오름차순으로 정렬한 후 정렬된 쪼개진 리스트들을 A, B, C, D, E, F ...라 할 때 |A|를 A의 크기라 하면 스택을 만들고 쪼개진 리스트들을 넣는데 |A| + |B| < |C|가 되도록 스택에 쪼개진 리스트들을 넣습니다. 단, 이 조건이 충족되지 않을 경우 새로운 리스트가 들어올 때 크기가 작은 리스트 둘을 merge_sort로 합쳐 정렬한 뒤 규칙에 맞게 되면 stack에 넣습니다. 이러한 것을 반복하면 stack의 맨 위부터 아래까지
|A| + |B| < |C| 이러한 규칙에 부합하게 정렬된 쪼개진 리스트들이 들어가게 되고 위에서부터 2개씩 merge_sort를 사용하여 합친 뒤 리스트를 반환하면 Tim sort가 끝납니다.
아래는 제가 구현한 merge sort 코드입니다.

def merge_sort(A, first, last):
    if first >= last: return
    middle = (first + last)//2
    merge_sort(A, first, middle)
    merge_sort(A, middle+1, last)
    i, j = first, middle+1
    B = []
    while i <= middle and j <= last:
        if A[i] <= A[j]:
            B.append(A[i])
            i += 1
        else:
            B.append(A[j])
            j += 1
    for k in range(i, middle+1): B.append(A[k])
    for k in range(j, last+1): B.append(A[k])
    for k in range(first, last+1): A[k] = B[k-first]

# 2021 08 25
이번주 모각코에서는 동적 계획법을 배웠습니다. 동적 계획법은 큰 문제를 작은 문제들로 나누어 문제를 해결하는 방법으로 4가지 단계를 가집니다. 1.큰 문제를 작은문제로 분할한다. 2.큰문제의 답을 작은 문제의 답을 이용하여 나타낼 수 있는 점화식을 구한다. 3.DP 테이블을을 만든다. 4.정확성을 증명한다. 그런데 배운 내용대로면 1,2,3단계를 거치면 대부분 4단계는 성립한다고 합니다. 3단계의 DP 테이블이란 동적 계획법을 사용할 때 이용하는 리스트로 큰 문제를 분할한 작은 문제들의 답들을 순서대로 저장한 리스트 입니다. 2번의 점화식에 이용할 작은 문제의 답들을 DP테이블에서 가져와 사용하는 것 입니다.
동적 계획법 실습문제로는 최대구간합 문제를 풀었는데 최대구간합이란 어떠한 리스트 A에서 i번 인덱스부터 j번 인덱스까지의 요소들의 합을 sumij라 할 때 A에서 최대의 sumij를 구하는 문제입니다. 저는 이 문제를 총 4가지 방법으로 풀었는데 문제를 푸는데 사용한 방법과 시간복잡도를 나열하면 1.모든ij를 구하여 최댓값을 구하는 방법 O(n^3) 2.prefixsum을 이용한 방법 O(n^2) 3.분할정복법 O(nlog2n)
4.동적계획법(DP) O(n) 1번 방법의 경우 삼중for문을 사용하여 느리고, 2번의 경우는 P[i] = A[0] + A[1] + ... + A[i]인 P라는 배열(prefixsum을 저장할 리스트)을 만들고 sumij = P[j] - P[i-1]임을 이용한 방법으로 sumij를 매번 구하는 1번보다는 빠르지만 여전히 이중for문을 사용하여 느립니다. 3번의 경우는 주어진 리스트 A를 반으로 나누고 재귀를 통해 왼쪽 리스트에서 가장 큰 구간합을 L, 오른쪽에서 가장 큰 구간합을 R, 왼쪽과 오른쪽을 모두 지나는 구간을 M이라 하여 L, M, R 중 최댓값이 최대구간합 문제의 해가 되는 방법입니다. M을 구하는 방법은 가운데 인덱스를 m이라 하고 왼쪽 끝과 오른쪽 끝의 인덱스를 각각 l, r 이라 하면 왼쪽에서 A[m]으로 끝나는 최대구간합과 오른쪽에서 A[m+1]로 시작하는 최대구간합을 구하여 더한 것이 M이 됩니다.
마지막으로 4번 방법은 dp[i]가 A[i]로 끝나는 최대구간합이 되도록 dp를 구성하여 max(dp)가 최대구간합 문제의 답이 되는 방법으로 dp[0] = A[0]라 한 뒤
인덱스를 1부터 n-1까지 dp[i] = max(dp[i]+A[i], A[i])를 반복하여 DP테이블을 모두 채웁니다. 그 다음 max(dp)가 최대구간합의 해가 됩니다.
아래는 순서대로 제가 짠 최대구간합 풀이 코드입니다.(1,2,3,4번 순서대로)

1.전체다 구해서 최댓값 구하는 방법 O(n^3)
n = int(input())
A = list(map(int,input().split()))
max_ = A[0]
for i in range(n):
    for j in range(i,n):
        sumij = 0
        for k in range(i,j+1):
            sumij += A[k]
        if max_ < sumij:
            max_ = sumij
print(max_)

2.prefixsum 구해서 이용하는 방법 O(n^2)
n = int(input())
A = list(map(int,input().split()))
max_ = A[0]
P = [0 for _ in range(n)] #prefixsum들이 담겨있는 리스트
P[0] = A[0]
for i in range(1,n):
    P[i] = P[i-1] + A[i]
for i in range(n):
    for j in range(i,n):
        if i == 0:
            sumij = P[j]
        else:
            sumij = P[j] - P[i-1]
        if max_ < sumij:
            max_ = sumij
print(max_)

3.분할정복법 이용하는 방법 O(nlog2n)
import sys
MIN_INT = -sys.maxsize
n = int(input())
A = list(map(int,input().split()))
def max_interval(A,l,r):
    if l >= r: return A[l]
    m = (l+r)//2
    L = max_interval(A,l,m)
    R = max_interval(A,m+1,r)
    #M 계산
    max_l, max_r = MIN_INT, MIN_INT
    for i in range(l,m+1):
        m_l = 0
        for j in range(i,m+1):
            m_l += A[j]
        if m_l > max_l:
            max_l = m_l
    for i in range(m+1,r+1):
        m_r = 0
        for j in range(m+1,i+1):
            m_r += A[j]
        if m_r > max_r:
            max_r = m_r
    M = max_l + max_r
    return max(L,M,R)
print(max_interval(A,0,n-1))

4.동적 계획법 이용하는 방법 O(n)
n = int(input())
A = list(map(int,input().split()))
def max_interval(A):
    dp = [0 for _ in range(n)] #dp[i]는 A[i]로 끝나는 최대구간합
    # A[k]로 끝나는 최대구간합 = dp[k-1] + A[k] or A[k]
    dp[0] = A[0]
    for i in range(1,n):
        dp[i] = max(dp[i-1]+A[i], A[i])
    return max(dp)
print(max_interval(A))

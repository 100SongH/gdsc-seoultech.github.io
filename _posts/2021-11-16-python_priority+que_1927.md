---
layout: post
title: �˰��� ���͵� �켱���� ť-1927��
date: 2021-10-13 12:00:00 +0900
author: Hanyoonjae
description: �켱���� ť �˰��� 1927�� Ǯ��
categories: ["study"]
---

# �켱���� ť 1927�� - �ּ� ��

�ȳ��ϼ���. GDSC �ȵ���̵� ��� �������Դϴ�.<br>
�켱���� ť 1927�� ���� Ǯ���մϴ�! <br>

[https://www.acmicpc.net/problem/1927](https://www.acmicpc.net/problem/1927)

# Ǯ��

���� ��������Ʈ���� ������ �ڷᱸ����, **�ִ񰪰� �ּڰ��� ã�� ����**�� ������ �ϱ� ���� ��ȵǾ����ϴ�.<br>
<br>
���̽㿡�� ���� ���� heapq ����� �־� heap�� ���� ����� �� �ֽ��ϴ�. �̶� �ּ� �� ���·� ���ĵ˴ϴ�.

```python
import sys
import heapq

n = int(sys.stdin.readline())

command = [0]*n

for i in range(n) :
    command[i] = int(sys.stdin.readline())

heap = []
heapq.heapify(heap)


for i in command :
    if i == 0 :
        if len(heap) == 0 :
            print(0)
        else :
            print(heapq.heappop(heap))
    else :
        heapq.heappush(heap, i)
```

## ������

Ȯ���� ���̽㿡�� ���� ����� ���� �ڷᱸ���� �����ϱ� ���մϴ�.

[���� �ڷ�](https://littlefoxdiary.tistory.com/3)


---
title: "Approximation Algorithm"
excerpt: "근사 알고리즘"

categories:
  - 과제
tags:
  - 알고리즘
  - java

---


# Approximation Algorithm

# 컴퓨터 알고리즘 5번째 과제

## 제출자 : 오성환

## 1. 근사 알고리즘(Approximation Algorithm)이란?

**근사 알고리즘**(approximation algorithm)은 어떤 최적화 문제에 대한 해의 근사값을 구하는 알고리즘이다. 가장 최적화되는 답을 구할 수는 없지만, 비교적 빠른 시간에 계산이 가능하며 어느 정도 보장된 근사해를 계산할 수 있다.

근사 알고리즘은 근사해가 얼마나 최적해에 근사한지를 나타내는 **근사비율**을 가진다. 근사 비율이란 근사해의 값과 최적해의 값의 비율로서, 1.0에 가까울수록 정확도가 높은 알고리즘임을 뜻한다.

하지만 실질적으로 문제의 최적해를 알 수 없으므로, 최적해를 대신할 수 있는 '간접적인' 최적해를 찾아 이를 최적해로 삼아 근사 비율을 계산한다.

따라서 이번엔 근사 알고리즘을 나타내어 시간복잡도와 근사비율을 구할 것이다.

## 2. 근사 알고리즘(Approximation Algorithm) 문제의 종류

근사 알고리즘으로 해결하는 문제에는 아래와 같은 문제들이 있다.

- 여행자 문제 (Traveling Salesman Problem, TSP)
- 정점 커버 문제 (Vertex Cover Problem)
- 통 채우기 문제(Bin Packing Problem)
- 작업 스케줄링 문제 (Job Scheduling Problem)
- 클러스터링 문제 (Clustering Problem)

여기서 우리는 **작업 스케줄링 문제**(Job Scheduling Problem)을 다룰 것이다.

## 3. 작업 스케줄링(Job scheduling) 문제란?

작업 스케줄링 문제란 n개의 작업과 각 작업의 수행 시간 t_i가 주어지고, m개의 동일한 기계가 주어졌을 때 모든 작업이 가장 빨리 종료되도록 작업을 기계에 배정하는 문제이다. (여기에서 한 작업은 배정된 기계에서 연속적으로 수행되어야 하고, 기계는 1번에 하나의 작업만을 수행한다)

여기에서 우리는 그리디 방법으로 작업을 배정한다. 현재까지 배정된 작업에 대해서 가장 빨리 끝나는 기계에 새로운 작업을 배정하는 것이다.

여기서 추가로 이야기하자면, 나는 작업 스케줄링 문제를 이미 그리디 알고리즘(Greedy Algorithm)을 배웠을 때 만들어 보았다. 당시 작업 스케줄링은 4개지로 나타낼 수 있었다.

- 빠른 시작시간 작업을 우선(Earliest start time first) 배정
- 빠른 종료시간 작업을 우선(Earliest finish time first) 배정
- 짧은 작업 우선(Shortest job first) 배정
- 긴 작업을 우선(Longest job first) 배정

당시 작업 스케줄링 문제를 다뤘을 때는 빠른 시작시간 작업을 우선(Earliest start time first)배정하여 알고리즘을 구성하였다. 여기서, 4가지 알고리즘들 중 첫 번째 알고리즘만이 항상 최적해를 찾을 수 있다고 하였다.

그러므로 이번 근사 알고리즘을 사용하는 문제에서는 이번과 같이 **짧은 작업 우선**(Shortest job first)배정을 사용해야 하는 것이다. 또한 이 기회로 왜 최적해를 찾지 못하는지 알 수 있는 계기가 될 수 있을 것이다.

## 4. 설계 과정, 수도 코드

작업 스케줄링 문제의 수도 코드는 다음과 같다.

입력: n개의 작업 / 각 작업 수행 시간 ti, i = 1, 2, ⋯, n / 기계 Mj, j = 1,2, ⋯, m
출력: 모든 작업이 종료된 시간
1. for j = 1 to m
2. L[j] = 0 // L[j]=기계 Mj
에 배정된 마지막 작업의 종료 시간
3. for i = 1 to n {
4. min = 1
5. for j = 2 to m { // 가장 일찍 끝나는 기계를 찾는다.
6. if (L[j] < L[min])
7. min = j
}
8. 작업 i를 기계 Mmin에 배정한다.
9. L[min] = L[min] + ti
}
10. return 가장 늦은 작업 종료 시간

## 5. 자바 코드

```java
import java.util.Scanner;

public class JobScheduling {
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        System.out.print("기계의 수 : ");
        int m = scanner.nextInt();
        System.out.print("작업의 수 : ");
        int n = scanner.nextInt();

        int[] L = new int[m];
        int[] t = new int[n];

        System.out.print("각 작업 수행 시간 (띄어쓰기해 입력) : ");
        for (int i=0; i<n; i++) t[i] = scanner.nextInt();

        for (int j=0; j<m; j++) L[j]=0;

        int min;

        for (int i=0; i<n; i++) {
            min = 0;
            for (int j=1; j<m; j++) {
                if(L[j] < L[min])
                    min = j;
            }
            L[min] = L[min] + t[i];
        }

        int last = 0;

        for (int j=1; j<m; j++) {
            if(L[j] > L[last])
                last = j;
        }

        System.out.println(" ");
        System.out.print("가장 늦은 작업 종료 시간 : " + L[last]);

    }
}

```

## 6. 코드 결과

![1](https://user-images.githubusercontent.com/80510972/118953638-fa91ee00-b997-11eb-91a0-46554bcd1688.png)

다음과 10개의 작업(1, 4, 2, 9, 5, 3, 7, 10, 6, 8)을 4개의 기계에 그리디 방법으로 배정하였을 때, 가장 늦은 작업 종료 시간은 17로 나타난다.

![2](https://user-images.githubusercontent.com/80510972/118954898-2b265780-b999-11eb-89ea-5622fed10744.png)

## 7. 시간 복잡도 분석

위 작업 스케줄링 알고리즘에서는 n개의 작업을 하나씩 가장 빨리 끝나는 기계에 배정한다. 따라서 n개의 작업을 배정하고, 마지막에 배열 L을 탐색해 작업 종료 시간을 찾아야 하므로 n x O(m) + O(m) = O(nm)이 소요된다.

## 8. 근사 비율

그렇다면 근사 비율을 한번 구해보자.

결론부터 말하자면, 작업 스케줄링 알고리즘의 근사해를 OPT'라 하고 최적해를 OPT라고 했을 때, OPT' <= 2OPT 이다. 즉, 근사해는 최적해의 2배를 넘지 않는다.

![3](https://user-images.githubusercontent.com/80510972/118958669-97568a80-b99c-11eb-8b9b-8497771c40f0.png)

마지막으로 배정된 작업이 T부터 수행되어 t_i만큼 수행되므로 작업 종료 시간 OPT'는 T+t_i로 나타낼 수 있다.
또한 작업 i를 제외한 모든 작업의 수행 시간 합을 기계의 수 m으로 나누었을 때 나오는 T'를 평균 종료 시간으로 나타낼 수 있다.

그렇다면 작업 i가 배정된 기계를 제외한 모든 기계에 배정된 작업이 종료되는 시간 T'는 적어도 T와 같게 또는 더 늦게 종료되게 된다. (T <= T')

이를 통해 OPT' <= 2OPT 를 증명할 수 있다.

![4](https://user-images.githubusercontent.com/80510972/118959407-43987100-b99d-11eb-8bcd-7729a72b26c3.png)

식 1번은 단순히 작업 i의 시간을 양변에 더해준 것이고, 식 2번은 최적해 OPT는 모든 작업의 수행 시간의 합을 기계의 수로 나눈 값 (평균 종료 시간)보다 같거나 크고 또한 하나의 작업 수행 시간과 같거나 크다는 것을 부등식에 반영한 것이다.
그리고 마지막 (2-1/m)OPT에서 m이 커질수록 2OPT에 가까워지기 때문에 2OPT보다 작거나 같다고 이야기할 수 있다.

* n=[2, 3, 4, 5]
* m=2

인 경우로 직접 최적해와 근사해를 알아보면,

최적해는 2+5, 3+4로 이루어진 7이지만, 작업 스케줄링 알고리즘을 적용하면 2+4, 3+5로 8의 근사해가 나타난다. 이 또한 OPT' <= 2OPT가 적용되는 것을 알 수 있다.

* n=[1, 4, 2, 9, 5, 3, 7, 10, 6, 8]
* m=4

위에 직접 나타냈던 소스코드에서의 최소한의 최적해를 알아보면

1+3+10, 2+4+8, 5+9, 6+7으로 최소 14로 나타낼 수 있지만, 작업 스케줄링 알고리즘을 사용한 근사해가 17로 나타난 것을 알 수 있다. 이 또한 OPT' <= 2OPT가 성립한다.

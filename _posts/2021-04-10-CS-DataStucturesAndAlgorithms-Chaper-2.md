---
layout: post
title: "Data Structures & Algorithms Chapter 2: Complexity"
date: 2021-04-10
category: read 
excerpt: ""


---

# [CS-Data Structures & Algorithms] Chapter 2: Complexity

- 알고리즘에게 확장성이란?

    (여기서 Input Size란 데이터 입력 크기를 뜻한다.)

    Input Size가 증가함에 따라 실행시간 및 메모리 사용량 측면에서 알고리즘이 수행하는 방식을 나타낸다.

- Big O notation(빅오 표기법)
    - 위 표기법은 인수가 특정 값이나 무한으로 향하는 경우, 함수의 동작을 제한하도록 설정하는 수학적 표기법
    - 알고리즘의 빅오 표기법은 두 가지(실행시간과 메모리 사용량) 차원에서 다양한 수준의 확장성을 표시함

        (프로그래머는 다양한 시간복잡도를 간결하게 표현하기 위해 빅오 표기법을 사용한다.)

    - Other notations(빅오 표기법 외의 표기법)
        1. Big Omega(빅오메가): 실행시간에 대한 최선의 케이스의 측정에 사용하는 표기법
        2. Big Theta(빅세타): 실행시간에 대해 최선/최악 케이스가 동일할 때 측정에 사용하는 표기법

- Time complexity(시간 복잡도)
    - 시간 복잡도란 Input size가 증가할 때, 알고리즘을 실행하는데 필요한 시간을 표현한 수치이다.
    - 시간 복잡도의 종류

        이 책에서 나오는 시간 복잡도이고, 사실 이 외에 다른 시간복잡도를 마주칠 일은 많지 않다.

        1. Contant Time(상수 시간): Input size에 관계없이 동일한 실행 시간을 갖는 알고리즘(빅오 표기법: O(1))

            ```swift
            func printFirst(nums: [Int]) {
            	if let first = nums.first {
            		print(first)
            	} else {
            		print("no numbers")
            	}
            }
            ```

        2. Linear Time(선형 시간): Input Size와 정비례해서 증가하는 시간 복잡도(빅오 표기법: O(n))

            ```swift
            func printNums(nums: [Int]) {
                for num in nums {
                    print(num)
                }
            }
            ```

        3. Quadratic Time(2차 시간): Input size의 제곱에 비례해서 시간이 증가하는 시간 복잡도(빅오 표기법: O(n2))

            ```swift
            func printNums(nums: [Int]) {
                for _ in nums {
                    for num in nums {
                        print(num)
                    }
                }
            }
            ```

        4. Logarithmic Time(로그 시간): 절반을 반복적으로 삭제하는 알고리즘은 로그 시간 복잡도를 가진다.(빅오 표기법: O(log n))

            ```swift
            // Binary Search(이진탐색)
            func naiveContains(_ value: Int, in array: [Int]) -> Bool {
                guard !array.isEmpty else { return false }
                let middleIndex = array.count / 2
                
                if value <= array[middleIndex] {
                    for index in 0...middleIndex {
                        if array[index] == value {
                            return true
                        }
                    }
                } else {
                    for index in middleIndex..<array.count {
                        if array[index] == value {
                            return true
                        }
                    }
                }
                
                return false
            }
            ```

        5. Quasilinear time(선형 로그 시간): 2차 시간보다 빠르고, 선형 시간보다 느린 시간 복잡도(빅오 표기법(빅오 표기법: O(n log n))

            ```swift
            // 대표적인 예시: 비교정렬(고속 푸리에 변환)
            ```

- Space complexity(공간 복잡도)
    - 공간 복잡도란 알고리즘을 실행하는데 필요한 자원(메모리)을 수치로 나타낸 것이다.
    - 예시 1

        ```swift
        func printSorted(_ array: [Int]) {
          let sorted = array.sorted()
          for element in sorted {
            print(element)
          }
        }
        ```

    - 예시 2

        ```swift
        func printSorted(_ array: [Int]) {
          // 1
          guard !array.isEmpty else { return }
        // 2
          var currentCount = 0
          var minValue = Int.min
        // 3
          for value in array {
            if value == minValue {
              print(value)
              currentCount += 1
            }
          }
          while currentCount < array.count {
        // 4
            var currentValue = array.max()!
            for value in array {
              if value < currentValue && value > minValue {
                currentValue = value
              }
        }
        // 5
            var printCount = 0
            for value in array {
              if value == currentValue {
                print(value)
                currentCount += 1
              }
        }
        // 6
            minValue = currentValue
          }
        }
        ```

        위 두 예시는 array를 정렬해서 출력하는 함수인데,

        우선 '예시 1'을 보면 함수 내부에서 array.sorted()가 실행되는데, 이 메서드가 실행되면

        메모리에 array와 같은 크기의 공간을 할당한다.(빅오 표기법: O(n))

        하지만, '예시 2'에서는 단순히 함수 내부의 몇가지 변수에 대한 공간만 할당하면 된다.

        (언제나 일정하다. / 빅오 표기법: O(1))

- Key points of the chapter
    - Time complexity
    - Constant time, Logarithmic time, Linear time, Quasi-Linear time, Quadratic time
    - Space complexity
    - Big O notation
    - Time and space complexity are high-level measure of scalability, They don't measure the actual speed of the algorithm itself

        (시간 복잡도와 공간 복잡도는 확장성에 대한 하이레벨 측정법임으로, 실제 알고리즘의 속도를 측정하지는 않습니다.)

    - For small data sets, time complexity is usually irrelevant. A quasilinear algorithm can be slower than a linear algorithm.

        (작은양의 데이터는 시간 복잡도와 상관없습니다. 선형 로그 알고리즘이 선형 알고리즘보다 느릴 수도 있습니다.)

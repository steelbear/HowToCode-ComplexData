# 9 Abstraction

## Parameterize

```Racket
;; ListOfString -> Boolean
;; produce true if los includes "UBC"

(define (contains-ubc? los)
  (cond [(empty? los) false]
        [else
         (if (string=? (first los) "UBC")
             true
             (contains-ubc? (rest los)))]))

;; ListOfString -> Boolean
;; produce true if los includes "McGill"

(define (contains-mcgill? los)
  (cond [(empty? los) false]
        [else
         (if (string=? (first los) "McGill")
             true
             (contains-mcgill? (rest los)))]))
```

두 함수를 비교해보면, 들어오는 데이터의 타입, 나가는 데이터의 타입, 함수 내의 대략적인 로직까지 비슷하다는 것을 알 수 있다. 유일하게 다르다면, 리스트 내 데이터들과 비교될 대상 정도만 다르다. 이렇게 입력과 출력, 내부 로직들이 같다면, 그 둘을 합칠 수 있다.

```Racket
;; String ListOLfString -> Boolean
ll produce true if los includes sample

(define (contains? sample los)
  (cond [(empty? los) false]
        [else
         (if (string=? (first los) sample)
             true
             (contains? sample (rest los)))]))
```

비슷한 부분은 하나로 묶고 다른 부분만 매개변수로 밭으면 같은 로직을 따르는 모든 함수들을 바로 만들 수 있다. 만약 매개변수를 두개 쓰는게 싫고, 처음에 나온 코드처럼 따로 함수로 쓰고싶다면...

```Racket
(define (contains-ubc? los) (contains? "UBC" los))
(define (contains-mcgill? los) (contains? "McGill" los))
```

이렇게 짧게 원하는 함수를 선언할 수 있다.

## Built-In Abstract Functions

어떤 언어든 코드를 짜다보면 비슷한 로직을 계속 사용하는 우리의 모습을 볼 수 있다. 그래서 항상 사용되는 로직들을 각 소스코드마다 선언하느라 손가락을 혹사시키지 말라고 미리 만들어져있는 abstract functions이 있다.

* (build-list n f) : Natural (Natural -> X) -> (listof X)
  * 0에서 n까지 f에 넣은 결과물들을 리스트로 생성
* (filter p lox)   : (X -> boolean) (listof X) -> (listof X)
  * lox에서 p가 true 나오는 놈만 남기기
* (map f lox)      : (X -> Y) (listof X) -> (listof Y)
  * lox에 있는 모든 데이터들에 f 적용시키기
* (andmap p lox)   : (X -> boolean) (listof X) -> boolean
  * (fold and true (map p lox))
* (ormap p lox)    : (X -> boolean) (listof X) -> boolean
  * (fold or false (map p lox))
* (foldr f base lox)   : (X Y -> Y) Y (listof X) -> Y
  * (f x1 (f x2 (f x3 (f x4 base)))) 대로 f 적용
* (foldl f base)   : (X Y -> Y) Y (listof X) -> Y
  * (f (f (f (f base x1) x2) x3) x4) 대로 f 적용

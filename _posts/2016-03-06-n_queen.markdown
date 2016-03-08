---
layout: post
title:  "N queen 문제 풀이"
comments: True
---

## 들어가기에 앞서  
설명에 앞서 이 소스는 많은 부분 [bitwise solution](http://gregtrowbridge.com/a-bitwise-solution-to-the-n-queens-problem-in-javascript/)의 소스를 이용하였습니다. 퍼포먼스의 큰 포인트 이 소스에 있다고 보시면 될 것 같네요.
실제 이분도 [Backtracking Algorithms in MCPL](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.51.7113&rep=rep1&type=pdf)에서 아이디어를 차용했다고 하네요. 
해당 pdf에는 n queen 문제 말고도 다양한 알고리즘에 대한 이야기가 나오니 알고리즘을 공부하시는 분들은 한 번 보시는 것도 괜찮을 것 같다는 생각이 드네요.  

## 핵심 로직  

### Backtracking
n queen 문제는 Backtracking이라는 방법의 해법이네요. 이것이 왜 Backtracking인가에 대해서는 저도 모르겠습니다만, 문제의 풀이는 단순합니다.   

* 주어진 N에 대해 첫째열 부터 마지막 N열까지 퀸을 위치시켜 올바른 정답을 찾는다.  
* 순차적으로 첫째행 부터 마지막 행까지 퀸을 위치시키면서 진행한다.
* 퀸끼리 충돌하는 경우가 발견되면 더 이상 진행하지 않고 다른 경우를 찾는다.

어떤가요? 로직 자체는 참 쉽죠? 실제 로직을 한번 보겠습니다. 이 코드는 실제 최종 작성된 코드는 아니지만, 이 후에 비트연산을 이용한 해법을 보기 전에 설명드립니다. 이미 Backtracking에 대한 이해가 있다면, 아래의 bit연산 해법 핵심을 보면 되겠습니다.

    /**
     * Created by kim-dong-o on 16. 3. 3.
     */
    public class Main {
        /**
         * 결과 카운트
         */
        private static int count = 0;

        /**
         * 충돌 체크
         *
         * @param location 퀸 위치정보
         * @param row
         * @return
         */
        public static boolean isConsistent(int[] location, int row) {
            for (int i = 0; i < row; i++) {
                if (location[i] == location[row]) return false;   //동일 열 체크
                if ((location[i] - location[row]) == (row - i)) return false;   // 탑->좌측 아래 방향 체크
                if ((location[row] - location[i]) == (row - i)) return false;   // 탑->우측 아래 방향 체크
            }
            return true;
        }

        /**
         * 퀸 출력
         *
         * @param location 퀸 위치 정보
         */
        public static void printQueens(int[] location) {
            int N = location.length;
            for (int index1 = 0; index1 < N; index1++) {
                for (int index2 = 0; index2 < N; index2++) {
                    if (location[index1] == index2) System.out.print("Q ");
                    else System.out.print("* ");
                }
                System.out.println();
            }
            System.out.println();
        }


        /**
         * 찾기
         *
         * @param N 주어진 N
         */
        public static void enumerate(int N) {
            //퀸을 저장할 공간 생성
            int[] location = new int[N];
            //계산을 수행
            enumerate(location, 0);
        }

        /**
         * 찾기
         *
         * @param location 위치정보
         * @param row      현재 row
         */
        public static void enumerate(int[] location, int row) {
            //목표 N
            int N = location.length;
            //마지막까지 왔다면
            if (row == N) {
    //            printQueens(q);// 실제 결과를 보려면 이 주석을 해제 하면 됩니다.
                //카운트 증가
                count++;
            } else {
                //열을 순환하며, 올바른 값을 찾습니다.
                for (int index = 0; index < N; index++) {
                    //위치 할당
                    location[row] = index;
                    //만일 충돌이 없다면 다음 열에 할당을 시작한다.
                    if (isConsistent(location, row)) enumerate(location, row + 1);
                }
            }
        }


        public static void main(String[] args) {
            printResult();
        }

        /**
         * 결과를 출력
         */
        public static void printResult() {
            StringBuilder s = new StringBuilder();
            for (int i = 1; i < 14; i++) {
                s.append("n = ");
                s.append(String.format("%02d", Integer.parseInt(String.valueOf(i))));
                s.append(", solution count is ");
                s.append(execute(i));// 실제 계산
                s.append(".\n");
            }
            System.out.print(s.toString());
        }

        /**
         * 실행
         *
         * @param N 조건 N
         * @return
         */
        private static int execute(int N) {
            //초기 카운트
            count = 0;
            //주어진 N에 대한 검색
            enumerate(N);
            return count;
        }
    }


달아둔 주석을 읽어 보시면 앞서 설명했던 로직과 크게 차이가 없다는 것을 아실 겁니다. 재미있는 부분이 있다면 재귀가 중간에 사용되었다는 점입니다. 실제로 알고리즘 문제에는 재귀로 푸는 방법이 많은 것 같네요. 이 부분은 핵심이 아니기에 흐름만을 이해하고 바로 다음으로 넘어 가기로 하겠습니다.

### 비트연산을 이용한 해법  
기본적으로 비트연산을 위한 해법 역시 큰 흐름은 Backtracking과 크게 차이가 없습니다. 그럼에도 비트연산쪽이 훨씬 빠른 이유는 2가지 있습니다.  
**첫째, 상대적으로 가볍고 컴퓨터가 이해하기 쉬운 비트연산을 사용한다.**
**둘째, 퀸끼리 충돌을 검증하는 부분에서 Backtracking은 루프 연산을 하면서 낭비가 발생하지만, 비트 연산의 경우 이부분이 훨씐 더 적은 연산을 한다.**  

우리는 행방향, 열방향, 위에서 우측 대각 방향, 위에서 좌측 대각 방향에 대해 충돌이 발생하는 지에 대해 검증을 해야 합니다. Backtracking에서는 루프를 순환하며, 퀸의 위치정보를 기반으로 검증을 해야 했습니다. 
이 루프문은 메서드로 나누어져 있기는 하지만, 실제 재귀 루프안에서 발생하는 루프입니다. 적어도 N제곱 이상의 비용을 가질 겁니다. 이러한 루프는 당연히 비싼 비용을 가집니다.

    for (int i = 0; i < row; i++) {
        if (location[i] == location[row]) return false;   //동일 열 체크
        if ((location[i] - location[row]) == (row - i)) return false;   // 탑->좌측 아래 방향 체크
        if ((location[row] - location[i]) == (row - i)) return false;   // 탑->우측 아래 방향 체크
    }

비트 연산은 이 부분에서 차이점을 가집니다. 앞서 4가지 방향에서 체크한다고 했습니다. 행방향에 대해서는 Backtracking이나 비트연산이나 한 행에 퀸을 배치 후 다음 행에 배치하기에 걱정을 할 필요가 없습니다.  
자 그럼 여기서 한가지 상상을 해볼까요? 만일 내가 퀸이 있는 체스판을 정면에서 바라본다고 생각해 봅시다. 4개의 위치가 있을 것이고, 만약에 하나도 비어 있지 않다면, 더 이상 충돌을 체크 하지 않아도 될 것입니다. 
그럼 위치를 바꿔서 체스판의 좌측 대각면에서 바라본다고 생각해 봅시다. 비어있는 위치가 있는지를 체크하면 될 것입니다. 우측 대각면도 마찬가지 겠지요. 자 그럼 여기서 간단히 손코딩뇌컴파일눈디버깅(?) 해보도록 합시다.

**4/4 체스판 시뮬레이트**   

* 먼저 비어있는 체스판이 존재합니다.
* 체스가 비어 있는 것을 확인 합니다.
* 최 우측에 한개의 퀸을 떨어 트립니다.(당연한 얘기지만 실제 프로그램에서는 순환하게 되겠지요?)   
  이것을 문자로 표현하면, 0001이 되겠습니다.
* 이제 2행에 배치합니다. 두번째 퀸은 최좌측 2번째에 배치 시키겠습니다. 그러면 0101 이 됩니다. 여기서 체스판을 볼까요? 실제 체스판은 아래와 같은 모양이 될겁니다.  
 0001  
 0100
   
 자 이제 3번째 행에 퀸을 배치해야 합니다만, 배치 가능한 포인트가 있을까요? 2열 4을은 정면 방향에서 막혀 있습니다. 놓아야 하는 것은 1열과 3열이 가능합니다. 
 1열에 배치한다면 상단에서 좌측으로 대각선 방향에서 충돌이 발생합니다. 만약 3열에 배치하게된다면, 상단에서 우측 방향의 대각선에 충돌이 발생하게 되지요. 
 즉 현재는 더 이상 놓을 수가 없는 포인트가 없습니다. 그러므로 다른 순환을 시작해야 겠지요.  
    
위의 시뮬을 전체에 대해 순환하면 올바른 답을 구할 수 있을 겁니다. 여기서 포인트는 퀸의 충돌 검증에 있습니다. 비트연산 해법이서는 이런식으로 구현합니다. 
  먼저 정면 방향 0101 로 퀸이 위치합니다. 상단에서 우측 대각선은 0010입니다. 3번째 행의 배치만을 따지면 되기에 1열 최하단에서 대각방향으로 따지면 됩니다. 
  또한 마지막은 3행의 4열 부터 대각방향으로 올라오면서 따지면 됩니다. 3열의 배치만을 따지기 때문에 3행 대상으로 4개열만 따지면 됩니다. 자연스레 N만큼의 갯수만 나옵니다. 
  우측 대각을 하셨다면 좌측 대각은 쉽습니다. 같은 이치로 1100이 나오게 되어 있습니다.자 이제 각 3개의 방향을 합칩니다. 0010, 0101, 1100 전부 합치면 1111이 나옵니다. 
  이것은 더 이상 퀸을 둘 자리가 없다는 뜻이지요. 실제 그림을 보시고 따지면 좀 더 이해가 쉬울것 같습니다.(**45도 각도 틀어지는 것과 그것이 한칸 씩 쉬프트 연산하는 것과 동일하다는 것이 포인트!!**)   

인간은 퀸의 위치를 보고 따지는 것이나, 체스판의 방향을 보고 따지나 따지는 속도에서 별반 차이가 나지 않습니다.(흠... 아마도 사람이 었다면 위치를 보고 따지는 게 훨씬 쉬어보이기도 합니다.) 
하지만 컴퓨터의 경우에는 다릅니다. 실제 해당 파트의 구현 부분을 보겠습니다.  

    leftDirection = 1100;
    righitDirection = 0010;
    forwardDirection =  0101;
    added = leftDirection | righitDirection | forwardDirection;// 1111

이해를 돕기 위해 위와 같이 표현 했습니다만, 실제 코드는 이와는 다릅니다. 하지만 그 요지는 일맥상통 하지요.**포인트는 논리합을 이용해 간단히 합산해 버린 다는 점입니다.**. 결과적으로는 이 충돌체크가 퍼포먼스 포인트가 되겠네요.
한가지 더 포인트가 있다면, 가능한 배치를 뽑을 때에도 직전에 놓은 퀸에 위치에 따라 이미 불가능한 포인틑 배제해버리는 특징이 있습니다. 예제 코드를 보자면
  
    leftDirection = 0000;
    righitDirection = 0010;
    forwardDirection =  0001;
    added = leftDirection | righitDirection | forwardDirection;// 0011 
  
자연스레 이전에 놓았던 퀸에 배치에 따라 불가능한 포인트는 아예 배제하고 시작해 버립니다. 
  
이제 부터 실질적인 소스 분석 전에 비트 연산에 대해 간단히 알아보도록 하겠습니다.  
이미 잘 알고 계신다면 소스 분석으로 넘어가면 되고, 가물가물 하시면 간단히 보고 넘어가면 되겠습니다.  
  
## 비트연산 기본  
| 연산자  | 사용법 | 설명         |
|:------------|:------------:|------------:|
|비트 AND	  |  a & b	| 두 피연산자의 대응되는 비트가 모두 1이면 1을 반환.                          |
|비트 OR	      |  a \| b	| 두 피연산자의 대응되는 비트에서 둘 중 하나가 1이거나 모두 1인 경우 1을 반환.          |
|비트 XOR	  |  a ^ b	| 두 피연산자의 대응되는 비트에서 둘 중 하나가 1이고, 둘 다 1이 아닐 경우 1을 반환.      |
|비트 NOT	  |  ~ a	| 피연산자의 비트를 뒤집음.                                          |
|왼쪽으로 이동 |	a << b	|a의 2진수 표현을 b 비트만큼 왼쪽으로 이동함. 오른쪽은 0으로 채움.                     |
|부호 비트로 채우는 오른쪽 이동	 |a >> b|	a의 2진수 표현을 b 비트만큼 오른쪽으로 이동함. 오른쪽 남는 비트는 버림.       |
|0으로 채우는 오른쪽 이동|	a >>> b|a의 2진수 표현을 b 비트만큼 오른쪽으로 이동함. 오른쪽 남는 비트는 버리고, 왼쪽은 0으로 채움.|

## 소스 분석(bit solution)  
이것은 실제 작성한 코드에 추가적으로 주석을 달아 보았습니다.또한 이해를 돕기 위해 추가적인 튜닝 포인트는 제거 하였습니다.  

        /**
         * N queen 문제 풀이
         * <p>
         * Created by kim-dong-o on 16. 3. 5.
         */
        public class Main {
            /**
             * 완료체크 변수
             */
            private static int done;
            /**
             * 카운트
             */
            private static int count;
            /**
             * N 절반 위치
             */
            private static int half;
            /**
             * 짝수 체크
             */
            private static boolean isPair;
        
            /**
             * 실행
             *
             * @param N 주어진 N
             * @return 정답
             */
            private static int execute(int N) {
                //초기화
                count = 0;
                //완료 체크 값 할당
                done = (int) Math.pow(2, N) - 1;
                //체크 수행. 처음 이기에 비어있는 체스판 0,0,0 으로 시작된다.논리적으로는 0000,0000,0000으로 생각하면 된다.
                check(0, 0, 0);
                //카운트 반환
                return count;
            }
        
            /**
             * 값 체크
             *
             * @param topToRight 우측 대각
             * @param forward    정면방향
             * @param topToLeft  좌측 대각
             */
            public static void check(int topToRight, int forward, int topToLeft) {
                //완료 체크 순환이 시작될 때 체크하여, 해당되면 바로 리턴한다.
                //퀸이 모드 들어왔다면 11---- 의 형태가 될 것이다.
                if (forward == done) {
                    count++;
                    return;
                }
        
                //충돌 체크. 실제 topToRight | topToLeft | forward 만으로도 충돌 체크가 되나, 이 후 연산을 위해 보수를 취한다.
                int poss = ~(topToRight | topToLeft | forward);
        
                //실제 충돌 체크 논리곱을 수행하여 비어 있는 자리가 있는 지를 체크 한다.
                while ((poss & done) != 0) {
                    //이 후 연산을 위한 비트 값을 추출 한다.
                    //0010, 0100 등과 같은 형태를 취하게 된다.
                    int bit = poss & -poss;
                    //위치 순한을 위한 증가. 또한 위치 추가
                    poss -= bit;
                    //정면은 단지 현재 비트를 논리합하면 구할 수 있다.
                    //좌우 대각 방향에 대해서는 1칸씩 쉬프트 연산으로 방향을 틀어버린다.
                    //현재 메서드에 진입한 이상 더 이상 첫째 행이 아니므로, isFirst는 항상 false를 할당
                    check((topToRight | bit) >> 1, forward | bit, (topToLeft | bit) << 1);
                }
            }
        
            /**
             * 메인
             *
             * @param args 아이고 의미없다.
             */
            public static void main(String[] args) {
                //스트링 빌더는 빠르다...
                StringBuilder s = new StringBuilder();
                for (int i = 1; i < 14; i++) {
                    s.append("n = ");
                    s.append(i < 10 ? "0" + i : i);
                    s.append(", solution count is ");
                    s.append(execute(i));
                    s.append(".\n");
                }
                System.out.print(s.toString());
            }
        
        }

앞서 설명한 부분에서 대략은 이해 하셨을 겁니다. 그래도 다시 한번 찬찬히 코드를 보죠. 중요한 점은 이 코드에서 정수는 그다지 의미가 없습니다. 다만 정수는 2진수의 표현일 뿐입니다. 
또한 2진수 자체 역시 크게 의미 있는 것은 아닙니다. 2진수 역시 체스판 위에 퀸을 형상화 하기 위한 표현일 뿐입니다. 
  
첫번 째, 들어가기 전에 로직이 끝날 종료값 설정(done)
  
    done = (int) Math.pow(2, N) - 1;
      
위에 코드는 주어지는 N에 따라 1,11,111,1111,... 계속적으로 설정됩니다. 이것은 퀸을 더 놓을 수 있는 자리가 있는지 없는 지에 대한 지표입니다.  

둘째, 순환하고 돌아 왔을 때, 카운트 체크. 여기시 체크가 된다면 더 이상 진행할 필요 없으니 리턴하여 다시 재귀 포인트로 돌아갑니다.   

    if (forward == done) {
        count++;
        return;
    }
    
매직넘버 poss, 프로그램 내에서 여러 의미로 쓰입니다만, 핵심은 충돌 체크 입니다. 좌우 쉬프트 연산 및 정면을 합산하여 이후 충돌 체크를 합니다. 
    
    int poss = ~(topToRight | topToLeft | forward);
    
충돌 체크. 변수 done 과 poss를 논리곱(&) 연산을 통해 충돌 체크 합니다. 충돌되면 더 이상의 진행이 없을 것이고, 충돌이 없다면 다음 행에 퀸을 배치하기 위해 다시 재귀될 것 입니다.        

    while ((poss & done) != 0) {
    
변수 bit는 퀸의 위치이기도 하면서, 다음 포지션을 잡기 위해서도 쓰여집니다. 
            
    int bit = poss & -poss;
    
다음 열로 넘어갈 준비를 합니다.    

    poss -= bit;

킌의 다음 열로 넘겨 배치 합니다. 쉬프트 연산을 통해, 좌우 대각방향의 정보도 넘겨 줌니다.

    check((topToRight | bit) >> 1, forward | bit, (topToLeft | bit) << 1);        
            
## 튜닝 포인트
이 정도면 충분히 괜찮은 속도가 나옵니다. 실제 서버에 올렸을 때, 104ms가 나왔습니다. 기본 backtracking 로직이 7-8백 대에서 왔다 갔다 하는 걸 보면, 정말 무시무시한 퍼포먼스 입니다. 
하지만 간단한 발상으로 좀 더 속도를 뽑을 수 있습니다. 간단한 이야기 입니다. 만약 어떠한 안전한 퀸의 위치가 있을 때, 체스판을 거꾸로 뒤집어 본다면 어떨까요. 아마도 마찬가지로 퀸들은 안전한 위치에 있을 겁니다.
아마 데칼코마니를 연상하면 좋을 것 같습니다. 한쪽면에 그림을 그리고 종이를 접게 되면 반대면에 똑같은 모양이 나오는 이치지요. 그렇다는 것은 모든 경우의 수를 일일히 다 셀 필요가 없다는 겁니다. 
만약 N 짝수면 정확히 절반만을 순환하고 끝냈후 카운트를 2로 곱하거나, 아니면 카운팅 할때 +2 로 올릴 수 있겠지요. 홀수 행일 때는 절반을 돌고 한번 더 순환 하면 됩니다. 그래서 전체 로직은 다음과 같습니다.
또한, 전체 소스와 관련 소스들은 [깃 허브](https://github.com/otwm/algorithm/blob/master/nqueen/src/Main.java)에 정리해서 올려 놓았으니 참조하면 되겠습니다.  

    /**
     * N queen 문제 풀이
     * <p>
     * Created by kim-dong-o on 16. 3. 5.
     */
    public class Main {
        /**
         * 완료체크 변수
         */
        private static int done;
        /**
         * 카운트
         */
        private static int count;
        /**
         * N 절반 위치
         */
        private static int half;
        /**
         * 짝수 체크
         */
        private static boolean isPair;
    
        /**
         * 실행
         *
         * @param N 주어진 N
         * @return 정답
         */
        private static int execute(int N) {
            //초기화
            count = 0;
            //절반 값 할당
            half = (int) Math.pow(2, N / 2);
            //짝수 체크
            isPair = N % 2 == 0;
            //완료 체크 값 할당
            done = (int) Math.pow(2, N) - 1;
            //체크 수행. 처음 이기에 비어있는 체스판 0,0,0 으로 시작된다.논리적으로는 0000,0000,0000으로 생각하면 된다.
            check(0, 0, 0, true);
            //카운트 반환
            return count;
        }
    
        /**
         * 값 체크
         *
         * @param topToRight 우측 대각
         * @param forward    정면방향
         * @param topToLeft  좌측 대각
         * @param isFirst    첫행인지 체크
         */
        public static void check(int topToRight, int forward, int topToLeft, boolean isFirst) {
            //완료 체크 순환이 시작될 때 체크하여, 해당되면 바로 리턴한다.
            //퀸이 모드 들어왔다면 11---- 의 형태가 될 것이다.
            if (forward == done) {
                count++;
                return;
            }
    
            //충돌 체크. 실제 topToRight | topToLeft | forward 만으로도 충돌 체크가 되나, 이 후 연산을 위해 보수를 취한다.
            int poss = ~(topToRight | topToLeft | forward);
    
            //실제 충돌 체크 논리곱을 수행하여 비어 있는 자리가 있는 지를 체크 한다.
            while ((poss & done) != 0) {
                //이 후 연산을 위한 비트 값을 추출 한다.
                //0010, 0100 등과 같은 형태를 취하게 된다.
                int bit = poss & -poss;
                //좌우 대칭에 대한 연산을 줄이기 위한 체크. 절반까지 도달하면 카운트를 두배로 증가시킨다.
                //데칼코마니를 생각한다면 쉬울 것 이다.
                if (isFirst && half == bit) {
                    //카운트 * 2
                    count *= 2;
                    //짝수 행은 더 이상 진행 할 필요가 없다.
                    if (isPair) break;
                }
                //홀수를 위한 체크. 절반 이 후 홀수 행이 계산되면 더 이상 연산하지 않아도 된다.
                if (isFirst && half < bit) {
                    break;
                }
                //위치 순한을 위한 증가.
                poss -= bit;
                //정면은 단지 현재 비트를 논리합하면 구할 수 있다.
                //좌우 대각 방향에 대해서는 1칸씩 쉬프트 연산으로 방향을 틀어버린다.
                //현재 메서드에 진입한 이상 더 이상 첫째 행이 아니므로, isFirst는 항상 false를 할당
                check((topToRight | bit) >> 1, forward | bit, (topToLeft | bit) << 1, false);
            }
        }
    
        /**
         * 메인
         *
         * @param args 아이고 의미없다.
         */
        public static void main(String[] args) {
            //스트링 빌더는 빠르다...
            StringBuilder s = new StringBuilder();
            for (int i = 1; i < 14; i++) {
                s.append("n = ");
                s.append(i < 10 ? "0" + i : i);
                s.append(", solution count is ");
                s.append(execute(i));
                s.append(".\n");
            }
            System.out.print(s.toString());
        }
    }
  
## 쥐어짜기
알고리즘에서는 실제 업무와는 달리 좀 더 가볍고 원시적인 자료형이 속도면에서 유리합니다. 또한 루프에서 빼낼 수 있는 녀석들은 철저히 빼내는 게 좋겠지요. 실제 동일한 backtraking 소스도 7-8 대에 속도를 왔다 갔다 
합니다. 큰 로직은 같으나, 사소한 부분에 차이가 나겠지요.   
  
쓰다 보니 꽤 길어졌네요. 설명은 여기 까지 할 까 합니다. 실제 알고리즘이 실무에서 쓰일 일은 거의 없으나, 사고를 키워준다는 면에서는 긍정적인 것 같습니다.  
긴 글 읽느라 수고 많으셨네요. 

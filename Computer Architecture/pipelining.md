## Pipelining

- 파이프라이닝은 여러 명령어가 중첩되어 실행되는 구현 기술이다. 어떤 명령이 일부 리소스를 사용하고 다른 리소스를 사용하지 않을 때, 사용되지 않는 리소스를 다른 명령이 사용하도록 한다. 

- 명령이 처리되는 사이클을 여러 단계로 나눈다. 

- 일 하나를 하는 시간 자체를 줄이는 것은 아니다. 하지만 일이 병렬로 처리되므로 전체 시간은 줄어든다.

- pipeline rate은 단계들 중 가장 오래 걸리는 것에 영향을 받는다. 

- 각 단계에 걸리는 시간이 거의 같고, 일이 충분히 많다면

  Potential speedup = Number of pipe stages




## Pipeline Hazard

### Structural Hazard

- 한 사이클 안에서 두개 이상의 명령이 같은 하드웨어를 사용하고자 하는 경우
- 해결법
  1. Stall:  이전 명령어가 해당 하드웨어를 다 쓸 때까지 기다렸다가 "버블"을 만들고 다음 명령을 처리한다. 쉽고 저렴하지만 CPI가 증가한다.
  2. 하드웨어적인 해결





### Data Hazard(Data Dependency)

- 서로 다른 명령이 같은 저장공간을 사용하고자 하는 경우(명령이 아직 파이프라인에 있는 이전 명령에 대하여 dependency가 있는 경우)

- Read After Write(RAW)

  - I가 r1을 쓰기 전에 다음 명령인 J가 r1을 읽으려고 하는 경우

    ```
    I:	add	r1, r2, r3
    J:	sub	r4, r1, r3
    ```

  - 해결법: Stall, Forwarding(이전 명령에서 값을 레지스터에 쓰기까지 기다리지 않고, ALU에서 계산이 끝나자마자 가져온다) 

  - Forwarding을 해도 데이터 해저드가 일어나는 경우도 있다. 하지만 forwarding을 하지 않는 것보다는 stall을 덜 할 수 있다. 
  
  - Pipeline scheduling으로 명령의 순서를 바꿔 stall을 줄일 수 있다. 





### Control Hazard

- 다음 PC를 어떻게 결정할 것인가?(분기를 할 것인가 말 것인가). 

- PC에 대한 dependency때문에 발생한다고 할 수 있다. 

- 해결법

  1. Stall

  2. Predict branch not taken: 

     ​	분기가 일어나지 않는다고 가정하고 다음 명령어들을 실행한다. 만약 분기가 일어나면 실행했던 명령들을 버리고(flush) Target address에서 명령을 실행한다. 

  3. Predict branch taken:

     ​	 분기가 일어났다고 가정하고 분기 목적지에서 명령을 실행한다. 만약 분기가 일어나지 않으면 분기점의 다음 명령어들을 실행한다. 메모리 주소를 업데이트해야하므로 한 사이클정도 손해이다. 

  4. Taken backwards, Not taken forwards:

     ​	Target address가 현재 위치의 뒤에 있으면 분기가 일어난다고 가정하고, 앞에 있으면 분기가 일어나지 않는다고 가정한다. 분기 목적지가 뒤에 있는 경우(e.g. for문, while문)는 분기가 일어날 확률이 높다는 것을 이용하는 방법이다. 

  5. Delayed branch:

     ​	Pipeline scheduling과 비슷한 컨셉이다.  분기를 할 지 말 지 결정되는 동안 분기와 상관 없이 실행되어야 하는 명령어들이 실행되도록 명령어들의 위치를 조정한다.

     

  
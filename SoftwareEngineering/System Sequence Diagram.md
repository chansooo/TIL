# System Sequence Diagram

# System sequence Diagram(SSD)

- system과 관련있는 input event와 output event를 시각화
- 또다른 requirement인 operation contract들을 도출하는데 활용
- 다음 단계인 design에서 사용됨
- UML sequence diagram을 사용함

## SSD가 포함하는 것

- external actors
- system
    - 블랙박스 형태로 취급
- system event
    - system 사이에서 오고가는 것

- system 아래로 가면서 더 뒤에 일어나는 것임
    
    

## SSD를 왜 그릴까요?

system event(외부의 input이벤트)가 무엇이 있는가 파악하기에 좋다~

system event가 포함하는 것

- actor로부터 발생한 이벤트들
- 시간에 관련된 이벤트들
- 고장이나 예외적인 상황

SSD는 system의 behavior를 설명한다

- what에 대한 것을 집중하는 것이지 어떻게 구현할 것인지를 생각하는 것이 아님
    - 그래서 blackbox로 생각한다
    

## UP에서 iterative한 SSD

- 주로 elaboration의 iteration을 거치며 많이 파악됨
    - 이 시스템이 대응해야하는 이벤트들을 파악할 수 있기 때문에
    - operation contract를 작성할 때 유용함
    - api의 러프한 이름과 같은 것들, 기간 등을 추정할 수 있음

# 7. Syntax Analysis: SLR parsing

앞서 LR(0) parsing은 reduce or shift conflict가 발생하기 때문에 SLR 방식을 사용하려고 한다.

## SLR parsing with DFA

---

![스크린샷 2022-05-11 00.42.21.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.42.21.png)

LR(0)와 비교했을 때 Reduce의 조건이 추가되었다.

다음 input symbol인 b가 Follow(X)에 포함되는지 확인하는 것이다.

⇒ 즉, reduce하려는 production 뒤에 b가 올 수 있는지를 지금 미리 확인하는 것이다!!!

이 때 X는 DFA의 qi state에서 포함하고 있는 production이다.

## 예제

---

LR(0) parsing의 예제에서 Conflict 발생한 부분을 다시 보자

![스크린샷 2022-05-11 00.46.35.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.46.35.png)

LR(0)에서는 E → T를 이용하여 reduce할지, 다음 input symbol인 *를 이용하여 shift할지 결정할 수 없었다.

SLR에서는 E → T로 reduce를 하려면 다음 input symbol인 *가 Follow(E)에 포함되어야 한다.

하지만 Follow(E) = {), $} 이므로 reduce를 진행할 수 없고, 따라서 Shift를 진행한다.

![스크린샷 2022-05-11 00.52.20.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.52.20.png)

(T* 에 대해 DFA 진행하면 위와 같고, qi에서 reduce 가능한 production은 없다. 따라서 shift 한다.

![스크린샷 2022-05-11 00.54.44.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.54.44.png)

suffix인 id에 대해 T→id로 reduce를 하려고 한다. 그러면 다음 input symbol인 )가 Follow(T)에 포함되어야 한다.

Follow(T) = {*, ), $} 이므로 reduce를 진행한다.

![스크린샷 2022-05-11 00.56.22.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.56.22.png)

(T*T에 대해 DFA를 진행하면 suffix인 T에 대해 qi에서 reduce가능한 production E → T.가 있다.

Follow(E) = {), $} 이므로, )를 포함해서 E→T로 reduce를 진행한다

![스크린샷 2022-05-11 00.58.51.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_00.58.51.png)

(T*E에 대해 DFA를 진행했을 때 suffix T*E에 대해 E→T*E가 있다.

Follow(E)에 )가 포함되므로 reduce시킨다.

![스크린샷 2022-05-11 01.00.36.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_01.00.36.png)

(E에 대해 DFA를 진행하면 다음과 같은데, reduce 가능한 production이 없으므로 )를 이용해 shift한다.

![스크린샷 2022-05-11 01.02.06.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_01.02.06.png)

(E)에 대해 DFA를 진행하면 T→(E)가 있다.

Follow(T) = {*,),$} 이므로 다음 input symbol $를 포함하니까 reduce시킨다.

![스크린샷 2022-05-11 01.03.24.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_01.03.24.png)

T에 대해 DFA를 진행시키면 E→T가 있다.

마찬가지로 Follow(E)는 $를 포함하므로 reduce시킨다.

![스크린샷 2022-05-11 01.04.08.png](7%20Syntax%20Analysis%20SLR%20parsing%20b76d1d6bc5334e999ea1ffee6dadc237/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-11_01.04.08.png)

dummy start symbol인 S’에서 출발하는 S’→E로 reduce가 가능하므로 **accept**

## SLR CFG

---

만약 ambiguous하면 SLR을 사용하더라도 conflict가 발생할 것이다.

예를 들어, E → E+E | E*E | id 일 때

input: id+id*id가 들어온다면

E+E|*id 상태에서 reduce될 것이다.

⇒ 우리가 원하는 결과는 shift인데 다른 트리가 생성된다.

### solution

*가 +보다 우선순위를 가지도록 정의해준다.
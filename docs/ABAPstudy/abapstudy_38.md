---
layout: default
title: 38. Function
parent: abapstudy
nav_order: 38
---

# 38. Function

우선 Function은 Subroutine처럼 모듈화하고 재사용이 가능하다.

- Subroutine과 Function의 차이
  1. Function은 Function Group이라는 Pool에 속해야 한다
  2. Function은 예외처리 기능을 제공하여 에러가 발생하면 예외 사항을 호출한 프로그램에 전달 할 수 있다.
  3. Function은 호출 프로그램에 상관없이 자체적으로 테스트를 할 수 있다.

여기서 Function Group은 Function을 모아 둔 곳으로, 별도로 수행하지는 않는다.

## Function 생성 (SE37)

Function Group을 생성 후 Function을 생성. Function Group이 Program이고 Function Module이 Program 내부에 있는 Sub-Rouine이라고 생각하면 된다.

다만 차이가 있다면 프로그램을 작성할 때 하나의 프로그램에 모든 내용을 처리하도록 하였지만, Function은 개별적으로 하나의 프로그램들을 구성하고 있다.

따라서 Function Group은 이들 개별적인 프로그램들을 모두 Include 명령문으로 기술하고 있어야 한다. 

실제적으로 하나의 Function Module을 만들고 그 Function Module을 포함하고 있는 Function Group의 본문을 확인 해 보면 Include가 하나 더 추가되는 것을 확인 할 수 있다.

Function을 작성하고자 할 때에는 작성하는 내용을 위하여 필요한 변수들이 어떤 것들이 있는지를 확인해야 하며, 필요한 변수들은 Import/Export Parameter들로 정의를 해 주어야 한다.

만약 필요한 내용들 중에 Internal Table의 형태가 있다면, Tables라는 정의 부분에 정의를 해 주어야 한다. 물론 필요한 변수들이 없다면 Import/Export Parameter들로 정의를 할 필요는 없다.

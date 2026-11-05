## Action이란?

ros2에선 토픽, 서비스, 액션이 있다. 토픽은 상대의 대답 상관없이 계속 값을 보내고, 서비스는 답을 기다린다. 목표지점까지 가라 같은 명령에 서비스를 사용하면, 오랫동안 기다려야하고, 중간에 얼마나 진행됬는지 알 수 없고, 취소할 수 없다. 여기서 액션을 사용하면 목표를 보내도 진행상황을 알려주고, 중간에 취소할 수 있다.

## Action Client
rotator는 위의 과정을 파이썬 코드로 만든 액션 클라이언트이다. 액션 서버(turtlesim)가 준비될 때까지 기다렸다가 목표 각도를 보내고, Feedback이 올 때마다 남은 각도를 출력하며, Result를 받으면 스스로 종료된다.

[Screencast from 2026-11-05 13-40-08.webm](https://github.com/user-attachments/assets/e828ad04-0550-40ca-9f4e-e1ee75072b0a)

현재 상황을 실시간으로 알려주는 것을 볼 수 있다.

[Screencast from 2026-11-05 13-48-04.webm](https://github.com/user-attachments/assets/67c48250-a1b0-4738-8544-fd3c15b8855a)

터미널을 보면 목표에 도달하면 성공이 나오고, 중간에 취고한 것은 취소됬다고 나온다.

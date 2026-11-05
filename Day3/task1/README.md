# TF2란?
누가 누구 기준으로 어디에 있는지를 말해준다. 라이더 센서가 장애물을 감지해서 map에 표시하면, 장애물과의 거리는 라이더가 기준이 되고 로봇이 기준이 되지 않는다. 이런 문제를 해결하기 위해 base_link를 정해둔다. base_link를 로봇 아무런 곳(편한 곳)에 지정해 두면, base_link기준으로 라이더가 어디에 있고, 라이다가 측정한 장애물과 base_link와 라이더 사이의 위치 관계를 고려하여 장애물과 로봇의 위치를 보다 정확하게 특정할 수 있다. 이제까지는 bask_link가 왜 필요한지에 대한 이유를 설명하고자한 예시이고, 이와 비슷한 이유로 트리 모양으로 연결한 좌표계에는 base_link가 들어간다. 

static TF와 dynamic TF는 값이 바귀는지, 바뀌지 않는지에 따라 나뉘어진다. 로봇에 있는 센서, 모터, 바퀴 같은 것들은 로봇이 움직임에 따라 변화가 없기때문에 static TF이고, world → turtle1 / map → odom같은 경우에는 거북이가 움직임에 따라 바뀌기 때문에 dynamic TF이다. 


## 1-1. 공식 데모
[Screencast from 2026-11-04 21-32-47.webm](https://github.com/user-attachments/assets/c0db1906-4fb0-46d0-8170-f32e0c3d22e6)

조종하는 거북이를 따라가는 것을 볼 수 있다. 

## 1-2. 직접 만든 노드
[Screencast from 2026-11-04 21-52-33.webm](https://github.com/user-attachments/assets/ea1c7673-27aa-4ede-8738-9659f08b6d07)

world → turtle1 좌표변환이 되고, Dynamic TF가 만들어지고 사용되어 거북이가 움직이는 것을 볼 수 있다.

## 여기서 한 것
과제 1,2,3을 합치기

## launch 파일 코드

```python
import os

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    # 1단계에서 만든 yaml 파일 위치
    config = os.path.join(
        get_package_share_directory('turtle_adv'), 'config', 'follow_params.yaml')

    return LaunchDescription([
        # (1) 거북이 창
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim',
        ),

        # (2) turtle1 위치를 TF로 발행
        Node(
            package='turtle_adv',
            executable='pose_broadcaster',
            name='broadcaster1',
            parameters=[{'turtlename': 'turtle1'}],
        ),

        # (3) turtle2 위치를 TF로 발행
        Node(
            package='turtle_adv',
            executable='pose_broadcaster',
            name='broadcaster2',
            parameters=[{'turtlename': 'turtle2'}],
        ),

        # (4) turtle2가 turtle1을 따라가게 하기
        Node(
            package='turtle_adv',
            executable='turtle_follower',
            name='turtle_follower',
            parameters=[config],
        ),
    ])
```
노드 4개를 실행하는 것은 리드미에 나온 정답 코드와 같다. 
얌 파일을 찾을 때, 정답같은 경우에는 A,B둘 다 굴러가야하므로 런치 파일이 있는 폴더 근처?에서 얌파일을 찾지만, 내 파일은 패키지에 넣고 빌드해서만 실행하니 설치 폴더에서 찾아도 충분하다. 정답처럼하면 A,B 둘 다 굴러가고 만약 못찾아도 기본값으로 실행되어 더 포괄적인 것 처럼 보인다. 근데 하다보니 다른 방법이 있어 그냥 해봤다.

## YAML 파라미터
YAML 파일의 맨 위에는 노드 이름인 turtle_follower를 적어서, 이 값들이 turtle_follower 노드에만 적용되게 하였다. 이때 YAML에 적은 노드 이름과 launch 파일의 name이 다르면 값이 적용되지 않고 기본값이 쓰이기 때문에, 둘을 똑같이 맞춰야 했다.

turtle_follower:
  ros__parameters:
    k_ang: 4.0
    k_lin: 0.5

새로 만든 launch 파일과 YAML 파일은 install 폴더로 복사되어야 ros2 launch가 찾을 수 있으므로, 파일을 추가한 뒤에는 colcon build를 다시 하였다.

### 결과
ros2 launch turtle_adv follow.launch.py 한 번으로 노드 4개(/sim, /broadcaster1, /broadcaster2, /turtle_follower)가 모두 실행되었고, turtle2가 (4, 2)에 생성되었다. teleop으로 turtle1을 움직이자 turtle2가 turtle1을 따라 이동하였다.

<img width="1372" height="2940" alt="Screenshot from 2026-11-05 14-18-53" src="https://github.com/user-attachments/assets/811e0d36-89b4-4acb-bb6a-e58a5ffbfc92" />

[Screencast from 2026-11-05 14-19-27.webm](https://github.com/user-attachments/assets/d5c964d0-b041-4f27-841d-6e66fca02cad)

---------------------------------------------------
view_frames로 확인한 TF 트리에서는 turtle1과 turtle2의 부모가 모두 world였고(ppt자료와 같음), 둘 다 약 62.7 Hz로 발행되고 있었다. 거북이 위치가 계속 바뀌기 때문에 계속 발행되는 dynamic TF임을 확인할 수 있었다.

<img width="3438" height="1688" alt="Screenshot from 2026-11-05 14-22-13" src="https://github.com/user-attachments/assets/eff4228f-e9bc-43a9-925d-677f7314729f" />

-------------------------------------------------------------
ros2 param get /turtle_follower k_ang으로 확인하자 YAML에 적은 값인 4.0이 출력되었다. YAML에서 k_ang을 1.0으로 바꾸고 다시 실행하자 turtle2가 방향을 더 천천히 틀었다. 코드를 고치지 않고 YAML만 바꿔서 동작을 조절할 수 있었다.

<img width="1380" height="2902" alt="Screenshot from 2026-11-05 14-24-29" src="https://github.com/user-attachments/assets/778b24bb-e85b-4be8-8222-9dc988680c4c" />

----------------------------------------------------------
rotator로 turtle1을 제자리에서 회전시키는 동안에도 turtle2는 멈추지 않고 turtle1의 위치를 향해 계속 이동하였다. 액션, TF 발행, 따라가기가 서로 방해하지 않고 동시에 동작한다는 것을 확인할 수 있었다. 단, turtle_follower는 turtle1의 위치만 따라가고 방향은 따라 하지 않기 때문에, 두 거북이가 이미 붙어 있을 때는 turtle1이 회전해도 turtle2에 눈에 띄는 변화가 없었다.

[Screencast from 2026-11-05 14-44-36.webm](https://github.com/user-attachments/assets/cef45f71-0f73-42c0-8d4a-0ff043943923)

영상이 좀 안예쁘게 나왔지만, 결과는 똑같다

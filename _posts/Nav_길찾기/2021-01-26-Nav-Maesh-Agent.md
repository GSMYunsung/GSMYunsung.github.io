---
 layout: post
 title: Unity_NavMesh
 date: 2021-01-26 4:10:47 +07:00
 tags: [unity, game, Project]
---

#### [플레이어 이동 설정]

---

먼저, 플레이어가 움직일 수 있게 이동 스크립트를 작성했습니다.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerM : MonoBehaviour
{
    Rigidbody rigid;
    float Hor;
    float Ver;
    public float speed;
    void Start()
    {
        
        rigid = GetComponent<Rigidbody>();        
    }

    void Update()
    {
      Hor = Input.GetAxisRaw("Horizontal");
      Ver = Input.GetAxisRaw("Vertical");

        transform.Translate(Vector3.right * speed * Time.deltaTime * Hor, Space.World);
        transform.Translate(Vector3.up * speed * Time.deltaTime * Ver, Space.World);
    }
}

```



#### [Nav Mesh Agent]

---

![ddd](C:\Users\user\Pictures\ddd.png)

적에게 추적기능을 넣어줍시다. `Nav Mesh Agent라는 컴포넌트를 추가`하고,

![image-20210126182526346](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210126182526346.png)

Window탭에 `AI > Navigation`이라는 창을 추가합니다.

그러면 Inspector창옆에 `Navigation`이라는 창이 생겨납니다.

그리고 `Bake 탭을 누르고 또 Bake`를 눌러주면됩니다.

![image-20210126182856491](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210126182856491.png)

하지만 아직 Bake를 눌러도 아무런 표시가 뜨지않습니다.

그 이유는, 우리가 해당 오브젝트에 대해 걸어갈 수 있는길. 통과할 수 없는 

장애물등을 지정 해주지 않았기 때문입니다.

우리는 이것을 위해 장애물이나 길 오브젝트에 Static을 넣어주셔야 합니다.

![image-20210126183057527](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210126183057527.png)

그 후 다시 Break버튼을 눌러보면,

![image-20210126183139316](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210126183139316.png)

이런식으로 상대플레이어 즉 AI가 갈 수있는 길이 나타난것을 알 수있습니다.

다시 Navigation 탭에 Bake탭에서 Bake를 눌러주시면 보시는 것 처럼 이동할 수 있는 공간을 색상으로 표현해줍니다.

만약 장애물로 만들 오브젝트를 Static화 시키면 저런 형태로 됩니다.  맵에 있는 큐브의 지면은 파란색으로 색칠이 되어 있지 않은 상태 즉, Nav Mesh Agent 컴포넌트를 추가시킨 오브젝트(적)가 지나갈 수 없게 되어 장애물이 되는 겁니다.



#### [Nav Maesh 코드작성]

---

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class AI : MonoBehaviour
{
    NavMeshAgent nav;
    GameObject target;
    void Start()
    {
        nav = GetComponent<NavMeshAgent>(); //네브설정
        target = GameObject.Find("Player"); 
        //태그가 플레이어인 오브젝트의 위치를 타겟에 넣어줌
    }

    void Update()
    {
        if(nav.destination != target.transform.position) 
            // 만약 네비게이션의 목적지가 플레이어의 포지션이 아니라면?
        {
            nav.SetDestination(target.transform.position);
            //네비게이션의 목적지를 플레이어의 포지션으로 설정한다.
        }
        else // 만약 네비게이션의 목적지가 플레이어의 포지션이 맞다면?
        {
            nav.SetDestination(transform.position);
            //그자리에 있는다.
        }
    }
}
```

그 후에 상대 오브젝트에 추가해줄 스크립트를 작성해줍니다.

+ `[스크립트에 설명]`

다시한번 설명을 하자면 nav변수에 Nav Mesh Agent 컴포넌트를 넣어주고, target 게임 오브젝트에 

플레이어의 위치를 넣어 계속 플레이어의 위치를 받아와 그 곳으로 Nav Mesh Agent 의 목적지를 설정해주는 코드입니다.

그리고 실행시키면 플레이어를 따라오는 상대(AI)를 볼 수있습니다.

![image-20210126184212263](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210126184212263.png)



##### Resources

-[Nav Maesh Agent_블로그](https://solution94.tistory.com/19)
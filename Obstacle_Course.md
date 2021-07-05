# Obstacle Course

```c#
// Mover
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Mover : MonoBehaviour
{
    // Private --> Public (Unity 안에서 자유자재로 값 변경이 가능하다)
    [SerializeField] float moveSpeed = 10f;

    void Start()
    {
        PrintInstructions();        
    }

    void Update()
    {
        MovePlayer();
    }

    // Camel-Case
    // Nothing to return
    void PrintInstructions() 
    {
        Debug.Log("Welcome to the game!");
        Debug.Log("Move Your Player with WASD or arrow keys");
        Debug.Log("Don't hit the walls!");
    }

    void MovePlayer() 
    {
        float xValue = Input.GetAxis("Horizontal") * Time.deltaTime * moveSpeed;
        float zValue = Input.GetAxis("Vertical") * Time.deltaTime * moveSpeed;
        transform.Translate(xValue, 0, zValue);
    }
}
```

```c#
// ObjectHit
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ObjectHit : MonoBehaviour
{
    // 누가 충돌했는가? 를 검사하는 역할을 한다.
    private void OnCollisionEnter(Collision other) 
    {
        // == means "is it exactly same?"
        if (other.gameObject.tag == "Player")
        {
            // Hit으로 Tag 이름이 변경되는 것을 확인할 수 있다.
            GetComponent<MeshRenderer>().material.color = Color.red;
            gameObject.tag = "Hit";
        }
    }
}
```

```c#
// Scorer
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Scorer : MonoBehaviour
{
    int hits = 0;

    private void OnCollisionEnter(Collision other) 
    {   
        // hits = hits + 1;
        if (other.gameObject.tag != "Hit") 
        {
            hits++;
            Debug.Log("You've bumped into a thing this many times: " + hits);                
        }
    }
}

```


```c#
// Dropper
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Dropper : MonoBehaviour
{
    MeshRenderer renderer;
    private Rigidbody rigidbody;
    [SerializeField] float timeToWait = 5f;
    // Start is called before the first frame update
    void Start()
    {
        // Dropper will disappear
        // GetComponent<MeshRenderer>().enabled = false;
        renderer = GetComponent<MeshRenderer>();
        rigidbody = GetComponent<Rigidbody>();

        renderer.enabled = false;
        rigidbody.useGravity = false;
    }

    // Update is called once per frame
    void Update()
    {

        // 5초 후에 Dropper가 보이고 떨어질 것이다.
        if (Time.time > timeToWait) 
        {
            // Debug.Log("5 seconds has elapsed");
            renderer.enabled = true;
            rigidbody.useGravity = true;
        }
    }
}
```


```c#
// Spinner
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Spinner : MonoBehaviour
{
    [SerializeField] float xAngle = 0;
    [SerializeField] float yAngle = 0;
    [SerializeField] float zAngle = 0;


    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    // https://docs.unity3d.com/kr/530/ScriptReference/Transform.html
    // e 키를 눌러서 회전한다고 생각하면 이해가 쉽다. (이거를 코드로 구현)
    // Rotate Method 사용
    void Update()
    {
        // transform.Rotate(x, y, z)
        // (1, 1, 1) 1 degree
        transform.Rotate(xAngle, yAngle, zAngle);
    }
}
```


Выполнил: Сериков Д.А.

Группа: ЭВТ-70

Игровой движок: Unity 2022.1.23

Название работы: разработка игры TileMap

Лабораторная работа №21.

Тема: разработка игры 2 CARS

Цель: разработать игру 2 CARS

Оборудование: компьютер.  

Ход работы

1. Выполнение работы

![image](https://user-images.githubusercontent.com/119733911/205501896-fb921fe8-253c-47bc-8bc0-369aeacc9fd1.png)
Рисунок 21.1 Импортировали компоненты

![image](https://user-images.githubusercontent.com/119733911/205501945-c4aa305b-eb65-46bb-b715-2fbbc15aa556.png)
Рисунок 21.2 Префаб Circle Blue

![image](https://user-images.githubusercontent.com/119733911/205501993-b34898c9-d52e-4074-8d66-244488b6e5d9.png)
Рисунок 21.3 Вид из окна Game

![image](https://user-images.githubusercontent.com/119733911/205502067-39d50082-59ba-4da1-8c39-b7ae37bd7f58.png)
Рисунок 21.4 Визуализация игровых объектов

Листинг 21.1 Car.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Car : MonoBehaviour
{
    public bool FirstLaneBlueCar, FirstLaneOrangeCar;
    public bool BlueCar;
    public Vector2 Xpos;




    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if(Input.GetKeyDown("left"))
        {
            LeftButtonPressed();
        }
        if (Input.GetKeyDown("right"))
        {
            RightButtonPressed();
        }
        if(BlueCar)
        {
            if(FirstLaneBlueCar)
            {
                transform.position = Vector3.Lerp(transform.position, new Vector3(-Xpos.y, transform.position.y, 0), .1f);
            }
            else
            {
                transform.position = Vector3.Lerp(transform.position, new Vector3(-Xpos.x, transform.position.y, 0), .1f);
            }
        }
        else
        {
            if(FirstLaneOrangeCar)
            {
                transform.position = Vector3.Lerp(transform.position, new Vector3(Xpos.y, transform.position.y, 0), .1f);
            }
            else
            {
                transform.position = Vector3.Lerp(transform.position, new Vector3(Xpos.x, transform.position.y, 0), .1f);
            }
        }
    }
    public void LeftButtonPressed()
    {
        if (FirstLaneBlueCar) { FirstLaneBlueCar = false; } else { FirstLaneBlueCar = true; }
    }
    public void RightButtonPressed()
    {
        if (FirstLaneOrangeCar) { FirstLaneOrangeCar = false; } else { FirstLaneOrangeCar = true; }
    }

    public void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.tag == "circle")
        {
            //give points
            FindObjectOfType<GameController>().AddScore();
            Destroy(collision.gameObject);
        }
        if(collision.tag == "Cube")
        {
            FindObjectOfType<GameController>().GameOver();
            Debug.Log("playerGO");
        }
    }
}

Листинг 21.2 CircleSquareGo.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CircleSquareGo : MonoBehaviour
{
    int speed;
    Rigidbody2D rgbd;
    void Start()
    {
        speed = 10;
        rgbd = GetComponent<Rigidbody2D>();
        rgbd.velocity = new Vector2(0, -speed);
    }

    
}




Листинг 21.3 EdgeCollider.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EdgeCollider : MonoBehaviour
{

    public void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "circle")
        {
            //gameOver
            Destroy(collision.gameObject);
        }
        else if(collision.tag == "Cube")
        {
            //destroy go
            Destroy(collision.gameObject);
        }
    }
}

Листинг 21.4 GameController.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameController : MonoBehaviour
{
    //Circle And Square GO
    public GameObject[] BlueGO;
    public GameObject[] OrangeGO;
    // To instantiate squate and circle
    public float startWait;
    public float spawnWait;

    GameObject Blue, Orange;

    float[] XPosition = new float[2] { 1.5f, 4.6f };
    bool GameOverBool;
    int Score;
    public Text ScoreText;
    public GameObject GameOverCanvas;
    void Start()
    {
        GameOverBool = false;
        Time.timeScale = 1;
        GameOverCanvas.SetActive(false);

    }

    // Update is called once per frame
    void Update()
    {

    }
    IEnumerator SpawnObjects()
    {
        yield return new WaitForSeconds(startWait);
        while(true)
        {
            for ( int i = 0; i< 50; i++)
            {
                float OrangeXpos = XPosition[Random.Range(0, XPosition.Length)];

                Vector3 OrangePos = new Vector3(OrangeXpos, 10, 0);

                Orange = OrangeGO[Random.Range(0, OrangeGO.Length)] as GameObject;

                Instantiate(Orange, OrangePos, Quaternion.identity);
                yield return new WaitForSeconds(spawnWait);

                float BlueXpos = -XPosition[Random.Range(0, XPosition.Length)];

                Vector3 BluePos = new Vector3(BlueXpos, 10, 0);
                Blue = BlueGO[Random.Range(0, BlueGO.Length)] as GameObject;
                Instantiate(Blue, BluePos, Quaternion.identity);
                yield return new WaitForSeconds(spawnWait);

            }
        }
    }
    public void StartGame()
    {
        StartCoroutine(SpawnObjects());
    }
    public void GameOver()
    {
        GameOverBool = true;
        Time.timeScale = 0;
        GameOverCanvas.SetActive(true);
    }
    public void AddScore()
    {
        Score += 1;
        ScoreText.text = Score.ToString();
    }
    public void Restart()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}

2.Вывод:
В ходе проделанной работы, мы разработали игру 2 CARS.

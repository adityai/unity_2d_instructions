## Initial Setup
1. Open Unity Hub and create a new 2D project[4].
2. Select a 2D template and name your project.

## Creating Game Objects

**Player Setup**
1. Create a circle sprite for the player:
   - Right-click in Hierarchy > 2D Object > Sprites > Circle[4]
   - Rename it to "Player"
   - Add Rigidbody2D component with gravity scale set to 0[1]
   - Add CircleCollider2D component[1]

**Predator Setup**
1. Create a square sprite:
   - Right-click in Hierarchy > 2D Object > Sprites > Square[4]
   - Rename it to "Predator"
   - Add Rigidbody2D component with gravity scale set to 0
   - Add BoxCollider2D component

**Goody Setup**
1. Create a diamond shape:
   - Right-click in Hierarchy > 2D Object > Sprites > Square
   - Rename it to "Goody"
   - Rotate it 45 degrees to make it diamond-shaped
   - Add BoxCollider2D and set "Is Trigger" to true

## Scripts

1. Select the Player object in the Hierarchy.

2. Right Click on Assets -> Select 'Create' -> Select 'C# Script'

3. Name the script 'PlayerMovement' and double click the script object.

4. The script file will open in Visual Studio Code or a default text editor.

5. Edit the script file with the following code.

6. Save the script and go back to the Unity screen.

7. Unity will load the script and compile it into the game.

8. Drag the 'PlayerMovement' script object onto the 'Player' object.

9. Run the game and use the up, down, right and left keys to verify that the Player object is moving as expected.

**Player Movement Script**
```csharp
public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    private Rigidbody2D rb;
    
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }
    
    void Update()
    {
        float moveX = Input.GetAxisRaw("Horizontal");
        float moveY = Input.GetAxisRaw("Vertical");
        
        rb.velocity = new Vector2(moveX * moveSpeed, moveY * moveSpeed);
    }
}
```

**Game Manager Script**
```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public int lives = 3;
    public int score = 0;
    
    void Awake()
    {
        Instance = this;
    }
    
    public void DecrementLives()
    {
        lives--;
        if (lives <= 0)
        {
            GameOver();
        }
    }
    
    private void GameOver()
    {
        Time.timeScale = 0;
        // Add game over UI logic here
    }
}
```

**Predator AI Script**
```csharp
public class PredatorAI : MonoBehaviour
{
    public float minSpeed = 3f;
    public float maxSpeed = 7f;
    private Transform player;
    private Rigidbody2D rb;
    
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        player = GameObject.FindGameObjectWithTag("Player").transform;
    }
    
    void Update()
    {
        Vector2 direction = (player.position - transform.position).normalized;
        float currentSpeed = Random.Range(minSpeed, maxSpeed);
        rb.velocity = direction * currentSpeed;
    }
    
    void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Player"))
        {
            Vector2 bounceDirection = (collision.transform.position - transform.position).normalized;
            collision.gameObject.transform.position += (Vector3)(bounceDirection * 3);
            GameManager.Instance.DecrementLives();
        }
    }
}
```

**Goody Script**
```csharp
public class Goody : MonoBehaviour
{
    private float spawnTime;
    private float disappearTime = 10f;
    
    void OnEnable()
    {
        spawnTime = Time.time;
        Invoke("Despawn", disappearTime);
        RandomizePosition();
    }
    
    void RandomizePosition()
    {
        float randomX = Random.Range(-8f, 8f);
        float randomY = Random.Range(-4f, 4f);
        transform.position = new Vector2(randomX, randomY);
    }
    
    void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.CompareTag("Player"))
        {
            float timeSinceSpawn = Time.time - spawnTime;
            if (timeSinceSpawn <= 3f)
                GameManager.Instance.score += 10;
            else if (timeSinceSpawn <= 7f)
                GameManager.Instance.score += 5;
            else
                GameManager.Instance.score += 1;
                
            gameObject.SetActive(false);
        }
    }
    
    void Despawn()
    {
        gameObject.SetActive(false);
    }
}
```

## Final Setup
1. Tag your Player object with "Player" tag[1].
2. Create a UI canvas for displaying lives and score.
3. Attach the scripts to their respective game objects.
4. Set up boundaries to keep objects within the game area using colliders[5].
5. Create a simple spawning system for the Goody to appear every few seconds.

Test the game by hitting the Play button. The circle should move with arrow keys or WASD, the predator should chase it, and goodies should appear randomly for scoring points.

Citations:
[1] https://www.youtube.com/watch?v=vA-jv_fP5EE
[2] https://www.youtube.com/watch?v=Pkc4A1ukbJU
[3] https://www.youtube.com/watch?v=RuvfOl8HhhM
[4] https://blog.udemy.com/how-to-make-a-2d-game-in-unity/
[5] https://www.youtube.com/watch?v=jyXZ3RVe5as
[6] https://www.youtube.com/watch?v=0-c3ErDzrh8
[7] https://www.instructables.com/Make-A-2D-Infinite-Runner-with-Unity/
[8] https://discussions.unity.com/t/2d-mouse-point-click-movement-system-quick-tutorial/523253
[9] https://www.youtube.com/watch?v=Cc2nUl81iaI
[10] https://www.youtube.com/watch?v=QplEeEAJxck
[11] https://www.youtube.com/watch?v=ee3CQvi3Oqs
[12] https://www.youtube.com/watch?v=NvlJLxnXvdk
[13] https://www.youtube.com/watch?v=dwcT-Dch0bA
[14] https://discussions.unity.com/t/player-controls-in-2d-game/694923
[15] https://www.youtube.com/watch?v=K1xZ-rycYY8
[16] https://www.youtube.com/watch?v=IGDrF1Cq9Q0
[17] https://discussions.unity.com/t/2d-platformer-movement-mechanics/866697

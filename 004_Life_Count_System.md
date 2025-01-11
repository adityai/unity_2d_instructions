## Core Implementation

**Basic Life System Setup**
Create a GameManager script to handle the life system with these essential components:
```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance;
    public int maxLives = 3;
    private int currentLives;
    
    void Awake()
    {
        Instance = this;
        currentLives = maxLives;
    }
    
    public void DecrementLives()
    {
        currentLives--;
        UpdateLifeDisplay();
        
        if (currentLives <= 0)
        {
            GameOver();
        }
    }
    
    private void GameOver()
    {
        Time.timeScale = 0;
        // Add game over scene loading logic here
    }
}
```

## UI Implementation

**Life Display Setup**
1. Create a UI Canvas
2. Add UI elements to display lives:
```csharp
public class LifeDisplay : MonoBehaviour
{
    [SerializeField] private Text livesText;
    
    void UpdateDisplay()
    {
        livesText.text = "Lives: " + currentLives.ToString();
    }
}
```

**Visual Heart System**
For a more visual approach using heart icons:
```csharp
public class HeartSystem : MonoBehaviour
{
    public GameObject[] hearts;
    
    void UpdateHearts(int currentLives)
    {
        for(int i = 0; i < hearts.Length; i++)
        {
            hearts[i].SetActive(i < currentLives);
        }
    }
}
```

## Persistence Between Scenes

To maintain life count between scenes:
```csharp
void SaveLives()
{
    PlayerPrefs.SetInt("lives", currentLives);
    PlayerPrefs.Save();
}

void LoadLives()
{
    currentLives = PlayerPrefs.GetInt("lives", maxLives);
}
```

## Collision Detection

Add this to your player script to handle damage:
```csharp
void OnCollisionEnter2D(Collision2D collision)
{
    if (collision.gameObject.CompareTag("Enemy"))
    {
        GameManager.Instance.DecrementLives();
        // Add player respawn or knockback logic here
    }
}
```

Make sure to:
1. Tag all hazardous objects appropriately
2. Set up proper collision layers
3. Implement respawn points or checkpoints
4. Add visual feedback when losing lives[3][12]

Citations:
[1] https://www.youtube.com/watch?v=Ay159WsGDJQ
[2] https://www.youtube.com/watch?v=zr7ys5lFakA
[3] https://www.youtube.com/watch?v=q5Kt45eZvNQ
[4] https://stackoverflow.com/questions/53457110/how-to-make-a-live-counter-in-unity3d
[5] https://www.youtube.com/watch?v=Xn_is38uqRg
[6] https://discussions.unity.com/t/lives-script/780717
[7] https://www.youtube.com/watch?v=nZqU4Q_zxsY
[8] https://discussions.unity.com/t/creating-players-remaining-lives-system-prior-game-over/919932
[9] https://discussions.unity.com/t/life-counter/376211
[10] https://discussions.unity.com/t/player-lives-script/115152
[11] https://www.reddit.com/r/gamedev/comments/125tdz3/lives_counter_in_unity_tutorial/
[12] https://kozmobot.com/2023/04/05/lives-counter/
[13] https://www.youtube.com/watch?v=-tjoAUVwRjA
[14] https://www.youtube.com/watch?v=Xn_is38uqRg
[15] https://stackoverflow.com/questions/60384659/lives-implementation-in-unity2d
[16] https://discussions.unity.com/t/adding-lives-system-to-my-game/156209
[17] https://www.youtube.com/watch?v=iX0BEiJTjrE
[18] https://blog.devgenius.io/day-21-of-game-dev-easily-setup-ui-in-unity-a51110f8f9bf?gi=857d6e826235
[19] https://www.youtube.com/watch?v=Ay159WsGDJQ
[20] https://www.youtube.com/watch?v=c9z30K0TVRQ
[21] https://discussions.unity.com/t/creating-players-remaining-lives-system-prior-game-over/919932
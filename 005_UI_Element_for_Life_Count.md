Here's how to implement a UI life display system in Unity:

## Canvas Setup
1. Create a UI Canvas in the Hierarchy (Right-click > UI > Canvas)
2. Set Canvas Scaler to "Scale With Screen Size" for proper scaling across different resolutions[2]

## Creating Life Display

**Text-Based Display**
1. Create a UI Text element in your Canvas
2. Position it in the desired screen location (typically top-left or top-right)
3. Set proper anchoring to maintain position across different screen sizes[2]

```csharp
public class LifeDisplay : MonoBehaviour
{
    [SerializeField] private TextMeshProUGUI livesText;
    private int currentLives = 3;

    void Start()
    {
        UpdateLivesDisplay();
    }

    public void UpdateLivesDisplay()
    {
        livesText.text = "Lives: " + currentLives.ToString();
    }
}
```

**Image-Based Display**
1. Create UI Image elements for each life icon
2. Import your life icon sprite
3. Set "Preserve Aspect" on the Image component[2]

```csharp
public class LivesCounter : MonoBehaviour
{
    [SerializeField] private int maxLives = 3;
    [SerializeField] private GameObject[] lifeIcons;
    private int currentLives;

    void Start()
    {
        currentLives = maxLives;
        UpdateLivesDisplay();
    }

    public void UpdateLivesDisplay()
    {
        for (int i = 0; i < lifeIcons.Length; i++)
        {
            lifeIcons[i].SetActive(i < currentLives);
        }
    }

    public void DecrementLife()
    {
        currentLives = Mathf.Max(0, currentLives - 1);
        UpdateLivesDisplay();
        
        if (currentLives <= 0)
        {
            GameOver();
        }
    }

    private void GameOver()
    {
        Time.timeScale = 0;
    }
}
```

## Connection to Player
1. Add the lives display script to your Canvas
2. Reference it in your player's health/collision script
3. Call UpdateLivesDisplay() whenever the life count changes[1]

Make sure to:
- Set proper anchoring points for UI elements
- Test the display at different screen resolutions
- Update the life count when the player takes damage
- Handle the game over state when lives reach zero[3]

Citations:
[1] https://www.homeandlearn.co.uk/games-programming/player-lives.html
[2] https://blog.devgenius.io/day-21-of-game-dev-easily-setup-ui-in-unity-a51110f8f9bf?gi=857d6e826235
[3] https://www.youtube.com/watch?v=q5Kt45eZvNQ
[4] https://www.youtube.com/watch?v=Zwgj3mwOVlg

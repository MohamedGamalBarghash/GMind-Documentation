# Introduction:
This quest system provides players with goals, tasks, and objectives to complete, It manages quest states, requirements, steps, and rewards. here are the main parts needed to carry it out:
## Quest Manager:
 Quest Manager manages the quests in the game.
          Initialization:
loadQuestState determines whether the system should load the quest state from previous sessions.
questMap holds the quests in the game.
         Quest Step State Change:
QuestStepStateChange fun is called when a quest step's state changes. It updates the quest's step state and triggers a quest state change.
Start methodrepresents the current quest steps for quests in progress and triggers a quest state change.
Update method checks the requirements for quests and updates their states.
ChangeQuestState method updates the state of a quest.
StartQuest starts a quest by representing its current quest step and updating its state.
AdvanceQuest advances a quest to the next step.
FinishQuest finishes a quest, triggers rewards, and updates the state to finished.
CheckRequirementsMet checks if the player meets the requirements to start a quest, like player level for example. 
OnApplicationQuit saves the state of quests 
SaveQuest saves the quest's data.
LoadQuest loads a quest's data from PlayerPrefs.
## User Interface Manager:
The UI Manager script handles the user interface related to quests. 
        Quest UI Update:
The UpdateQuestUI method clears the existing quest UI elements 
         Quest Completion Display:

Showquest Completion method displays a message indicating the completion of a quest.
## Quest Step:
It contains methods and properties related to quest step tracking and completion. 
## Quest Step State:
QuestStepState class shows the state of a quest step. It contains a state variable that can hold information about the current state of it.
## Reward 
The Reward class represents the reward given to the player when the quest is completed. It contains a type (such as experience, currency, or item) and the amount of the reward.

## Event Manager and Quest Events:
The Event Manager handles the events related to quests. It contains the QuestEvents , which defines events like starting a quest, advancing a quest, finishing a quest, changing quest state, and changing quest step state.

![Quest System](https://github.com/MohamedGamalBarghash/GMind-Documentation/assets/26281629/eeacd3ed-4403-411b-9998-f58585f81ea1)

# Specifications:  
# Quest Class
## Description:
Represents a quest in a game, storing information about its state and progress.
## Attributes:
``` c#
public QuestDataScriptableObject info;               // Static information about the quest.
public QuestState state;                             // The current state of the quest.
public int questId;                                  // The unique identifier for the quest.
private int currentQuestStepIndex;                   // The index of the current quest step within the quest.
private QuestStepState[] questStepStates;            // The states of individual quest steps.
```
## Constructors:
###### Initializes a new quest instance with the provided quest data created with the Quest class.
#### Parameters: 
- `questInfo` --> The static information about the quest.
``` c#
public Quest(QuestDataScriptableObject questInfo)
```

###### Initializes a new quest instance with saved state information.
#### Parameters: 
- `questInfo` --> The static information about the quest.  
- `questState` --> The saved state of the quest.  
- `currentQuestStepIndex` --> The index of the current quest step.  
- `questStepStates` --> The saved states of individual quest steps.  
``` c#
public Quest(QuestDataScriptableObject questInfo, QuestState questState, int currentQuestStepIndex, QuestStepState[] questStepStates)
```

## Functions:
### - MoveToNextStep ():
###### Moves to the next step of the quest.
``` c#
public void MoveToNextStep()
```

### - CurrentStepExists ():
###### Checks if the current step of the quest exists.
- Returns: True if the current step exists; otherwise, false.
``` c#
public bool CurrentStepExists()
```

### - InstantiateCurrentQuestStep ():
###### Instantiates the current quest step as a game object.
#### Parameters: 
- `parentTransform` --> The transform where the quest step should be instantiated.
``` c#
public void InstantiateCurrentQuestStep(Transform parentTransform)
```

### - GetCurrentQuestStepPrefab ():
###### Retrieves the prefab for the current quest step.
- Returns: The quest step prefab for the current step, or null if it doesn't exist.
``` c#
private GameObject GetCurrentQuestStepPrefab()
```

### - StoreQuestStepState ():
###### Stores the state of a quest step at a specified index.
#### Parameters:
- `questStepState` --> The state of the quest step to be stored.  
- `stepIndex` --> The index of the quest step to store the state for.
``` c#
public void StoreQuestStepState(QuestStepState questStepState, int stepIndex)
```
        
### - GetQuestData ():
###### Retrieves the data representing the current state and progress of the quest.
- Returns: A QuestData object containing the quest's state and step states.
``` c#
public QuestData GetQuestData()
```
# Quest Data ScriptableObject
## Description:
Unity scriptable object designed to store and manage quest-related data in a game. such as their display names, description, prerequisites, steps, and rewards.
## Attributes:
``` c#
public int id;                                                     // The quest id (unique identifier)
public string displayName;                                         // The display name of the quest.
public string description;                                         // The description of the quest.
public int levelRequirement;                                       // The level requirement to start the quest.
public QuestDataScriptableObject[] questPrerequisites;             // Array of quest prerequisites required to unlock this quest.
public GameObject[] questStepPrefabs;                              // Array of quest step prefabs that get instantiated during the quest.
public Reward[] questRewards;                                      // Array of rewards for completing the quest.
```

# QuestStep Class
An abstract base class for implementing individual quest steps in a game.
``` c#
private bool isFinished = false;             // Whether the step is finished or not (used by the Quest to determine the upcoming step).
private int questId;                         // The quest the step belongs to.
private int stepIndex;                       // The index of the step between the other steps.
```
## Functions:
### - InitializeQuestStep ():
###### Initializes the quest step with relevant information.
#### Parameters: 
- `questId` --> The unique identifier of the associated quest.  
- `stepIndex` --> The index of this quest step within the quest.  
- `questStepState` --> The initial state of the quest step (optional).  
``` c#
public void InitializeQuestStep(int questId, int stepIndex, string questStepState)
```

### - FinishQuestStep ():
###### Marks the quest step as finished and triggers quest progression.
``` c#
protected void FinishQuestStep()
```

### - ChangeState() :
###### Notifies a change in the state of the quest step to the EventManager.
#### Parameters: 
- `newState` --> The new state of the quest step.
``` c#
protected void ChangeState(string newState)
```

### - SetQuestStepState ():
###### Sets the state of the quest step. Derived classes must implement this method.
#### Parameters: 
- `state` --> The new state of the quest step.
``` c#
protected abstract void SetQuestStepState(string state);
```

# QuestData class
## Description:
Represents the data associated with a quest's current state and progress.

## Attributes:
``` c#
public QuestState state;                  // The state of the quest (e.g., in progress, completed, etc.).
public int questStepIndex;                // The index of the current step within the quest.
public QuestStepState[] questStepStates;  // An array containing the states of individual quest steps.
```

## Constructors:
###### Initializes a new instance of the QuestData class with the specified values.
#### Parameters: 
- `state` --> The state of the quest.  
- `questStepIndex` --> The index of the current quest step.  
- `questStepStates` --> An array of quest step states.  
``` c#
public QuestData(QuestState state, int questStepIndex, QuestStepState[] questStepStates)
```

# QuestState Enum
## Description:
Enumeration representing the possible states of a quest.

## Attributes:
```
REQUIREMENTS_NOT_MET,        // The quest's requirements are not met, and it cannot be started.
CAN_START,                   // The quest can be started but is not yet in progress.
IN_PROGRESS,                 // The quest is currently in progress.
CAN_FINISH,                  // The quest can be finished but is not yet completed.
FINISHED                     // The quest has been successfully completed.
```

# QuestStepState Class
## Description:
Represents the state of an individual quest step.
## Attributes
``` c#
public string state;    // The state of the quest step.
```

## Constructors:

###### Initializes a new quest step state with the provided state value.

#### Parameters:
- `state` --> The initial state of the quest step.
``` c#
public QuestStepState(string state)
```

### Initializes a new quest step state with an empty state value.
``` c#
public QuestStepState()
```

# Reward Class

## Description
Represents a reward given to a player, which may include various types of rewards such as experience points, currency, items, or karma.

## Attributes
``` c#
public RewardType type;          // The type of reward.
public int amount;               // The amount or value of the reward.
```

## Constructors

###### Initializes a new reward instance with the specified type and amount.

#### Parameters:
- `t` --> The type of reward.
- `amt` --> The amount or value of the reward.
``` c#
public Reward(RewardType t, int amt)
```

# RewardType Enum

## Description
Enumeration representing the possible types of rewards that can be given to a player.

#### Values:
``` c#
Experience     // Represents experience points as a reward.
Currency       // Represents in-game currency as a reward.
Item           // Represents an item as a reward.
Karma          // Represents karma points as a reward.
```

# UIManager Class

## Description:
Manages the user interface (UI) elements related to quests and provides functions for updating and displaying quest-related information.

## Attributes:
``` c#
public static UIManager instance;    // A reference to the singleton instance of the UIManager.
public GameObject questListUI;       // The UI panel displaying the list of quests.
public GameObject questUIPrefab;     // The prefab used for creating quest UI elements.
```

## Functions:
### - Awake ():
###### Awake is a Unity method called when the script instance is loaded. In this case, it ensures that there is only one instance of the UIManager.
``` c#
private void Awake()
```

### - UpdateQuestUI ():
###### Updates the quest UI by clearing existing UI elements and populating it with active quests.
``` c#
public void UpdateQuestUI()
```

### - ShowQuestCompletion ():
###### Displays a UI element or message indicating quest completion.
``` c#
public void ShowQuestCompletion(QuestData quest)
```

## Usage:
- Call `UpdateQuestUI()` to refresh the quest list UI.
- Call `ShowQuestCompletion(quest)` to display quest completion information.

# EventManager Class

## Description:
Manages game events and provides access to various event-related functionalities.

## Attributes:
``` c#
public static EventManager instance { get; private set; }  // A reference to the singleton instance of the EventManager.
public QuestEvents questEvents;                            // An instance of the QuestEvents class for managing quest-related events.
```

## Functions:

### - Awake ():

###### Awake is a Unity method called when the script instance is loaded. In this case, it ensures that there is only one instance of the EventManager and initializes the `questEvents` instance of the QuestEvents class.
``` c#
void Awake()
```

## Usage:
- Access the EventManager singleton instance through `EventManager.instance`.
- Use `questEvents` to manage quest-related events and functionalities.

# QuestManager Class
## Description:
Class responsible for managing quests in a game. It handles quest initialization, progression, and completion.
## Attributes:
``` c#
[SerializeField] private bool loadQuestState;           //Determines whether the quest states should be loaded from the player's saved progress.
private int currentPlayerLevel;                         //Stores the current player level.
public Dictionary<int, Quest> questMap;                 //Dictionary that maps quest IDs to Quest objects for efficient quest management.
public static QuestManager instance;                    //Static reference to the QuestManager instance, utilizing the singleton pattern.
```

## Functions:
### - QuestStepStateChange ():
###### Handles changes in quest step states and updates the quest's overall state.
#### Parameters: 
- id: The ID of the quest.
- stepIndex: The index of the quest step.
- QuestStepState: The index of the quest step.
``` c#
private void QuestStepStateChange(int id, int stepIndex, QuestStepState questStepState)
```
### - CheckRequirementsMet ():
###### Checks if the player meets the requirements to start a quest, including player level and quest prerequisites.

#### Parameters: 
- quest: The quest to check requirements for.
``` c#
private bool CheckRequirementsMet(Quest quest)
```
### - ChangeQuestState ():
###### Updates the state of a specific quest and triggers a QuestStateChange event.

#### Parameters: 
- id: The ID of the quest to update.
- state: The new state of the quest.
``` c#
private void ChangeQuestState(int id, QuestState state)
```
### - StartQuest ():
###### Initiates a quest, instantiates the current quest step, and changes the quest state to in-progress.

#### Parameters: 
- id: The ID of the quest to start.
``` c#
public void StartQuest(int id)
```
### - AdvanceQuest ():
###### Advances a quest to its next step, instantiates the new step, and updates the quest state accordingly.

#### Parameters: 
- id: The ID of the quest to advance.
``` c#
public void AdvanceQuest(int id)
```
### - FinishQuest ():
###### Completes a quest, claims rewards, and changes the quest state to finished.

#### Parameters: 
- id: The ID of the quest to finish.
``` c#
public void FinishQuest(int id)
```
### - ClaimRewards ():
###### Placeholder for handling the granting of quest rewards (not implemented).

#### Parameters: 
- quest: The quest for which to claim rewards.
``` c#
private void ClaimRewards(Quest quest)
```
### - CreateQuestMap ():
###### Creates a dictionary mapping quest IDs to Quest objects by loading QuestDataScriptableObject assets.

#### Return: 
- Dictionary<int, Quest>: A dictionary mapping quest IDs to Quest objects.
``` c#
private Dictionary<int, Quest> CreateQuestMap()
```
### - GetQuestById ():
###### Retrieves a quest by its ID from the questMap dictionary.

#### Parameters: 
- id: The ID of the quest to retrieve.
#### Return:
- quest: The quest associated with the provided ID.
``` c#
private Quest GetQuestById(int id)
```
### - SaveQuest ():
###### Serializes and saves the state of a quest using PlayerPrefs.

#### Parameters: 
- quest: The quest to save.
``` c#
private void SaveQuest(Quest quest)
```
### - LoadQuest ():
###### Loads and initializes a quest from PlayerPrefs or creates a new quest if none exists.

#### Parameters: 
- questInfo: The QuestDataScriptableObject that defines the quest.
#### Return: 
- quest: The loaded or newly created quest.
``` c#
private Quest LoadQuest(QuestDataScriptableObject questInfo)
```
### - Awake ():
###### Initializes questMap and sets the QuestManager instance.

### - OnEnable ():
###### Registers event handlers for quest-related events when the script is enabled.

### - Start ():
###### Initializes quests, instantiates quest steps for in-progress quests, (unimplemented) updates the quest UI.

### - Update ():
###### Periodically checks if quest requirements are met and updates quest states, (unimplemented) updates the Quest UI.

### - OnApplicationQuit ():
###### Saves the state of all quests when the application quits.

### - On Disable ():
###### Unregisters event handlers for quest-related events when the script is disabled.

# QuestEvents Class

## Description:
Handles events related to quests and provides methods for triggering these events.

## Event Handlers:

- `public event Action<int> onStartQuest;`: Event handler for when a quest is started.
- `public event Action<int> onAdvanceQuest;`: Event handler for when a quest is advanced.
- `public event Action<int> onFinishQuest;`: Event handler for when a quest is finished.
- `public event Action<Quest> onQuestStateChange;`: Event handler for when the state of a quest changes.
- `public event Action<int, int, QuestStepState> onQuestStepStateChange;`: Event handler for when the state of a quest step changes.

## Functions

### - StartQuest ():

###### Triggers the `onStartQuest` event.

#### Parameters:
- `id` (int): The unique identifier of the quest.
``` c#
public void StartQuest(int id)
```

### - AdvanceQuest ():
###### Triggers the `onAdvanceQuest` event.

#### Parameters:
- `id` (int): The unique identifier of the quest.
``` c#
public void AdvanceQuest(int id)
```

### - FinishQuest ():
###### Triggers the `onFinishQuest` event.

#### Parameters
- `id` (int): The unique identifier of the quest.

``` c#
public void FinishQuest(int id)
```

### - QuestStateChange ():
###### Triggers the `onQuestStateChange` event.

#### Parameters
- `quest` (Quest): The quest object whose state has changed.
``` c#
public void QuestStateChange(Quest quest)
```

### - QuestStepStateChange ():
###### Triggers the `onQuestStepStateChange` event.

#### Parameters
- `id` (int): The unique identifier of the quest.
- `stepIndex` (int): The index of the quest step.
- `questStepState` (QuestStepState): The state of the quest step.
``` c#
public void QuestStepStateChange(int id, int stepIndex, QuestStepState questStepState)
```

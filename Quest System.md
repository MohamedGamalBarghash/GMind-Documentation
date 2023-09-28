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
public int id;                                             // The quest id (unique identifier)
public string displayName;                                 // The display name of the quest.
public string description;                                 // The description of the quest.
public int levelRequirement;                               // The level requirement to start the quest.
public QuestDataScriptableObject[] questPrerequisites;     // Array of quest prerequisites required to unlock this quest.
public GameObject[] questStepPrefabs;                      // Array of quest step prefabs that get instantiated during the quest.
public Reward[] questRewards;                              // Array of rewards for completing the quest.
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

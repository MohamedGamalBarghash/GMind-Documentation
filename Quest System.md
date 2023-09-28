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

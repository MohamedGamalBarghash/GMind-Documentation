# Quest Class
Represents a quest in a game, storing information about its state and progress.
```
public QuestDataScriptableObject info;               // Static information about the quest.
public QuestState state;                             // The current state of the quest.
public int questId;                                  // The unique identifier for the quest.
private int currentQuestStepIndex;                   // The index of the current quest step within the quest.
private QuestStepState[] questStepStates;            // The states of individual quest steps.
```
## Constructors:
###### Initializes a new quest instance with the provided quest data created with the Quest class.
Parameters: - questInfo --> The static information about the quest.
```
public Quest(QuestDataScriptableObject questInfo)
```

###### Initializes a new quest instance with saved state information.
Parameters: - questInfo --> The static information about the quest.
            - questState --> The saved state of the quest.
            - currentQuestStepIndex --> The index of the current quest step.
            - questStepStates --> The saved states of individual quest steps.
```
public Quest(QuestDataScriptableObject questInfo, QuestState questState, int currentQuestStepIndex, QuestStepState[] questStepStates)
```

## Functions:
### - MoveToNextStep ():
###### Moves to the next step of the quest.
```
public void MoveToNextStep()
```

### - CurrentStepExists ():
###### Checks if the current step of the quest exists.
- Returns: True if the current step exists; otherwise, false.
```
public bool CurrentStepExists()
```

### - InstantiateCurrentQuestStep ():
###### Instantiates the current quest step as a game object.
Parameters: - parentTransform --> The transform where the quest step should be instantiated.
```
public void InstantiateCurrentQuestStep(Transform parentTransform)
```

### - GetCurrentQuestStepPrefab ():
###### Retrieves the prefab for the current quest step.
Returns: The quest step prefab for the current step, or null if it doesn't exist.
```
private GameObject GetCurrentQuestStepPrefab()
```

### - StoreQuestStepState ():
###### Stores the state of a quest step at a specified index.
Parameters: - questStepState --> The state of the quest step to be stored.
            - stepIndex --> The index of the quest step to store the state for.
```
public void StoreQuestStepState(QuestStepState questStepState, int stepIndex)
```
        
### - GetQuestData ():
###### Retrieves the data representing the current state and progress of the quest.
Returns: A QuestData object containing the quest's state and step states.
```
public QuestData GetQuestData()
```

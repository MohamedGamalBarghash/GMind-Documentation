# QuestStep Class
An abstract base class for implementing individual quest steps in a game.
```
private bool isFinished = false;
private int questId;
private int stepIndex;
```
## Functions:
### - InitializeQuestStep ():
###### Initializes the quest step with relevant information.
Parameters: - questId --> The unique identifier of the associated quest.
            - stepIndex --> The index of this quest step within the quest.
            - questStepState --> The initial state of the quest step (optional).
```
public void InitializeQuestStep(int questId, int stepIndex, string questStepState)
```

### - FinishQuestStep ():
###### Marks the quest step as finished and triggers quest progression.
```
protected void FinishQuestStep()
```

### - ChangeState() :
###### Notifies a change in the state of the quest step to the EventManager.
Parameters: newState --> The new state of the quest step.
```
protected void ChangeState(string newState)
```

### - SetQuestStepState ():
###### Sets the state of the quest step. Derived classes must implement this method.
Parameters: state --> The new state of the quest step.
```
protected abstract void SetQuestStepState(string state);
```

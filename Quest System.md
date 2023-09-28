### Quest Class
Represents a quest in a game, storing information about its state and progress.
```
    public QuestDataScriptableObject info;               // Static information about the quest.
    public QuestState state;                             // The current state of the quest.
    public int questId;                                  // The unique identifier for the quest.
    private int currentQuestStepIndex;                   // The index of the current quest step within the quest.
    private QuestStepState[] questStepStates;            // The states of individual quest steps.
```
### Constructors:
###### Initializes a new quest instance with the provided quest data created with the Quest class.
Parameters: - questInfo --> The static information about the quest.
```
    public Quest(QuestDataScriptableObject questInfo)
```

###### Initializes a new quest instance with saved state information.
Parameters: - questInfo --> The static information about the quest.
            - questState --> The saved state of the quest.
        /// <param name="currentQuestStepIndex">The index of the current quest step.</param>
        /// <param name="questStepStates">The saved states of individual quest steps.</param>
        public Quest(QuestDataScriptableObject questInfo, QuestState questState, int currentQuestStepIndex, QuestStepState[] questStepStates)
        {
            this.info = questInfo;
            this.state = questState;
            this.currentQuestStepIndex = currentQuestStepIndex;
            this.questStepStates = questStepStates;

            // Check for any inconsistencies between step prefabs and step states.
            if (this.questStepStates.Length != this.info.questStepPrefabs.Length)
            {
                Debug.LogWarning("Quest Step Prefabs and Quest Step States are "
                    + "of different lengths. This indicates something changed "
                    + "with the QuestInfo, and the saved data is now out of sync. "
                    + "Reset your data, as this might cause issues. QuestId: " + this.info.id);
            }
        }

        /// <summary>
        /// Moves to the next step of the quest.
        /// </summary>
        public void MoveToNextStep()
        {
            currentQuestStepIndex++;
        }

        /// <summary>
        /// Checks if the current step of the quest exists.
        /// </summary>
        /// <returns>True if the current step exists; otherwise, false.</returns>
        public bool CurrentStepExists()
        {
            return (currentQuestStepIndex < info.questStepPrefabs.Length);
        }

        /// <summary>
        /// Instantiates the current quest step as a game object.
        /// </summary>
        /// <param name="parentTransform">The transform where the quest step should be instantiated.</param>
        public void InstantiateCurrentQuestStep(Transform parentTransform)
        {
            GameObject questStepPrefab = GetCurrentQuestStepPrefab();
            if (questStepPrefab != null)
            {
                QuestStep questStep = Object.Instantiate<GameObject>(questStepPrefab, parentTransform)
                    .GetComponent<QuestStep>();
                questStep.InitializeQuestStep(info.id, currentQuestStepIndex, questStepStates[currentQuestStepIndex].state);
            }
        }

        /// <summary>
        /// Retrieves the prefab for the current quest step.
        /// </summary>
        /// <returns>The quest step prefab for the current step, or null if it doesn't exist.</returns>
        private GameObject GetCurrentQuestStepPrefab()
        {
            GameObject questStepPrefab = null;
            if (CurrentStepExists())
            {
                questStepPrefab = info.questStepPrefabs[currentQuestStepIndex];
            }
            else
            {
                Debug.LogWarning("Tried to get quest step prefab, but stepIndex was out of range indicating that "
                    + "there's no current step: QuestId=" + info.id + ", stepIndex=" + currentQuestStepIndex);
            }
            return questStepPrefab;
        }

        /// <summary>
        /// Stores the state of a quest step at a specified index.
        /// </summary>
        /// <param name="questStepState">The state of the quest step to be stored.</param>
        /// <param name="stepIndex">The index of the quest step to store the state for.</param>
        public void StoreQuestStepState(QuestStepState questStepState, int stepIndex)
        {
            if (stepIndex < questStepStates.Length)
            {
                questStepStates[stepIndex].state = questStepState.state;
            }
            else
            {
                Debug.LogWarning("Tried to access quest step data, but stepIndex was out of range: "
                    + "Quest Id = " + info.id + ", Step Index = " + stepIndex);
            }
        }

        /// <summary>
        /// Retrieves the data representing the current state and progress of the quest.
        /// </summary>
        /// <returns>A QuestData object containing the quest's state and step states.</returns>
        public QuestData GetQuestData()
        {
            return new QuestData(state, currentQuestStepIndex, questStepStates);
        }
    }
}
```
### Quest Data ScriptableObject
``` c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// A ScriptableObject class representing data for a game quest.
/// </summary>
[CreateAssetMenu(fileName = "New QuestData", menuName = "ScriptableObjects/QuestData")]
public class QuestDataScriptableObject : ScriptableObject
{
    /// <summary>
    /// Unique identifier for the quest.
    /// </summary>
    [field: SerializeField]
    public int id { get; private set; }

    /// <summary>
    /// The display name of the quest.
    /// </summary>
    [Header("General")]
    public string displayName;

    /// <summary>
    /// The description of the quest.
    /// </summary>
    public string description;

    /// <summary>
    /// The level requirement to start the quest.
    /// </summary>
    [Header("Requirements")]
    public int levelRequirement;

    /// <summary>
    /// Array of quest prerequisites required to unlock this quest.
    /// </summary>
    public QuestDataScriptableObject[] questPrerequisites;

    /// <summary>
    /// Array of quest step prefabs that get instantiated during the quest.
    /// </summary>
    [Header("Steps")]
    public GameObject[] questStepPrefabs;

    /// <summary>
    /// Array of rewards for completing the quest.
    /// </summary>
    [Header("Rewards")]
    public Reward[] questRewards;

    /// <summary>
    /// This method is called in the Unity Editor when the object is validated.
    /// It ensures that the id is set to the name of the scriptable object.
    /// </summary>
    private void OnValidate()
    {
        #if UNITY_EDITOR
        UnityEditor.EditorUtility.SetDirty(this);
        #endif
    }
}

```

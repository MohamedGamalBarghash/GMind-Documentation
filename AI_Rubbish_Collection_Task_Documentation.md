# Table of Contents
1. [Introduction](#introduction)
2. [Things made in Unity](#things-made-in-unity)
3. [Specifications](#specifications)
   1. [PickupType Class](#pickuptype-class)
   2. [ThrowRubbish Class](#throwrubbish-class)
   3. [RubbishContainer Class](#rubbishcontainer-class)

# Introduction:
This document outlines the functionality of three scripts that are part of an AI-related quest system in a Unity game and how they were used. The scripts manage different aspects of the quest system, including picking up objects, tracking quest progress, and managing interaction with specific objects.

## Things made in unity:
A scriptable object for the the activity was made in the folder ```/Resources/ThrowRubbish``` to handle the data for the simple activity and the rest of the scripts were added beside it for clarity of purpose.

## NOTE:
- Some modification to:
- ```\Scripts\AI Related\NPC\Quest System\QuestSystem\QuestStep.cs```,  
- ```\Scripts\AI Related\NPC\Movement and Interaction\DialogueSystem\DialogueManager.cs```,  
- ```\Scripts\AI Related\NPC\Movement and Interaction\NPCInteractable.cs```,  
- ```\Scripts\MovementController.cs```,  
- ```\Scripts\AI Related\NPC\Movement and Interaction\DialogueSystem\DialogueTrigger.cs```,  
- ```\Scripts\AI Related\NPC\Quest System\QuestSystem\QuestManager.cs``` and  
- ```\Scripts\AI Related\NPC\Movement and Interaction.cs```  
- were made.

## Specifications:

### PickupType Class

#### Description:
Responsible for handling the pickup behavior of different types of objects in the game.

#### Attributes:
- `type`: Represents the type of object being picked up.
- `questInfoForPoint`: Stores information about the quest associated with the pickup.

#### Methods:
- `OnPick()`: Called when the object is picked up. Starts the associated quest and performs necessary actions.

### ThrowRubbish Class

#### Description:
Manages the behavior of throwing rubbish in the game and tracks the progress of related quests.

#### Attributes:
- `rubbishType`: Enumerates the different types of rubbish in the game.
- `rubbishContainers`: Stores the game objects representing the rubbish containers.
- `completed`: Tracks the completion status of the rubbish disposal process.
- `inRange`: Tracks whether the player is in range to interact with the rubbish.

#### Methods:
- `UpdateState()`: Updates the state of the current quest based on the completion status.
- `Start()`: Initializes the rubbish containers and related components.
- `LateUpdate()`: Handles updates related to the state of rubbish disposal and quest progression.
- `SetQuestStepState()`: Updates the state of the current quest step.

### RubbishContainer Class

#### Description:
Detects the presence of the player within the range of rubbish containers in the game.

#### Attributes:
- `inRange`: Indicates whether the player is within the trigger range of the rubbish container.

#### Methods:
- `OnTriggerStay(Collider other)`: Triggers when another collider stays in the trigger, updating the player's presence.
- `OnTriggerExit(Collider other)`: Triggers when another collider exits the trigger, updating the player's absence.

Please refer to the individual sections below for detailed information on each script.
### PickupType.cs
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

namespace AI {
    public class PickupType : MonoBehaviour
    {
        public ThrowRubbish.RubbishType type;

        [Header("Quest options")]
        [SerializeField] private QuestDataScriptableObject questInfoForPoint;

        /// <summary>
        /// Called when the object is picked up.
        /// </summary>
        public void OnPick () {
            questInfoForPoint.questStepPrefabs[0].GetComponent<ThrowRubbish>().rubbishContainers = GameObject.FindGameObjectsWithTag(type.ToString()+"Rubbish");
            QuestsEventManager.instance.questEvents.StartQuest(questInfoForPoint.id);
        }
    }
}
```
### ThrowRubbish.cs
```
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

namespace AI {
    public class ThrowRubbish : QuestStep
    {
        public enum RubbishType {
            Plastic,
            Metal,
            Paper,
            Organic
        }
        public GameObject[] rubbishContainers;
        int completed = 0;
        public bool inRange = false;

        /// <summary>
        /// Updates the state of the quest.
        /// </summary>
        private void UpdateState()
        {
            ChangeState(completed.ToString());
        }

        // Start is called before the first frame update
        void Start()
        {  
            foreach (GameObject container in rubbishContainers) {
                container.transform.GetChild(0).gameObject.SetActive(true);
            }
        }

        // Update is called once per frame
        void LateUpdate()
        {
            foreach (GameObject container in rubbishContainers) {
                if (container.GetComponent<RubbishContainer>().inRange) {
                    inRange = true;
                    break;
                }
                inRange = false;
            }
            if (inRange) {
                if (Input.GetKeyDown(KeyCode.R)) {
                    foreach (GameObject container in rubbishContainers) {
                        container.transform.GetChild(0).gameObject.SetActive(false);
                    }
                    completed = 1;
                    FinishQuestStep();
                    CompletelyFinishQuest();
                }
            }
        }

        protected override void SetQuestStepState(string state)
        {
            UpdateState();
        }
    }
}
```
RubbishContainer.cs
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RubbishContainer : MonoBehaviour
{
    public bool inRange = false;

    /// <summary>
    /// Triggered while another collider stays in the trigger.
    /// </summary>
    void OnTriggerStay (Collider other) {
        if (other.gameObject.tag == "Player") {
            inRange = true;
        }
    }

    /// <summary>
    /// Triggered when another collider exits the trigger.
    /// </summary>
    void OnTriggerExit (Collider other) {
        if (other.gameObject.tag == "Player") {
            inRange = false;
        }
    }
}
```

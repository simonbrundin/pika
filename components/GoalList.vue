<template>
  <div id="goal-list">
    <p>
      <strong>
        <u>Min lista</u>
      </strong>
    </p>
    <draggable
      v-model="goals"
      group="people"
      @start="zeroSelection"
      @end="finishSelection"
      :move="changeSelection"
    >
      <ul v-for="(goal, index) in goals" :key="index">
        <li
          :class="{
            prio: goal.prio,
            done: goal.completed,
            'selected-goal': index === selectedGoal
          }"
          @click="checkGoal(goal, index)"
          v-if="!goal.editing"
        >{{ goal.title }}</li>
        <input v-focus v-if="goal.editing" class="goal-line-edit" type="text" v-model="goal.title" />
      </ul>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'goal-list',
  components: {
    draggable
  },

  data() {
    return {
      editingGoal: false,
      inputInFocus: false,
      goalInputText: '',

      selectedGoal: 0,
      goals: [
        {
          id: 1,
          title: 'Pussa på Emma',
          completed: false,
          editing: false,
          prio: false
        },
        {
          id: 2,
          title: 'Krama Emma',
          completed: false,
          editing: false,
          prio: false
        },
        {
          id: 3,
          title: 'Mata Emma',
          completed: false,
          editing: false,
          prio: false
        }
      ]
    }
  },
  directives: {
    focus: {
      // directive definition
      inserted: function(el) {
        el.focus()
      }
    }
  },
  mounted() {
    document.addEventListener('keydown', this.keyboardShortcuts)
  },

  methods: {
    zeroSelection: function() {
      this.selectedGoal = this.goals.length + 1

      draggable.drag = true
    },
    finishSelection: function() {
      draggable.drag = false
    },

    changeSelection: function(evt) {
      this.selectedGoal = evt.draggedContext.futureIndex
    },
    keyboardShortcuts(event) {
      // 1 - Om ett målnamn håller på att redigeras

      if (this.editingGoal) {
        // 1.1 - Avsluta redigering med Enter eller Caps Lock
        if (event.keyCode === 13 || event.keyCode === 27) {
          console.log('avslutat redigering')
          // Om inget fylls i -> ta bort målet
          if (this.goals[this.selectedGoal].title === '') {
            this.goals.splice(this.selectedGoal, 1)
            this.selectedGoal--
          }
          this.goals[this.selectedGoal].editing = false

          this.editingGoal = false
        }
      } else {
        const plats = this.selectedGoal - 1
        switch (event.keyCode) {
          // 2 - Navigera mellan målen
          case 73: // I - Navigera upp
            if (this.selectedGoal > 0) {
              this.selectedGoal--
            }

            break
          case 75: // K - Navigera ner
            if (this.selectedGoal < this.goals.length - 1) {
              this.selectedGoal++
            }

            break
          // Flytta mål
          case 69: // E - Flytta upp
            if (this.editingGoal) {
              console.log('gör inget')
            } else {
              if (this.selectedGoal > 0) {
                this.goals.splice(
                  plats,
                  2,
                  this.goals[this.selectedGoal],
                  this.goals[this.selectedGoal - 1]
                )
                this.selectedGoal--
              }
            }
            break
          case 68: // D - Flytta ner
            if (this.editingGoal) {
              console.log('gör inget')
            } else {
              if (this.selectedGoal < this.goals.length - 1) {
                this.goals.splice(
                  this.selectedGoal,
                  2,
                  this.goals[this.selectedGoal + 1],
                  this.goals[this.selectedGoal]
                )
                this.selectedGoal++
              }
            }

            break
          // 3 - Skapa nytt mål
          case 13: // Enter
            console.log('enter')
            this.editingGoal = true
            this.goals.splice(this.selectedGoal + 1, 0, {
              id: this.goals.length + 1,
              title: '',
              completed: false,
              editing: true,
              prio: false
            })
            this.selectedGoal++

            break
          // 4 Redigera text på valt mål
          case 27: // Caps Lock
            this.goals[this.selectedGoal].editing = true

            this.editingGoal = true

            break
          // 5 Ta bort mål
          case 32: // Space
            this.goals.splice(this.selectedGoal, 1)
            this.selectedGoal--

            break
          // Prioritera mål
          case 186: // Ö
            this.goals[this.selectedGoal].prio = !this.goals[this.selectedGoal]
              .prio

            break

          default:
            console.log(event.keyCode)
            break
        }
      }
    },
    addGoal() {
      if (this.goalInputText !== '') {
        let newID = this.goals.length + 1

        this.goals.push({
          id: newID,
          title: this.goalInputText,
          completed: false,
          editing: false,
          planned: false,
          startTime: '13.00 14/12/2019',
          priority: 0
        })
        this.goalInputText = ''
      } else {
        this.showInputBox = false
      }
    },
    selectGoal(goal) {
      this.selectedGoal = goal.id
    },
    checkGoal(goal, index) {
      this.selectedGoal = index
    },
    editGoal(goal) {
      console.log('test')

      goal.editing = true
    },
    doneEditingGoal(goal) {
      goal.editing = false
    }
  }
}
</script>

<style>
#goal-list {
  /* width: 800px; */
  text-align: center;
  margin: 100px auto;
  font-size: 30px;
  font-weight: 300;
}

.goal-input {
  width: 100%;
  padding: 20px 0;
  font-size: 30px;
  font-weight: 700;

  color: white;
  background: black;
  border: none;
  text-align: center;
}

.goal-line-edit {
  font-size: 30px;
  font-weight: 300;
  text-align: center;
  color: white;
  background: darkgrey;
  border: none;
}

.done {
  text-decoration: line-through;
}

.selected-goal {
  font-weight: 900;
  background: ;
}

h1 {
  color: white;
}

ul {
  list-style-type: none;
}
.prio {
  color: red;
}
</style>

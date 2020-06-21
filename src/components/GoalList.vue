<template>
  <div id="goal-list">
    <input
      v-if="showInputBox"
      type="text"
      class="goal-input"
      @keyup.enter="addGoal"
      @keydown="navigateGoals"
      @focus="inputInFocus = true"
      @blur="inputInFocus = false"
      v-model="goalInputText"
      placeholder="Skriv nytt mål"
    />
    <ul v-for="(goal, index) in goals" :key="index">
      <li
        :class="{
          done: goal.completed,
          'selected-goal': index === selectedGoal
        }"
        @click="checkGoal(goal, index)"
        @dblclick="editGoal(goal)"
        v-if="!goal.editing"
      >
        {{ goal.title }}
        <input
          v-if="goal.editing"
          class="goal-line-edit"
          v-focus
          type="text"
          v-model="goal.title"
          @keyup.enter="doneEditingGoal(goal)"
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'goal-list',

  data() {
    return {
      inputInFocus: false,
      goalInputText: '',
      showInputBox: false,
      selectedGoal: 0,
      goals: [
        {
          id: 1,
          title: 'Pussa på Emma',
          completed: false,
          editing: false,
          prio: true
        },
        {
          id: 2,
          title: 'Krama Emma',
          completed: false,
          editing: false,
          prio: true
        },
        {
          id: 3,
          title: 'Okej Emma',
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
    document.addEventListener('keydown', this.evaluateKeystroke)
  },

  methods: {
    evaluateKeystroke() {
      if (this.inputInFocus) {
        console.log(event.keyCode)
      } else {
        this.navigateGoals()
      }
    },
    navigateGoals() {
      const plats = this.selectedGoal - 1
      switch (event.keyCode) {
        case 38: // Upp
          if (this.selectedGoal > 0) {
            this.selectedGoal--
          }
          break
        case 40: // Ner
          if (this.selectedGoal < this.goals.length) {
            this.selectedGoal++
          }
          break
        case 9: // Tab
          this.goals[this.selectedGoal - 1].editing = true

          break
        case 32: // Space
          if (this.goals[this.selectedGoal].completed) {
            this.goals[this.selectedGoal].completed = false
          } else {
            this.goals[this.selectedGoal].completed = true
          }

          break
        case 69: // E
          if (this.selectedGoal > 0) {
            this.goals.splice(
              plats,
              2,
              this.goals[this.selectedGoal],
              this.goals[this.selectedGoal - 1]
            )
            this.selectedGoal--
          } else {
            console.log('e')
          }

          break
        case 68: // D
          if (this.selectedGoal < this.goals.length) {
            this.goals.splice(
              this.selectedGoal,
              2,
              this.goals[this.selectedGoal + 1],
              this.goals[this.selectedGoal]
            )
            this.selectedGoal++
          } else {
            console.log('d')
          }

          break
        case 78: // N
          console.log('ny')
          if (this.showInputBox) {
            this.showInputBox = false
          } else {
            this.showInputBox = true
          }
          this.showInputBox

          break

        default:
          console.log(event.keyCode)
          break
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
        alert('Fyll i ruta')
      }
    },
    selectGoal(goal) {
      this.selectedGoal = goal.id
    checkGoal(goal, index) {
      if (goal.completed) {
        goal.completed = false
      } else {
        goal.completed = true
      }
      this.selectedGoal = index
    },
    editGoal(goal) {
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
</style>

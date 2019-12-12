<template>
  <div id="goal-list">
    <input
      type="text"
      class="goal-input"
      @keyup.enter="addGoal"
      v-model="goalInputText"
      placeholder="Skriv nytt mål"
    />
    <div v-for="goal in goals" :key="goal.id">
      <div
        :class="{
          done: goal.completed,
          'selected-goal': goal.id === selectedGoal
        }"
        @click="checkGoal(goal)"
        @dblclick="editGoal(goal)"
        v-if="!goal.editing"
      >{{ goal.title }}</div>

      <input
        v-if="goal.editing"
        class="goal-line-edit"
        v-focus
        type="text"
        v-model="goal.title"
        @keyup.enter="doneEditingGoal(goal)"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'goal-list',

  data() {
    return {
      goalInputText: '',
      selectedGoal: 0,
      goals: [
        {
          id: 1,
          title: 'Pussa på Emma',
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: 'Krama Emma',
          completed: false,
          editing: false
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
    document.addEventListener('keydown', this.navigateGoals)
  },

  methods: {
    navigateGoals() {
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
          if (this.goals[this.selectedGoal - 1].completed) {
            this.goals[this.selectedGoal - 1].completed = false
          } else {
            this.goals[this.selectedGoal - 1].completed = true
          }

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
          editing: false
        })
        this.goalInputText = ''
      } else {
        alert('Fyll i ruta')
      }
    },
    checkGoal(goal) {
      if (goal.completed) {
        goal.completed = false
      } else {
        goal.completed = true
      }
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
  width: 800px;
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

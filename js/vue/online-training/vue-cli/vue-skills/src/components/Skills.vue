<template>
  <div class="hello">
    {{ name }}

    {{ btnState ? 'the button is disabled' : 'the button is active' }}

    <button v-bind:disabled="btnState">Change name</button>

    <div class="holder">
      <ul>
        <transition-group name="list">
          <li
            v-for="(data, index) in skills"
            :key="index+1">
            {{ index }} . {{ data.skill }}
            <p v-on:click="remove(index)">[X]</p>
          </li>
        </transition-group>
      </ul>

      <p v-if="skills.length >= 1" >You have more than 1 skills</p>
      <p v-else>You have less than or equal to 1 skill</p>
    </div>

    <div v-bind:class="{ alert: showAlert }"></div>

    <div v-bind:class="{ alert: showAlert, 'another-class': showClass }"></div>

    <div v-bind:class="alertObject"></div>

    <div v-bind:style="{ backgroundColor: bgColor, width: bgWidth, height: bgHeight }"></div>

    <hr>

    <p>These are the skills that you possess</p>

    <form @submit.prevent="addSkill">
      <input type="text" placeholder="enter a skills you have" v-model="skill" v-validate="'min:5'" name="skill">

      <transition name="alert-in" enter-active-class="animated flipInX" leave-active-class="animated flipOutX">
        <p class="alert" v-if="errors.has( 'skill' )">{{errors.first('skill')}}</p>
      </transition>

      <input type="checkbox" id="checkbox" v-model="checked">
    </form>
    {{ skill }}

  </div>
</template>

<script>
export default {
  name: 'Skills',
  data() {
    return {
      skills: [
        { "skill": "Vue.JS" },
        { "skill": "Front End Developer" }
      ],
      showAlert: true,
      showClass: true,
      name: 'Abakos',
      btnState: true,
      alertObject: {
        alert: true,
      },
      bgColor: 'green',
      bgWidth: '150px',
      bgHeight: '50px',
      skill: '',
      checked: false,
    }
  },
  methods: {
    addSkill() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.skills.push({skill: this.skill})
          this.skill = '';
          console.log(this.checked)
        } else {
          console.log('not valid')
        }
      })
    },
    remove(id) {
      this.skills.splice(id,1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Skills.css" scoped>

</style>

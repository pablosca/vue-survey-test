<template>
  <div class="welcome">
    <div class="greeting">
      <div class="contain">
        <h1 class="hey">Hello, {{ candidateName }}</h1>
        <p>Please read the instructions below before starting. <br> This assessment should take about 10-20 minutes.</p>
      </div>
    </div>

    <div class="intro contain">
      <h1 class="title">Personal Values Assessment</h1>
      <p>Here we briefly describe different people. Please read each description and think about how much that person is or is not like you.</p>
      <a @click="goToQuestionary" tabindex="0" class="button fluid primary">
        Get Started
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'welcome',
  props: ['candidate'],
  computed: {
    candidateName () {
      if (this.candidate) {
        return this.candidate.name;
      }
    }
  },
  methods: {
    goToQuestionary () {
      this.$bus.$emit('screen:change', { screen: 'Questionary' });
    }
  }
}
</script>

<template>
  <div class="repository">
    <h3>{{ organization }} / <a :href="repository.url">{{ repository.name }}</a></h3>
      <ul>
        <li v-for="pullRequest in repository.pullRequests.nodes" :key="pullRequest.url">
          <PullRequest :pullRequest="pullRequest" />
        </li>
      </ul>
  </div>
</template>

<script>
/* eslint-disable */
import PullRequest from '@/components/PullRequest.vue'

export default {
  name: 'Repository',

  components: {
    PullRequest
  },

  props: {
    organization: String,
    repository: Object
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

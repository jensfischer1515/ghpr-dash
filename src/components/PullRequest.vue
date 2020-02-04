<template>
  <div class="pullrequest">
    <h4><a :href="pullRequest.url" target="_new">{{ pullRequest.title }}</a></h4>
    <h5>#{{ pullRequest.number }} opened {{ pullRequest.publishedAt }} by {{ pullRequest.author.login }}</h5>
    <h5>updated {{ pullRequest.updatedAt }}</h5>
    <h6>
      <span class="commits">{{ pullRequest.commits.totalCount }} commits</span>&nbsp;
      <span class="files">changed {{ pullRequest.changedFiles }} files</span>&nbsp;
      <span class="additions">+{{ pullRequest.additions }}</span>&nbsp;
      <span class="deletions">-{{ pullRequest.deletions }}</span>
    </h6>
    <h6>from {{ pullRequest.headRef.repository.url }} :: {{ pullRequest.headRef.name }}</h6>
    <h6>into {{ pullRequest.baseRef.repository.url }} :: {{ pullRequest.baseRef.name }}</h6>
  </div>
</template>

<script>
export default {
  name: 'PullRequest',
  props: {
    pullRequest: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .additions {
    color: #28a745;
  }

  .deletions {
    color: #cb2431;
  }

  h5 {
    color: lightslategray
  }

  h6 {
    color: darkgray;
  }
</style>

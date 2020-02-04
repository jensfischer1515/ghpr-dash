<template>
  <div class="pullrequest">
    <h4>#{{ pullRequest.number }} <a :href="pullRequest.url" target="_new">{{ pullRequest.title }}</a></h4>
    <h5>opened {{ pullRequest.publishedAt | humanTime }} by {{ pullRequest.author.login }}</h5>
    <h5>updated {{ pullRequest.updatedAt | humanTime }}</h5>
    <h6>
      <span class="commits">{{ pullRequest.commits.totalCount }} commits</span>&nbsp;
      <span class="files">changed {{ pullRequest.changedFiles }} files</span>&nbsp;
      <span class="additions">+{{ pullRequest.additions }}</span>&nbsp;
      <span class="deletions">-{{ pullRequest.deletions }}</span>
    </h6>
    <h6>Source: <a :href="pullRequest.headRef.repository.url">{{ pullRequest.headRef.repository.url }}</a> :: {{ pullRequest.headRef.name }}</h6>
    <h6>Target: <a :href="pullRequest.baseRef.repository.url">{{ pullRequest.baseRef.repository.url }}</a> :: {{ pullRequest.baseRef.name }}</h6>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  name: 'PullRequest',

  props: {
    pullRequest: Object
  },

  filters: {
    humanTime: function (value) {
      if (!value) return '';
      const now = moment(new Date());
      const then = moment(value);
      const diff = moment.duration(then.diff(now));
      return diff.humanize(true);       
    }
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

<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>

<script>
/* eslint-disable */
import gql from "graphql-tag";

const pullRequestsQuery = gql`
  query PullRequests($organization: String!, $team: String!) {
    organization(login: $organization) {
      name
      team(slug: $team) {
        name
      }
    }
  }
`;

export default {
  name: "PullRequests",

  data: function() {
    return {
      organization: {},
      skipQuery: false,
    };
  },

  mounted: async function() {
    console.info(`==== mounted PullRequests @ ${this.$options.name}`);
  },

  methods: {
    fetchPullRequests: async function() {
      this.$apollo.queries.pullRequests.skip = false;
      await this.$apollo.queries.pullRequests.refetch();
    },  
  },

  apollo: {
    pullRequests: {
      query() {
        return pullRequestsQuery;
      },
      variables() {
        return {
          organization: process.env.VUE_APP_GITHUB_ORGANIZATION,
          team: process.env.VUE_APP_GITHUB_TEAM
        };
      },
      skip() {
        return this.skipQuery;
      },
      update(data) {
        console.info(
          `Skip GraphQL query? @ ${this.$options.name}`,
          this.skipQuery
        );
        if (this.skipQuery) {
          return;
        }
        this.organization = data.organization;
        this.skipQuery = true;
      }
    }
  },
}
</script>
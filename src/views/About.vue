<template>
  <div class="repositories">
    <h1>{{ team }}@{{ organization }} Repositories</h1>
      <ul>
        <li v-for="repository in repositories" :key="repository.url">
          <Repository :organization="organization" :repository="repository" />
        </li>
      </ul>
  </div>
</template>

<script>
/* eslint-disable */
import gql from "graphql-tag";
import Repository from '@/components/Repository.vue'

const pullRequestsQuery = gql`
  query PullRequests(
      $organizationLogin: String!,
      $teamSlug: String!,
      $numberOfRepositories: Int = 100,
      $numberOfPullRequests: Int = 25,
      $numberOfAssignees: Int = 10,
      $numberOfLabels: Int = 10
    ) {
    organization(login: $organizationLogin) {
      name
      team(slug: $teamSlug) {
        name
        repositories(first: $numberOfRepositories, orderBy: {field: NAME, direction: ASC}) {
          nodes {
            name
            url
            primaryLanguage {
              name
            }
            isFork
            isPrivate
            isArchived
            isDisabled
            isLocked
            isTemplate
            isMirror
            mergeCommitAllowed
            rebaseMergeAllowed
            squashMergeAllowed
            deleteBranchOnMerge
            pullRequests(states: OPEN, first: $numberOfPullRequests, orderBy: {field: UPDATED_AT, direction: DESC}) {
              nodes {
                url
                number
                title
                #bodyHTML
                state
                closed
                merged
                locked
                lastEditedAt
                publishedAt
                updatedAt
                mergeable
                baseRef {
                  name
                  repository {
                    url
                  }
                }
                headRef {
                  name
                  repository {
                    url
                  }
                }
                assignees(first: $numberOfAssignees) {
                  nodes {
                    login
                  }
                }
                author {
                  login
                }
                labels(first: $numberOfLabels) {
                  nodes {
                    name
                  }
                }
                changedFiles
                additions
                deletions
                commits {
                  totalCount
                }
              }
            }
          }
        }
      }
    }
  }
`;

export default {
  name: "Repositories",

  components: {
    Repository
  },

  data: function() {
    return {
      organization: null,
      team: null,
      repositories: [],
    };
  },

  mounted: async function() {
    console.info(`==== mounted PullRequests @ ${this.$options.name}`);
  },

  methods: {
    fetchPullRequests: async function() {
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
          organizationLogin: process.env.VUE_APP_GITHUB_ORGANIZATION,
          teamSlug: process.env.VUE_APP_GITHUB_TEAM
        };
      },
      skip() {
        return false;
      },
      update(data) {
        this.organization = data.organization.name;
        this.team = data.organization.team.name;
        this.repositories = data.organization.team.repositories.nodes.filter(function (el) {
          let hasOpenPullRequests = el.pullRequests.nodes.some(function (el){
            return el.closed == false;
          })
          return hasOpenPullRequests;
        });
      }
    }
  },
}
</script>

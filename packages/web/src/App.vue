<template>
  <div id="root">
    <Modals />
    <!-- <PageLoader v-if="!isAppReady" /> -->
    <div id="app-frame">
      <router-view></router-view>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/no-explicit-any */
import { defineComponent, ref, computed } from 'vue'
import Navigation from '@/components/Navigation/Navigation.vue'
import PageLoader from '@/components/Loader.vue'
import ErrorPage from '@/components/ErrorPage.vue'
import Modals from '@/components/Modals/Modals.vue'
import { useQuery } from '@vue/apollo-composable'
import { getProjectWithUsersAndIssues } from '@/graphql/queries/project'
import { mutations, getters } from './store'

const data = {
    "getProjectWithUsersAndIssues": {
      "id": "553",
      "name": "INVMON",
      "url": "https://www.atlassian.com/software/jira",
      "description": "The robbery of the Royal Mint of Spain",
      "category": "marketing",
      "createdAt": "2023-08-04T08:45:15.118Z",
      "updatedAt": "2023-08-04T08:45:15.118Z",
      "users": [
        {
          "id": "b3747c93-9ffc-413d-9052-c68380a7a678",
          "name": "Denver",
          "avatarUrl": "https://res.cloudinary.com/datlyfe/image/upload/v1583949061/casa%20del%20papel/denver_wntrkk.jpg",
          "projectId": 553,
          "__typename": "User"
        },
        {
          "id": "3055843c-446c-499a-b044-fbae1de27994",
          "name": "Berlin",
          "avatarUrl": "https://res.cloudinary.com/datlyfe/image/upload/v1583949061/casa%20del%20papel/berlin_tjeb95.jpg",
          "projectId": 553,
          "__typename": "User"
        },
        {
          "id": "a1e3b95b-27d9-4f3d-8e1f-92a13bbdcd6d",
          "name": "Tokyo",
          "avatarUrl": "https://res.cloudinary.com/datlyfe/image/upload/v1583949061/casa%20del%20papel/tokyo_eiij3f.jpg",
          "projectId": 553,
          "__typename": "User"
        },
        {
          "id": "06b21c64-56b0-4f53-95c5-c11a62484030",
          "name": "El Profesor",
          "avatarUrl": "https://res.cloudinary.com/datlyfe/image/upload/v1583949197/casa%20del%20papel/profesor_dcwdlt.jpg",
          "projectId": 553,
          "__typename": "User"
        }
      ],
      "issues": [
        {
          "id": "3919",
          "title": "Couple hostages tried to escape",
          "description": "<h3>The Bastards almost killed me 😠😠</h3><p><br></p><p>Arturo tried escaping with the help of his secretary Mónica Gaztambide</p>",
          "type": "bug",
          "status": "inprogress",
          "priority": "5",
          "listPosition": 4,
          "createdAt": "2023-08-04T08:45:15.341Z",
          "updatedAt": "2023-08-04T08:45:15.341Z",
          "userIds": [
            "3055843c-446c-499a-b044-fbae1de27994",
            "b3747c93-9ffc-413d-9052-c68380a7a678"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3925",
          "title": "Each issue has a single reporter but can have multiple assignees.",
          "description": "<h2>Try assigning <u style=\"background-color: #ff7979;\">Denver</u> to this issue. <span style=\"color: rgb(51, 51, 51);\">🤣&nbsp;🤣&nbsp;🤣</span></h2><p><br></p>",
          "type": "story",
          "status": "inprogress",
          "priority": "4",
          "listPosition": 1,
          "createdAt": "2023-08-04T08:45:15.486Z",
          "updatedAt": "2023-08-04T09:06:17.107Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030",
            "a1e3b95b-27d9-4f3d-8e1f-92a13bbdcd6d"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3921",
          "title": "Print money for a total of 2.4 billion Euros",
          "description": "<h2>💰💰💰 We are making our own money <strong>💰💰💰</strong></h2><p>The plan is not just to rob the bank no no no..., we are going to take the employees hostage and make them print 2.4 billion Euros</p>",
          "type": "task",
          "status": "inprogress",
          "priority": "3",
          "listPosition": 3,
          "createdAt": "2023-08-04T08:45:15.342Z",
          "updatedAt": "2023-08-04T08:45:15.342Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030",
            "a1e3b95b-27d9-4f3d-8e1f-92a13bbdcd6d",
            "b3747c93-9ffc-413d-9052-c68380a7a678"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3924",
          "title": "Try leaving a comment on this issue.",
          "description": "<p>Adding comments to an issue is a useful way to record additional detail about an issue, and collaborate with team members. Comments are shown in the&nbsp;<strong>Comments</strong>&nbsp;section when you&nbsp;<a href=\"https://confluence.atlassian.com/jira064/what-is-an-issue-720416138.html\" rel=\"noopener noreferrer\" target=\"_blank\" style=\"background-color: rgb(255, 255, 255); color: rgb(0, 82, 204);\">view an issue</a>.</p><p><br></p><ol><li>Open the&nbsp;<a href=\"https://confluence.atlassian.com/jira064/what-is-an-issue-720416138.html\" rel=\"noopener noreferrer\" target=\"_blank\" style=\"color: rgb(0, 82, 204);\">issue</a>&nbsp;on which to add your comment.</li><li>Click the&nbsp;<strong>Add a comment</strong>&nbsp;button.</li><li>In the&nbsp;<strong>Comment</strong>&nbsp;text box, type your comment</li><li>Click the&nbsp;<strong>Save </strong>button or the <strong>Enter </strong>key to save the comment.</li></ol><p><br></p>",
          "type": "task",
          "status": "done",
          "priority": "3",
          "listPosition": 7,
          "createdAt": "2023-08-04T08:45:15.483Z",
          "updatedAt": "2023-08-04T08:45:15.483Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3920",
          "title": "This is our escape plan",
          "description": "<h1>⛏️⛏️ We are digging our way out of the bank <strong>⛏️⛏️</strong></h1><p><br></p><p>Moscow and Denver will take care of digging a tunnel into the safe they are the best in the business.</p>",
          "type": "task",
          "status": "inprogress",
          "priority": "2",
          "listPosition": 2,
          "createdAt": "2023-08-04T08:45:15.342Z",
          "updatedAt": "2023-08-04T08:45:15.342Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030",
            "b3747c93-9ffc-413d-9052-c68380a7a678"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3922",
          "title": "The rules and guidlines of the heist",
          "description": "<h3>These rules <strong>MUST</strong> be followed :</h3><p><br></p><ul><li><strong>No Killing:</strong> We want to appear to be Just Like Robin Hood, we do not intend to hurt anyone</li><li><strong>No Names:</strong> Everyone is nicknamed after a city. do not share you real names with anyone even me</li><li><strong>No Personal Relationships:</strong> this is good for preventing attachments that might compromise the operation</li></ul><p><br></p><h3><u style=\"background-color: initial;\">That's it!</u></h3><h2>💯💯</h2><p><br></p>",
          "type": "story",
          "status": "selected",
          "priority": "4",
          "listPosition": 5,
          "createdAt": "2023-08-04T08:45:15.344Z",
          "updatedAt": "2023-08-04T09:06:15.103Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030",
            "3055843c-446c-499a-b044-fbae1de27994",
            "a1e3b95b-27d9-4f3d-8e1f-92a13bbdcd6d",
            "b3747c93-9ffc-413d-9052-c68380a7a678"
          ],
          "__typename": "Issue"
        },
        {
          "id": "3923",
          "title": "We are the resistance",
          "description": "<h2>They left us no choice but to come and take what's rightfully ours ✊✊✊</h2><p><br></p><p>In this world, everything is governed by balance. There’s what you stand to gain and what you stand to lose. And when you think you’ve got nothing to lose, you become over confident.</p>",
          "type": "story",
          "status": "backlog",
          "priority": "2",
          "listPosition": 5,
          "createdAt": "2023-08-04T08:45:15.431Z",
          "updatedAt": "2023-08-04T08:45:15.431Z",
          "userIds": [
            "06b21c64-56b0-4f53-95c5-c11a62484030",
            "3055843c-446c-499a-b044-fbae1de27994",
            "b3747c93-9ffc-413d-9052-c68380a7a678"
          ],
          "__typename": "Issue"
        }
      ],
      "__typename": "Project"
    }
  }

export default defineComponent({
  components: {
    Navigation,
    PageLoader,
    ErrorPage,
    Modals
  },
  setup() {

    console.log(data)

    const expanded = ref<boolean>(true)
    const handleNavigationResize = (isExpanded: boolean) => {
      expanded.value = isExpanded
    }

    // const isAppReady = computed(
    //   () =>
    //     getters.isAuthenticated() &&
    //     Object.keys(getters.currentUser()).length !== 0
    // )

    const getContentStyles = computed(() => ({
      'padding-left': `${expanded.value ? 240 : 20}` + 'px',
      'margin-left': '55px'
    }))

    const match = window.matchMedia('(max-width: 1100px)')
    const matchHandler = (e: MediaQueryListEventInit) =>
      (expanded.value = !e.matches)

    matchHandler(match)
    match.addListener(matchHandler)

    // const { loading, onResult, error } = useQuery(
    //   getProjectWithUsersAndIssues,
    //   {},
    //   { fetchPolicy: 'no-cache' }
    // )

    // onResult(res => {
    //   const { data } = res as any
    //   if (data) {
    //     mutations.setProject(data)
    //   }
    // })
    
    mutations.setProject(data.getProjectWithUsersAndIssues)

    return {
      //isAppReady,
      //loading,
      //error,
      expanded,
      handleNavigationResize,
      getContentStyles
    }
  }
})
</script>

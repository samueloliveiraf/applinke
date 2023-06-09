<template>
  <q-page class="">
    <CampaignConf @userCreated="setUserId" />
    <CampaignUser :userId="userId" />
  </q-page>
</template>

<script>
/* eslint-disable no-trailing-spaces, space-before-function-paren, eol-last, semi, vue/no-unused-components */
import { defineComponent, ref } from 'vue'
import CampaignConf from './CampaignConf.vue';
import CampaignUser from './CampaignUser.vue';

export default defineComponent({
  name: 'IndexPage',
  components: {
    CampaignConf,
    CampaignUser
  },
  setup() {
    const userId = ref(null);

    function setUserId(id) {
      userId.value = id;
    }

    return {
      userId,
      setUserId
    }
  }
})
</script>

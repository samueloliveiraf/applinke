<template>
  <q-page class="">
    <CampaignConf />
    <CampaignUser />
  </q-page>
</template>

<script>
/* eslint-disable no-trailing-spaces, space-before-function-paren, eol-last, semi, vue/no-unused-components */
import { defineComponent } from 'vue'
import CampaignConf from './CampaignConf.vue';
import CampaignUser from './CampaignUser.vue';

export default defineComponent({
  name: 'IndexPage',
  components: {
    CampaignConf,
    CampaignUser
  }
})
</script>

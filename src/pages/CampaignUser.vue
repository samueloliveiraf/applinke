<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>Campanha nome</th>
          <th>Criada</th>
          <th>Is Active</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="campaign in campaigns" :key="campaign.id" @click="showCampaign(campaign)">
          <td>{{ campaign.name_campaign }}</td>
          <td>{{ formatDate(campaign.created_at) }}</td>
          <td>{{ campaign.is_active }}</td>
        </tr>
      </tbody>
    </table>
    <div v-if="selectedCampaign" class="list-info">
      <h2>{{ selectedCampaign.name_campaign }}</h2>
      <p>Criada: {{ formatDate(selectedCampaign.created_at) }}</p>
      <p>Departamento primario: {{ selectedCampaign.primary_departament }}</p>
      <p>Departamento secondário: {{ selectedCampaign.secondary_departament }}</p>
      <p>Dias de envio: {{ selectedCampaign.sending_days }}</p>
      <p>Copy primaria: {{ selectedCampaign.primary_copies }}</p>
      <p>Copy secondaria: {{ selectedCampaign.secondary_copies }}</p>
      <p>Horários de envios: {{ selectedCampaign.sending_time }}</p>
    </div>
  </div>
</template>

<script>
import { format } from 'date-fns'
export default {
  name: 'CampaignUser',
  data () {
    return {
      campaigns: [],
      selectedCampaign: null
    }
  },
  computed: {
    userId () {
      return this.$route.params.id
    }
  },
  mounted () {
    this.fetchCampaigns()
  },
  methods: {
    fetchCampaigns () {
      fetch(`http://localhost:8000/campaigns/user/${this.userId}/`)
        .then(response => response.json())
        .then(data => {
          this.campaigns = data
        })
        .catch(error => {
          console.error('Error:', error)
        })
    },
    showCampaign (campaign) {
      this.selectedCampaign = campaign
    },
    formatDate (date) {
      return format(new Date(date), 'yyyy-MM-dd HH:mm:ss')
    }
  }
}
</script>

<style scoped>
table {
    width: 100%;
    margin-top: 3rem;
    border-collapse: collapse;
}

th,
td {
    padding: 8px;
    border: 1px solid #ccc;
}

th {
    background-color: #f2f2f2;
}

tr:hover {
    background-color: #f5f5f5;
    cursor: pointer;
}

.list-info {
    background-color: #f2f2f2;
    border-radius: 3rem;
    padding: 0.8rem;
    margin-top: 1.5rem;
}

.scroll-button {
    margin-top: 1rem;
}
</style>

<template>
  <q-btn label="Configurar campanha" @click="showDialog = true" />

  <q-dialog v-model="showDialog" full-width>
    <q-card style="width: 70vw !important">
      <q-card-section class="q-card-section">
        <div class="text-h6">Criar campanha do linkedin</div>
      </q-card-section>
      <q-card-section>
        <q-tabs v-model="currentStep" align="justify">
          <q-tab name="1" label="Passo 1" />
          <q-tab name="2" label="Passo 2" />
          <q-tab name="3" label="Passo 3" />
          <q-tab name="4" label="Passo 4" />
          <q-tab name="5" label="Passo 5" />
        </q-tabs>

        <q-tab-panels v-model="currentStep" animated>
          <!-- Step 1 -->
          <q-tab-panel name="1">
            <q-input v-model="nameCampaign" label="Nome da campanha" />
            <div class="q-mb-md">Dias permitidos para envio</div>
            <q-checkbox v-model="days['Segunda-feira']" label="Seg" />
            <q-checkbox v-model="days['Terça-feira']" label="Ter" />
            <q-checkbox v-model="days['Quarta-feira']" label="Qua" />
            <q-checkbox v-model="days['Quinta-feira']" label="Qui" />
            <q-checkbox v-model="days['Sexta-feira']" label="Sex" />
            <q-checkbox v-model="days['Sábado']" label="Sáb" />
            <q-checkbox v-model="days['Domingo']" label="Dom" />

            <div class="q-mt-md">Horário de disparo</div>
            <q-input v-model="sendTime.start" type="time" mask="time" />
            às
            <q-input v-model="sendTime.end" type="time" mask="time" />

            <div class="q-mt-md">Tempo mínimo antes de reenviar da mensagem ao mesmo contato</div>
            <q-select v-model="resendInterval" :options="[2, 3, 4, 5]" suffix="dias" />

            <q-btn class="q-mt-md" @click="currentStep = '2'">Próximo</q-btn>
          </q-tab-panel>

          <!-- Step 2 -->
          <q-tab-panel name="2">
            <div class="q-mb-md">Com qual departamento você deseja falar?</div>
            <q-select v-model="primaryDept" label="Primário"
              :options="['Qualquer um', 'Financeiro', 'Operacional', 'Administrativo', 'TI', 'Marketing', 'Gestão', 'Comercial', 'Indústrial', 'Jurídico', 'Proprietários', 'Recursos Humanos', 'Suprimentos']" />
            <q-select v-model="secondaryDept" label="Secundário (opcional)"
              :options="['Qualquer um', 'Financeiro', 'Operacional', 'Administrativo', 'TI', 'Marketing', 'Gestão', 'Comercial', 'Indústrial', 'Jurídico', 'Proprietários', 'Recursos Humanos', 'Suprimentos']" />

            <q-checkbox v-model="authorizeOtherDepts"
              label="Autorizo o disparo para outros departamentos caso não seja possível a primeira nem segunda opção." />

            <div class="q-mt-md">Qual o nível do cargo?</div>
            <q-radio v-model="jobLevel" val="Alto" label="Alto" />
            <q-radio v-model="jobLevel" val="Médio" label="Médio" />
            <q-radio v-model="jobLevel" val="Baixo" label="Baixo" />
            <q-radio v-model="jobLevel" val="Não classificado" label="Não classificado" />

            <q-btn @click="currentStep = '1'">Voltar</q-btn>
            <q-btn @click="currentStep = '3'">Próximo</q-btn>
          </q-tab-panel>

          <!-- Step 3 -->
          <q-tab-panel name="3">
            <div class="q-mb-md">Selecione até três listas para o envio dos e-mails das campanhas ({{ selectedLists.length
            }} selecionado)</div>
            <q-input v-model="searchTerm" label="Buscar pelo nome da lista" />

            <q-select v-model="selectedLists" multiple label="Selecione listas" :options="filteredLists"
              option-value="name" option-label="label" use-input use-chips />
            <q-btn @click="currentStep = '2'">Voltar</q-btn>
            <q-btn @click="currentStep = '4'">Próximo</q-btn>
          </q-tab-panel>

          <!-- Step 4 -->
          <q-tab-panel name="4">
            <div class="q-mb-md">Personalize os textos de acordo com o seu negócio</div>
            <div class="q-mb-md">
              <div>Jornada principal</div>
              <q-select v-model="mainJourneyCount" :options="[1, 2, 3, 4, 5]" label="Escolha a quantidade de textos"
                @input="primaryCopies.length = mainJourneyCount" />
              <q-input v-for="(text, index) in primaryCopies" :key="index" v-model="primaryCopies[index].copy"
                label="Texto" />
            </div>

            <div class="q-mb-md">
              <div>Jornada alternativa</div>
              <q-select v-model="altJourneyCount" :options="[1, 2, 3]" label="Escolha a quantidade de textos"
                @input="secondaryCopies.length = altJourneyCount" />
              <q-input v-for="(text, index) in secondaryCopies" :key="index" v-model="secondaryCopies[index].copy"
                label="Texto" />
            </div>
            <q-btn @click="currentStep = '3'">Voltar</q-btn>
            <q-btn @click="currentStep = '5'">Próximo</q-btn>
          </q-tab-panel>

          <!-- Step 5 -->
          <q-tab-panel name="5">
            <div class="q-mb-md">Defina suas credenciais do LinkedIn</div>

            <q-input v-model="linkedInEmail" label="E-mail do LinkedIn" />
            <q-input v-model="linkedInPassword" label="Senha do LinkedIn" :type="showPassword ? 'text' : 'password'">
              <template v-slot:append>
                <q-icon name="visibility" class="cursor-pointer" @click="showPassword = !showPassword" />
              </template>
            </q-input>
            <q-btn class="q-mt-md" @click="currentStep = '4'">Voltar</q-btn>
            <q-btn class="q-mt-md" @click="submitForm">Concluir</q-btn>
          </q-tab-panel>
        </q-tab-panels>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
/* eslint-disable no-trailing-spaces, space-before-function-paren, eol-last, semi, vue/no-unused-components */
import { ref, computed } from 'vue'

import { useQuasar } from 'quasar'
import axios from 'axios'

export default {
  name: 'CampaignConf',
  setup() {
    const $q = useQuasar()

    const nameCampaign = ref('')
    const infosetId = ref('as]ASKDASKD')
    const showDialog = ref(false)
    const currentStep = ref('1')
    const days = ref({
      'Segunda-feira': false,
      'Terça-feira': false,
      'Quarta-feira': false,
      'Quinta-feira': false,
      'Sexta-feira': false,
      Sábado: false,
      Domingo: false
    })
    const sendingDays = computed(() => {
      return {
        'Segunda-feira': days.value['Segunda-feira'] ? 1 : 0,
        'Terça-feira': days.value['Terça-feira'] ? 2 : 0,
        'Quarta-feira': days.value['Quarta-feira'] ? 3 : 0,
        'Quinta-feira': days.value['Quinta-feira'] ? 4 : 0,
        'Sexta-feira': days.value['Sexta-feira'] ? 5 : 0,
        Sábado: days.value.Sábado ? 6 : 0,
        Domingo: days.value.Domingo ? 7 : 0
      }
    })
    const sendTime = ref({
      start: '09:00',
      end: '18:00'
    })
    const resendInterval = ref(2)
    const primaryDept = ref('')
    const secondaryDept = ref('')
    const authorizeOtherDepts = ref(false)
    const jobLevel = ref('')

    const lists = ref([
      { name: 'Energia Solar', companies: 7 },
      { name: '400k+', companies: 19 },
      { name: 'Contabilidade', companies: 1 }
    ])
    const selectedLists = ref([])
    const searchTerm = ref('')

    const filteredLists = computed(() => {
      if (searchTerm.value === '') {
        return lists.value.map(list => ({ value: list.name, label: `${list.name} (${list.companies} empresas)` }))
      } else {
        return lists.value.filter(list => list.name.toLowerCase().includes(searchTerm.value.toLowerCase())).map(list => ({ value: list.name, label: `${list.name} (${list.companies} empresas)` }))
      }
    })

    const mainJourneyCount = ref(5)
    const altJourneyCount = ref(3)
    const primaryCopies = ref(Array.from({ length: mainJourneyCount.value }, (_, i) => ({ [`copy${i + 1}`]: '' })))
    const secondaryCopies = ref(Array.from({ length: altJourneyCount.value }, (_, i) => ({ [`copy${i + 1}`]: '' })))

    const linkedInEmail = ref('')
    const linkedInPassword = ref('')
    const showPassword = ref(false)

    async function submitForm() {
      try {
        const response = await axios.post('http://localhost:8000/campaigns/create/', {
          name_campaign: nameCampaign.value,
          target_levels: jobLevel.value,
          primary_departament: primaryDept.value,
          secondary_departament: secondaryDept.value,
          primary_copies: primaryCopies.value,
          sending_days: sendingDays.value,
          secondary_copies: secondaryCopies.value,
          infoset_id: searchTerm.value,
          sending_interval: resendInterval.value,
          sending_time: {
            start: sendTime.value.start,
            end: sendTime.value.end
          },
          user: 1
        })

        if (response.status === 201) {
          $q.notify({
            message: 'Sucesso ao cadastrar campanha.',
            type: 'positive'
          })
          window.location.href = '/campaign/user/' + response.data.user
        }
      } catch (error) {
        if (error.response) {
          if (error.response.data.validation_errors) {
            const errorData = error.response.data
            console.log(errorData.validation_errors)
          }
          console.log(error.response.status)
        } else if (error.request) {
          console.log(error.request)
        } else {
          console.log('Error', error.message)
        }
      }
    }

    return {
      nameCampaign,
      infosetId,
      showDialog,
      currentStep,
      days,
      sendTime,
      resendInterval,
      primaryDept,
      secondaryDept,
      authorizeOtherDepts,
      jobLevel,
      lists,
      selectedLists,
      searchTerm,
      filteredLists,
      mainJourneyCount,
      primaryCopies,
      altJourneyCount,
      secondaryCopies,
      linkedInEmail,
      linkedInPassword,
      showPassword,
      submitForm
    }
  }
}
</script>

<style scoped>
q-card-section:nth-of-type(2) {
  margin: 0;
}
</style>
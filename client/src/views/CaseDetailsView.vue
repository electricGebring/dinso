<script setup lang="ts">
import { computed, onMounted, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import customer from '@customer/config'
import { activeLocale, translate } from '../i18n'
import { useDemoSessionStore } from '../stores/demoSession'
import { useDemoPortalStore } from '../stores/demoPortal'
import CaseDetails from '../components/CaseDetails.vue'
import DemoNotice from '../components/DemoNotice.vue'

const route = useRoute()
const router = useRouter()
const session = useDemoSessionStore()
const portal = useDemoPortalStore()
const t = (source: string, values?: Record<string, string | number>): string =>
  translate(activeLocale.value, source, values)

const caseItem = computed(() =>
  portal.cases.find((item) => item.id === route.params.caseId) ?? null,
)
const labels = computed(() => ({
  eyebrow: t('Ärende'),
  close: t('Tillbaka till ärenden'),
  status: t('Status'),
  due: t('Förfaller'),
  details: t('Detaljer'),
  approve: t('Godkänn ärende'),
  approved: t('Godkänd'),
  readOnly: t('Du har läsbehörighet och kan inte godkänna ärenden.'),
}))

const close = (): void => {
  void router.push({ name: 'company-cases' })
}
const approve = async (): Promise<void> => {
  if (caseItem.value) await portal.approveCase(caseItem.value)
}

watch(
  () => route.params.caseId,
  () => portal.clearCaseError(),
)

onMounted(() => {
  portal.clearCaseError()
  if (customer.key !== 'svenskebanken') {
    void router.replace({ name: 'company-cases' })
    return
  }
  void portal.loadCompanyData()
})
</script>

<template>
  <main class="case-details-page">
    <CaseDetails
      v-if="caseItem"
      :item="caseItem"
      :can-approve="session.canApproveCases"
      :error="portal.caseError"
      :labels="labels"
      @close="close"
      @approve="approve"
    />
    <DemoNotice v-else role="alert">
      {{ t('Ärendet kunde inte hittas.') }}
    </DemoNotice>
  </main>
</template>

<style scoped>
.case-details-page {
  max-width: 720px;
  margin: 0 auto;
  padding: 24px;
  background: var(--surface);
  border: var(--panel-border, 1px solid var(--border));
  border-radius: var(--radius-panel);
  box-shadow: var(--shadow-panel, none);
}

@media (max-width: 520px) {
  .case-details-page {
    margin: 12px;
    padding: 16px;
  }
}
</style>

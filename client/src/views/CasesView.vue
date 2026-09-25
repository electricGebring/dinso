<script setup lang="ts">
import { computed, ref } from 'vue'
import DataTable, { type DataTableColumn } from '../components/DataTable.vue'
import EmptyState from '../components/EmptyState.vue'
import PageHeader from '../components/PageHeader.vue'
import Panel from '../components/Panel.vue'
import StatusBadge from '../components/StatusBadge.vue'
type Case = { id: string; name: string; status: string; value: string; detail: string }
type StatusTone = 'positive' | 'pending' | 'attention' | 'neutral'
const props = defineProps<{
  title: string
  cases: Case[]
  t: (source: string) => string
  statusTone: (status: string) => StatusTone
}>()
const emit = defineEmits<{ open: [item: Case] }>()
const searchQuery = ref('')

const filteredCases = computed(() => {
  const query = searchQuery.value.trim().toLocaleLowerCase()
  if (!query) return props.cases

  return props.cases.filter((item) =>
    `${item.name} ${item.detail}`.toLocaleLowerCase().includes(query),
  )
})

const columns = computed<DataTableColumn[]>(() => [
  { key: 'name', label: props.t('Ärende') },
  { key: 'status', label: props.t('Status') },
  { key: 'value', label: props.t('Förfaller'), align: 'right' },
])
</script>
<template>
  <section>
    <PageHeader
      class="cases-page-header"
      :title="`${title} banan`"
      :description="
        t('Klicka på ett ärende för att se detaljer och godkänna det.')
      "
    />
    <div class="cases-layout">
      <div>
        <Panel>
          <DataTable
            v-if="filteredCases.length > 0"
            :columns="columns"
            :rows="filteredCases"
          >
            <template #cell-name="{ row }">
              <button
                class="table-link"
                type="button"
                @click="emit('open', row as Case)"
              >
                {{ t((row as Case).name) }}
              </button>
            </template>
            <template #cell-status="{ row }">
              <StatusBadge
                :label="t((row as Case).status)"
                :tone="statusTone((row as Case).status)"
              />
            </template>
          </DataTable>
          <EmptyState
            v-else
            :title="searchQuery ? t('Inga träffar') : t('Inga ärenden')"
            :description="
              searchQuery
                ? t('Prova ett annat sökord.')
                : t('Det finns inga ärenden att visa just nu.')
            "
          />
        </Panel>
      </div>

      <div class="cases-sidebar">
        <div class="cases-search">
          <div class="cases-search__header">
            <label class="cases-search__label" for="case-search">
              {{ t('Sök ärenden') }}
            </label>
            <span
              v-if="searchQuery.trim()"
              class="cases-search__count"
              aria-live="polite"
            >
              {{ filteredCases.length }}
              {{ t(filteredCases.length === 1 ? 'träff' : 'träffar') }}
            </span>
          </div>
          <input
            id="case-search"
            class="cases-search__input"
            type="search"
            v-model="searchQuery"
            :placeholder="t('Sök på ärende eller person')"
          />
        </div>

        <Panel title="Filter">
          <div class="cases-filter">
          <div class="cases-filter__heading">
            <span>{{ t('3 filter används') }}</span>
            <button class="cases-filter__clear" type="button">
              {{ t('Rensa filter') }}
            </button>
          </div>

          <fieldset class="cases-filter__group">
            <legend>{{ t('Förfallostatus') }}</legend>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Förfaller') }}</span>
            </label>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Förfaller snart') }}</span>
            </label>
          </fieldset>

          <fieldset class="cases-filter__group">
            <legend>{{ t('Ärendestatus') }}</legend>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Godkänd') }}</span>
            </label>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Genomförd') }}</span>
            </label>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Pågående') }}</span>
            </label>
            <label class="cases-filter__check">
              <input type="checkbox" />
              <span>{{ t('Väntar') }}</span>
            </label>
          </fieldset>
          </div>
        </Panel>
      </div>
    </div>
  </section>
</template>
<style scoped>
.cases-page-header {
  margin-bottom: 12px;
}

.cases-search {
  width: 100%;
  margin-bottom: 24px;
}

.cases-search__header {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 8px;
}

.cases-search__label {
  display: block;
  color: var(--ink);
  font-size: 0.9rem;
  font-weight: 700;
}

.cases-search__count {
  color: var(--muted);
  font-size: 0.9rem;
}

.cases-search__input {
  box-sizing: border-box;
  width: 100%;
  min-height: 44px;
  padding: 10px 12px;
  border: 1px solid var(--border-strong, var(--border));
  border-radius: var(--radius-control);
  background: var(--surface);
  color: var(--ink);
  font: inherit;
  line-height: 1.4;
}

.cases-search__input::placeholder {
  color: var(--muted);
}

.cases-search__input:focus-visible {
  outline: 3px solid color-mix(in srgb, var(--primary) 24%, transparent);
  outline-offset: 2px;
  border-color: var(--primary);
}

.cases-layout {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(240px, 280px);
  gap: 24px;
  align-items: start;
}

.cases-sidebar {
  min-width: 0;
}

.cases-filter {
  display: grid;
  gap: 16px;
}

.cases-filter__heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--border);
  color: var(--muted);
  font-size: 0.9rem;
}

.cases-filter__clear {
  padding: 0;
  border: 0;
  background: transparent;
  color: var(--primary);
  font: inherit;
  cursor: pointer;
}

.cases-filter__check,
.cases-filter__sort label {
  display: flex;
  align-items: center;
  gap: 10px;
  color: var(--ink);
  font-size: 0.95rem;
}

.cases-filter__check input,
.cases-filter__sort input {
  width: 18px;
  height: 18px;
  accent-color: var(--primary);
}

.cases-filter__group {
  display: grid;
  gap: 6px;
  margin: 8px 0 0;
  padding: 0;
  border: 0;
}

.cases-filter__group legend {
  margin-bottom: 12px;
  color: var(--ink);
  font-size: 0.95rem;
  font-weight: 700;
}

.table-link {
  display: inline;
  padding: 0;
  min-height: auto;
  border: 0;
  background: transparent;
  color: var(--primary);
  font-weight: 700;
  text-align: left;
  text-decoration: underline;
  text-decoration-thickness: 1px;
  text-underline-offset: 3px;
}
.table-link:hover {
  color: var(--primary-strong);
}

@media (max-width: 760px) {
  .cases-search {
    width: 100%;
  }

  .cases-layout {
    grid-template-columns: 1fr;
  }
}
</style>

<script setup lang="ts">
export interface CaseDetailsItem {
  name: string
  status: string
  value: string
  detail: string
}

defineProps<{
  item: CaseDetailsItem
  canApprove: boolean
  error?: string
  labels: {
    eyebrow: string
    close: string
    status: string
    due: string
    details: string
    approve: string
    approved: string
    readOnly: string
  }
}>()

const emit = defineEmits<{ close: []; approve: [] }>()

const isApproved = (status: string): boolean =>
  ['approved', 'godkänd'].includes(status.trim().toLocaleLowerCase())
</script>

<template>
  <article class="case-details">
    <div class="case-details__header">
      <div>
        <p class="case-details__eyebrow">{{ labels.eyebrow }}</p>
        <h1>{{ item.name }}</h1>
      </div>
      <button
        class="icon-button"
        type="button"
        :aria-label="labels.close"
        @click="emit('close')"
      >
        ×
      </button>
    </div>

    <dl class="case-details__summary">
      <div>
        <dt>{{ labels.status }}</dt>
        <dd>{{ item.status }}</dd>
      </div>
      <div>
        <dt>{{ labels.due }}</dt>
        <dd>{{ item.value }}</dd>
      </div>
    </dl>

    <div class="case-details__content">
      <h2>{{ labels.details }}</h2>
      <p>{{ item.detail }}</p>
    </div>

    <p
      v-if="!canApprove && !isApproved(item.status)"
      class="case-details__readonly"
    >
      {{ labels.readOnly }}
    </p>
    <p v-if="error" class="case-details__error" role="alert">
      {{ error }}
    </p>

    <div class="case-details__actions">
      <button class="button secondary" type="button" @click="emit('close')">
        {{ labels.close }}
      </button>
      <button
        v-if="canApprove && !isApproved(item.status)"
        class="button"
        type="button"
        @click="emit('approve')"
      >
        {{ labels.approve }}
      </button>
    </div>
  </article>
</template>

<style scoped>
.case-details {
  color: var(--ink);
}

.case-details__header {
  display: flex;
  justify-content: space-between;
  gap: 24px;
  align-items: flex-start;
}

.case-details__eyebrow {
  margin: 0 0 8px;
  color: var(--muted);
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: var(--label-tracking, 0.02em);
  text-transform: var(--label-case, uppercase);
}

.case-details h1,
.case-details h2 {
  margin: 0;
  font-family: var(--font-display, var(--font-sans));
}

.case-details h1 {
  font-size: 1.55rem;
}

.case-details h2 {
  font-size: 1rem;
}

.icon-button {
  width: 44px;
  min-height: 44px;
  padding: 0;
  border: 1px solid var(--border);
  border-radius: var(--radius-control);
  background: transparent;
  color: var(--ink);
  font-size: 1.6rem;
  line-height: 1;
}

.case-details__summary {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
  margin: 28px 0;
  padding: 16px 0;
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
}

.case-details__summary div {
  display: grid;
  gap: 4px;
}

.case-details dt {
  color: var(--muted);
  font-size: 0.8rem;
}

.case-details dd {
  margin: 0;
  font-weight: 700;
}

.case-details__content {
  line-height: 1.55;
}

.case-details__content p {
  margin: 8px 0 0;
  color: var(--muted);
}

.case-details__readonly,
.case-details__error {
  margin-top: 20px;
  color: var(--muted);
  font-size: 0.9rem;
}

.case-details__error {
  padding: 12px 14px;
  border: 1px solid var(--error-border, var(--border));
  border-radius: var(--radius-control);
  background: var(--error-bg, var(--surface));
  color: var(--error-ink, var(--ink));
  line-height: 1.45;
}

.case-details__actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 28px;
}

@media (max-width: 520px) {
  .case-details h1 {
    max-width: calc(100vw - 112px);
    font-size: 1.3rem;
  }

  .case-details__summary {
    grid-template-columns: 1fr;
  }

  .case-details__actions {
    flex-direction: column-reverse;
  }

  .case-details__actions .button {
    width: 100%;
  }
}
</style>

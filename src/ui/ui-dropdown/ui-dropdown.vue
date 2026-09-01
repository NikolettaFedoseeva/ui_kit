<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount, nextTick } from 'vue'
import type { DropdownOption } from './types'

interface Props {
  modelValue?: string | null
  options: DropdownOption[]
  label?: string
  placeholder?: string
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: null,
  label: '',
  placeholder: 'Выберите значение...',
  disabled: false,
})

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
  (e: 'select', option: DropdownOption): void
}>()

const isOpen = ref(false)
const dropUp = ref(false)
const dropdownRef = ref<HTMLElement | null>(null)
const menuRef = ref<HTMLElement | null>(null)

const selectedOption = computed(() => {
  return props.options.find((opt) => opt.id === props.modelValue) || null
})

const calculatePosition = () => {
  if (!dropdownRef.value) return
  const rect = dropdownRef.value.getBoundingClientRect()
  const spaceBelow = window.innerHeight - rect.bottom
  const menuHeight = menuRef.value ? menuRef.value.offsetHeight : 220

  if (spaceBelow < menuHeight && rect.top > menuHeight) {
    dropUp.value = true
  } else {
    dropUp.value = false
  }
}

const toggleDropdown = async () => {
  if (props.disabled) return
  isOpen.value = !isOpen.value

  if (isOpen.value) {
    await nextTick()
    calculatePosition()
  }
}

const onSelectOption = (opt: DropdownOption) => {
  if (opt.disabled) return
  emit('update:modelValue', opt.id)
  emit('select', opt)
  isOpen.value = false
}

const handleClickOutside = (event: MouseEvent) => {
  if (dropdownRef.value && !dropdownRef.value.contains(event.target as Node)) {
    isOpen.value = false
  }
}

const handleScrollOrResize = () => {
  if (isOpen.value) {
    calculatePosition()
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
  window.addEventListener('resize', handleScrollOrResize)
  window.addEventListener('scroll', handleScrollOrResize, true)
})

onBeforeUnmount(() => {
  document.removeEventListener('click', handleClickOutside)
  window.removeEventListener('resize', handleScrollOrResize)
  window.removeEventListener('scroll', handleScrollOrResize, true)
})
</script>

<template>
  <div
    ref="dropdownRef"
    class="ui-dropdown"
    :class="{ 'ui-dropdown--open': isOpen, 'ui-dropdown--disabled': props.disabled }"
  >
    <label v-if="props.label" class="ui-dropdown__label">{{ props.label }}</label>

    <div class="ui-dropdown__trigger" @click="toggleDropdown">
      <span v-if="selectedOption" class="ui-dropdown__selected">
        <span v-if="selectedOption.icon" class="ui-dropdown__option-icon">{{ selectedOption.icon }}</span>
        <span class="ui-dropdown__selected-text">{{ selectedOption.label }}</span>
      </span>
      <span v-else class="ui-dropdown__placeholder">{{ props.placeholder }}</span>
      <span class="ui-dropdown__arrow">▼</span>
    </div>

    <transition name="dropdown-fade">
      <div
        v-if="isOpen"
        ref="menuRef"
        class="ui-dropdown__menu"
        :class="dropUp ? 'ui-dropdown__menu--top' : 'ui-dropdown__menu--bottom'"
      >
        <div
          v-for="opt in props.options"
          :key="opt.id"
          class="ui-dropdown__item"
          :class="{
            'ui-dropdown__item--selected': props.modelValue === opt.id,
            'ui-dropdown__item--disabled': opt.disabled
          }"
          @click="onSelectOption(opt)"
        >
          <span v-if="opt.icon" class="ui-dropdown__option-icon">{{ opt.icon }}</span>
          <span class="ui-dropdown__item-text">{{ opt.label }}</span>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped lang="scss">
.ui-dropdown {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  position: relative;
  width: 100%;

  &--disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  &__label {
    font-size: 0.85rem;
    font-weight: 700;
    color: var(--text-main);
  }

  &__trigger {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem 0.85rem;
    border-radius: var(--radius-sm);
    background: var(--bg-card-hover);
    border: 1px solid var(--border-color);
    color: var(--text-main);
    font-weight: 600;
    font-size: 0.85rem;
    cursor: pointer;
    backdrop-filter: blur(10px);
    transition: all 0.2s ease;
    white-space: nowrap;

    &:hover {
      border-color: var(--primary);
    }
  }

  &__selected {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  &__selected-text {
    overflow: hidden;
    text-overflow: ellipsis;
  }

  &--open &__trigger {
    border-color: var(--primary);
    box-shadow: var(--shadow-glow);
  }

  &__arrow {
    font-size: 0.65rem;
    opacity: 0.7;
    margin-left: 0.5rem;
    transition: transform 0.2s ease;
  }

  &--open &__arrow {
    transform: rotate(180deg);
  }

  &__placeholder {
    color: var(--text-muted);
  }

  &__menu {
    position: absolute;
    right: 0;
    min-width: 220px;
    z-index: 1000;
    display: flex;
    flex-direction: column;
    padding: 0.4rem;
    border-radius: var(--radius-md);
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    box-shadow: 0 16px 40px rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(16px);
    max-height: 260px;
    overflow-y: auto;

    &--bottom {
      top: calc(100% + 6px);
    }

    &--top {
      bottom: calc(100% + 6px);
    }
  }

  &__item {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.6rem 0.75rem;
    border-radius: var(--radius-sm);
    color: var(--text-main);
    font-size: 0.85rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.15s ease;

    &:hover:not(&--disabled):not(&--selected) {
      background: var(--bg-card-hover);
      color: var(--primary);
    }

    &--selected {
      background: var(--primary-gradient);
      color: #ffffff !important;
    }

    &--disabled {
      opacity: 0.4;
      cursor: not-allowed;
    }
  }
}

.dropdown-fade-enter-active,
.dropdown-fade-leave-active {
  transition: all 0.2s ease;
}

.dropdown-fade-enter-from,
.dropdown-fade-leave-to {
  opacity: 0;
  transform: translateY(-6px);
}
</style>
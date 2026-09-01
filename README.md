# 🎨 Lern UI Kit & Theme System

Единая библиотека компонентов и динамических визуальных стилей для платформы микрофронтендов **Lern Codex Platform**.

---

## 📦 Установка (Installation)

### Из Git-репозитория:
`ash
npm install git+https://github.com/your-username/lern-ui-kit.git#main
`

### При локальной разработке (Local file linking):
Добавьте в package.json вашего подпроекта:
`json
"dependencies": {
  "lern-ui-kit": "file:../lern_ui_kit"
}
`
Затем выполните:
`ash
npm install
`

---

## 🚀 Использование компонентов (Usage)

В Vue 3 + TypeScript компонентах:

`ue
<script setup lang="ts">
import { 
  UiButton, 
  UiCard, 
  UiBadge, 
  UiInput, 
  UiQuestion, 
  useTheme 
} from 'lern-ui-kit'

// Управление 7 динамическими темами
const { currentTheme, setTheme } = useTheme()
</script>

<template>
  <UiCard variant="glass">
    <UiBadge variant="primary">УРОК 01</UiBadge>
    <h2>Основы Vue 3 Composition API</h2>
    <UiButton variant="primary" @click="setTheme('codex-navy')">
      Сменить тему на Codex Navy 🧭
    </UiButton>
  </UiCard>
</template>
`

---

## 🧩 Состав UI Kit (15+ FSD компонентов)

| Компонент | Описание | Основные варианты |
|---|---|---|
| **UiButton** | Кнопка действия | primary, secondary, ghost, success, warning, danger |
| **UiCard** | Карточка-контейнер | glass, solid, glow, clay, parchment |
| **UiBadge** | Индикатор статуса и тэги | primary, secondary, success, warning, danger |
| **UiInput** | Поле ввода | Текстовые инпуты, иконки, очистка, ошибко-валидация |
| **UiQuestion** | Тест с выбором ответов | A/B/C, радио-выбор, автопроверка correct/wrong |
| **UiTabs** | Переключатель вкладок | Размеры sm/md/lg, иконки, счетчики-бейджи |
| **UiProgressBar** | Прогресс-бар выполнения | Анимированный шиммер, линейный процент |
| **UiToggle** | Тумблер переключатель | Focus Mode, Автопроверка |
| **UiDropdown** | Выпадающий список | Автоориентация ottom/top, затененное меню |
| **UiModal** | Модальное окно | Слоты header/body/footer, закрытие по Esc |
| **UiAvatar** | Аватар пользователя | Инициалы, статусы online/busy/away/offline |
| **UiAlert** | Уведомление / плашка | info, success, warning, danger |
| **UiTable** | Таблица данных | Сортировка по колонкам, кастомные слоты |
| **UiCarousel** | Слайдер / Карусель | Навигация «‹» «›», точечные индикаторы |
| **InteractiveFlashcard** | 3D-флешкарта Active Recall | Разворот карточки по оси Y, самооценка |

---

## 🎨 7 Динамических тем

1. 🧭 **codex-navy**: Академический морской сине-золотой фон + светлый пергамент.
2. 🍷 **codex-burgundy**: Бордовый виноградный фон + теплый кремовый пергамент.
3. 🧘‍♀️ **mindful-sage**: Шалфейное туманное стекло Mindora Glassmorphism.
4. ⚡ **uturistic-light**: Светлый Sci-Fi макет Zenith.
5. 🎨 **pastel-clay**: Объёмный 3D-пластилиновый стиль Claymorphism.
6. 🌈 **ura-holographic**: Голографическое перламутровое стекло.
7. 🌌 **dark-cyber**: Высокотехнологичный тёмный High-Tech UI.
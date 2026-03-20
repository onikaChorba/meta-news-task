<script setup lang="ts">
type Locale = 'UA' | 'RU';

defineProps<{
  isOpen: boolean,
  currentLocale: Locale
}>();

const rubrics: string[] = [
  'НОВОСТИ', 'РЕГИОНЫ', 'СПОРТ', 'СТАВКИ', 'ФИНАНСЫ',
  'ШОУ-БИЗНЕС', 'LIFESTYLE', 'ЗДОРОВЬЕ', 'IT', 'АВТО'
];

const projects: string[] = [
  'ПОЧТА', 'ПОГОДА', 'ТВ-ПРОГРАММА', 'КАРТЫ', 'ПЕРЕВОДЧИК'
];

const emit = defineEmits<{
  (e: 'close'): void;
  (e: 'update:locale', locale: Locale): void;
}>();

const setLocale = (locale: Locale) => {
  emit('update:locale', locale);
};

const closeMenu = (): void => {
  emit('close');
};
</script>

<template>
  <transition name="slide">
    <aside v-if="isOpen" class="sidebar">
      <div class="sidebar__content">
        <div class="sidebar__logo">
          <img src="@/assets/imgs/logo-light-theme.png" alt="META" />
        </div>

        <div class="sidebar__wrapper">
          <nav class="sidebar__nav">
            <div class="sidebar__section">
              <h4 class="sidebar__title">Рубрики</h4>
              <ul class="sidebar__list">
                <li v-for="item in rubrics" :key="item" :class="{ 'is-active': item === 'СПОРТ' }">
                  <a href="#">{{ item }}</a>
                </li>
              </ul>
            </div>

            <div class="sidebar__section">
              <h4 class="sidebar__title">Проекты мета</h4>
              <ul class="sidebar__list">
                <li v-for="item in projects" :key="item">
                  <a href="#">{{ item }}</a>
                </li>
              </ul>
            </div>
          </nav>

          <div class="sidebar__footer footer-sidebar">
            <div class="footer-sidebar__item">
              <span class="footer-sidebar__label">Темная тема</span>
              <div class="theme-switch">
                <input type="checkbox" id="theme-toggle" class="theme-switch__input" />
                <label for="theme-toggle" class="theme-switch__slider"></label>
              </div>
            </div>

            <div class="footer-sidebar__item">
              <span class="footer-sidebar__label">Текст новостей</span>
              <div class="lang-switcher">
                <button type="button" class="lang-switcher__btn" :class="{ 'is-active': currentLocale === 'UA' }"
                  @click="setLocale('UA')">UA</button>
                <span class="lang-switcher__divider"></span>
                <button type="button" class="lang-switcher__btn" :class="{ 'is-active': currentLocale === 'RU' }"
                  @click="setLocale('RU')">RU</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="sidebar__overlay" @click="closeMenu"></div>
    </aside>
  </transition>
</template>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 2000;
  display: flex;

  &__content {
    width: 300px;
    height: 100%;
    z-index: 2;
    display: flex;
    flex-direction: column;
    background-color: $side-bar-bg;
  }

  &__wrapper {
    overflow-y: scroll;
    scrollbar-width: none;
    -ms-overflow-style: none;

    &::-webkit-scrollbar {
      display: none;
    }
  }

  &__logo {
    align-self: center;
    width: 120px;
    margin: 40px 0px;

    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }

  &__section {
    padding-top: 20px;
    margin-bottom: 32px;
    border-top: 1px solid $tag-bg;
  }

  &__nav {
    margin-top: 8px;
    border-bottom: 1px solid $tag-bg;

    li {
      display: flex;
      align-items: center;
      height: 40px;
      font-family: 'Roboto';
      font-style: normal;
      font-weight: 500;
      font-size: 14px;
      line-height: 16px;
      text-transform: uppercase;
      color: $tag-color;
      padding-left: 24px;


      &:hover {
        background-color: $side-bar-nav-hover;
      }
    }
  }

  &__title {
    padding-left: 24px;
    margin-bottom: 8px;
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    color: $primary-blue;
  }

  &__overlay {
    flex: 1;
  }

  &__footer {
    display: flex;
    flex-direction: column;
    color: $tag-color;
    background-color: $side-bar-bg;
    padding: 19px 24px 58px 32px;
  }
}

.footer-sidebar {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 16px;
  color: $tag-color;
  margin-top: auto;

  &__item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 40px;
  }
}

.theme-switch {
  position: relative;
  width: 40px;
  height: 24px;

  &__input {
    opacity: 0;
    width: 0;
    height: 0;

    &:checked+.theme-switch__slider {
      border: 2px solid $primary-blue;

      &::before {
        transform: translateX(14px);
        background-color: $primary-blue;
      }
    }
  }

  &__slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    transition: 0.4s;
    border-radius: 20px;
    border: 2px solid $grey;

    &::before {
      position: absolute;
      content: "";
      height: 16px;
      width: 16px;
      left: 3px;
      top: 2px;
      background-color: $grey;
      transition: 0.4s;
      border-radius: 50%;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
  }
}

.lang-switcher {
  display: flex;
  align-items: center;
  gap: 4px;

  &__btn {
    font-family: 'Roboto';
    font-weight: 700;
    font-size: 12px;
    color: $grey;
    background: none;
    border: none;
    cursor: pointer;
    padding: 2px 4px;
    transition: color 0.3s;

    &.is-active {
      color: $primary-blue;
    }
  }
}

.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.3s ease;

  .sidebar__content {
    transition: transform 0.3s ease;
  }
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;

  .sidebar__content {
    transform: translateX(-100%);
  }
}

@media (max-width: 1024px) {

  .nav-top,
  .header__divider,
  .user-actions__btn:last-child {
    display: none;
  }

  .header-content {
    padding: 10px 0;

    &__left {
      gap: 12px;
    }

    &__logo {
      width: 70px;
    }

    &__lang {
      margin-left: auto;
      margin-right: 12px;
      gap: 4px;
      font-size: 11px;
    }

    &__actions {
      gap: 12px;
    }
  }

  .header__mainlink {
    padding: 12px 0;
    margin: 0 -16px;
    background-color: #2D2D2D;
  }

  .main-link {
    display: flex;
    flex-wrap: nowrap;
    overflow-x: auto;
    padding: 0 16px;
    gap: 20px;
    scrollbar-width: none;

    &::-webkit-scrollbar {
      display: none;
    }

    &__item {
      flex: 0 0 auto;

      &--more {
        display: none;
      }
    }

    &__link {
      font-size: 13px;
      white-space: nowrap;

      &::after {
        bottom: -12px;
      }
    }
  }
}

@media (max-width: 360px) {
  .header-content {
    &__left {
      gap: 8px;
    }

    &__lang {
      margin-right: 8px;
    }

    .user-actions {
      gap: 8px;
    }
  }
}
</style>
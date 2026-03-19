<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

interface NavLink {
  name: string;
  href: string;
}

type Locale = 'UA' | 'RU';

const isProfileOpen = ref(false);
const currentLocale = ref<Locale>('UA');
const notificationsCount = ref(1);
const activeMainLink = ref("ПОЛИТИКА");
const isMoreMenuOpen = ref(false);
const profileRef = ref<HTMLElement | null>(null);
const moreMenuRef = ref<HTMLElement | null>(null);

const topNavLinks: NavLink[] = [
  { name: "НОВОСТИ", href: "#" },
  { name: "спорт", href: "#" },
  { name: "беттинг", href: "#" },
  { name: "финансы", href: "#" },
  { name: "шоубиз", href: "#" },
  { name: "лайфстайл", href: "#" },
  { name: "IT", href: "#" },
  { name: "авто", href: "#" }
];

const mainNavLinks: NavLink[] = [
  { name: "ГЛАВНАЯ", href: "#" },
  { name: "ПРОИСШЕСТВИЯ", href: "#" },
  { name: "ОБЩЕСТВО", href: "#" },
  { name: "ЭКОНОМИКА", href: "#" },
  { name: "ПОЛИТИКА", href: "#" },
  { name: "ОБРАЗОВАНИЕ", href: "#" },
  { name: "МЕДИЦИНА", href: "#" },
  { name: "ВИДЕО", href: "#" },
  { name: "ФОТО", href: "#" }
];

const setLocale = (locale: Locale) => {
  currentLocale.value = locale;
  console.log(`Language changed to: ${locale}`);
};

const setActive = (name: string) => {
  activeMainLink.value = name;
};

const toggleMoreMenu = () => {
  isMoreMenuOpen.value = !isMoreMenuOpen.value;
};

const handleClickOutside = (event: MouseEvent) => {
  const target = event.target as HTMLElement;

  if (isProfileOpen.value && profileRef.value && !profileRef.value.contains(target)) {
    isProfileOpen.value = false;
  }

  if (isMoreMenuOpen.value && moreMenuRef.value && !moreMenuRef.value.contains(target)) {
    isMoreMenuOpen.value = false;
  }
};

onMounted(() => {
  document.addEventListener('click', handleClickOutside);
});

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside);
});

</script>

<template>
  <div class="header-wrapper">
    <div class="wrapper">
      <header class="header">
        <div class="header-content">
          <div class="header-content__left">
            <button class="header-content__burger">
              <img src="/src/assets/icons/icon-menu.svg" alt="menu" />
            </button>
            <div class="header-content__logo">
              <img src="/src/assets/imgs/logo.png" alt="logo" />
            </div>
            <nav class="header-content__nav nav-top">
              <ul class="nav-top__list">
                <li v-for="link in topNavLinks" :key="link.name" class="nav-top__item">
                  <a :href="link.href" class="nav-top__link">
                    {{ link.name }}
                  </a>
                </li>
              </ul>
            </nav>
          </div>
          <div class="header-content__right">
            <div class="header-content__lang lang-switcher">
              <button type="button" class="lang-switcher__btn" :class="{ 'is-active': currentLocale === 'UA' }"
                @click="setLocale('UA')">UA</button>
              <span class="lang-switcher__divider"></span>
              <button type="button" class="lang-switcher__btn" :class="{ 'is-active': currentLocale === 'RU' }"
                @click="setLocale('RU')">RU</button>
            </div>
            <div class="header-content__actions user-actions">
              <button class="user-actions__btn">
                <img src="/src/assets/icons/icon-search.svg" alt="search" />
              </button>
              <div class="user-actions__btn">
                <button class="btn-notification">
                  <img src="/src/assets/icons/icon-notification.svg" alt="notification" />
                  <span class="btn-notification__count">
                    {{ notificationsCount }}
                  </span>
                </button>
              </div>

              <div class="user-actions__profile user-profile" ref="profileRef">
                <button class="user-actions__btn" @click="isProfileOpen = !isProfileOpen">
                  <img src="/src/assets/icons/icon-user.svg" alt="user" />
                </button>

                <div v-if="isProfileOpen" class="user-profile__dropdown dropdown-menu">
                  <ul class="dropdown-menu__list">
                    <li class="dropdown-menu__item">Мій профіль</li>
                    <li class="dropdown-menu__item">Налаштування</li>
                    <li class="dropdown-menu__item">Вихід <img src="/src/assets/icons/icon-exit.svg" /></li>
                  </ul>
                </div>
              </div>

              <button class="user-actions__btn">
                <img src="/src/assets/icons/icon-message.svg" alt="message" />
              </button>
            </div>
          </div>
        </div>

        <hr class="header__divider" />

        <div class="header__mainlink">
          <ul class="main-link">
            <li v-for="link in mainNavLinks" :key="link.name" class="main-link__item">
              <a :href="link.href" class="main-link__link" :class="{ 'is-active': activeMainLink === link.name }"
                @click.prevent="setActive(link.name)">
                {{ link.name }}
              </a>
            </li>

            <li class="main-link__item main-link__item--more" ref="moreMenuRef">
              <button class="main-link__more-btn" @click="toggleMoreMenu">
                ЕЩЕ
                <img src="/src/assets/icons/icon-arrow.svg" />
              </button>

              <div v-if="isMoreMenuOpen" class="main-link__dropdown dropdown-more">
                <ul class="dropdown-more__list">
                  <li><a href="#" class="dropdown-more__link">Архів</a></li>
                  <li><a href="#" class="dropdown-more__link">Контакти</a></li>
                </ul>
              </div>
            </li>
          </ul>
        </div>
      </header>
    </div>
  </div>
</template>

<style lang="scss">
.header-wrapper {
  display: flex;
  flex-direction: column;
  background-color: $header-bg;
  color: $header-text;
}

.header {
  max-width: 1208px;
  margin: 0 auto;

  &__divider {
    width: 100%;
    height: 1px;
    background: $hr;
  }

  &__mainlink {
    padding: 24px 0px;
  }
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 18px 0px 16px 0px;

  &__left {
    display: flex;
    align-items: center;
    gap: 32px;
  }

  &__right {
    display: flex;
    align-items: center;
    gap: 26px;
  }

  &__burger {
    width: 24px;
    height: 24px;

    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }

  &__logo {
    width: 91px;
    height: 32px;

    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }

  &__nav {
    margin-left: 24px;
  }

  &__lang {
    display: flex;
    gap: 5px;
    color: $primary-blue;
  }

  &__actions {
    display: flex;
    align-items: center;
    gap: 20px;
  }
}

.btn-notification {
  position: relative;

  &__count {
    position: absolute;
    top: -7px;
    right: -5px;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background-color: $primary-orange;
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 700;
    font-size: 11px;
    line-height: 14px;
    text-align: center;
    text-transform: uppercase;
    color: $black;
  }
}

.nav-top {
  &__list {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 20px;
    color: $primary-blue;
  }

  &__item {
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 500;
    font-size: 12px;
    line-height: 14px;
    text-transform: uppercase;
    color: $primary-blue;
  }
}

.lang-switcher {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
  line-height: 14px;
  text-transform: uppercase;
  color: $header-text-secondary;

  &__btn {
    cursor: pointer;

    &.is-active {
      color: $primary-blue;
    }
  }
}

.user-actions {
  &__btn {
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

.main-link {
  max-width: 1162px;
  display: flex;
  flex-wrap: wrap;
  gap: 35px;
  list-style: none;
  padding: 0;
  margin: 0;

  &__item {
    &--more {
      position: relative;
      margin-left: auto;
    }
  }

  &__link {
    position: relative;
    display: inline-block;
    font-family: 'Roboto', sans-serif;
    font-weight: 700;
    font-size: 14px;
    text-transform: uppercase;
    text-decoration: none;
    color: $header-text;

    transition: color 0.3s ease;
    cursor: pointer;

    &::after {
      content: '';
      position: absolute;
      bottom: -24px;
      left: 0;
      width: 100%;
      height: 4px;
      background-color: $primary-blue;
      transform: scaleX(0);
      transform-origin: left;
      transition: transform 0.3s ease;
    }

    &:hover {
      color: $primary-blue;
    }

    &.is-active {
      color: $primary-blue;

      &::after {
        transform: scaleX(1);
      }
    }
  }

  &__more-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    background: none;
    border: none;
    cursor: pointer;
    font-family: 'Roboto', sans-serif;
    font-weight: 700;
    font-size: 14px;
    text-transform: uppercase;
    color: $header-text;
    transition: color 0.3s ease;
  }
}

.user-profile {
  position: relative;

  &__dropdown {
    position: absolute;
    top: calc(100% + 15px);
    right: -10px;
    background-color: $header-text;
    width: 200px;
    height: 153px;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    border: 1px solid rgba(255, 255, 255, 0.1);
    z-index: 1000;

    &::before {
      content: '';
      position: absolute;
      top: -6px;
      right: 15px;
      width: 12px;
      height: 12px;
      background-color: $header-text;
      transform: rotate(45deg);
      border-left: 1px solid rgba(255, 255, 255, 0.1);
      border-top: 1px solid rgba(255, 255, 255, 0.1);
    }
  }
}

.dropdown-menu {
  display: flex;
  flex-direction: column;

  &__list {
    list-style: none;
    padding: 0;
    margin: 0;
    margin-top: 8px;
    margin-bottom: 8px;
  }

  &__item {
    padding: 12px 20px;
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 16px;

    color: $tag-color;
    cursor: pointer;
    transition: background 0.2s ease, color 0.2s ease;

    &:hover {
      background: linear-gradient(0deg, rgba(0, 0, 0, 0.03), rgba(0, 0, 0, 0.03)), #FFFFFF;
    }

    &:last-child {
      display: flex;
      justify-content: space-between;
      border-top: 1px solid rgba(0, 0, 0, 0.05);
      margin-top: 8px;
    }
  }

}
</style>
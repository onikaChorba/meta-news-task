<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import SidebarMenu from './SidebarMenu.vue';
import ProfileDropdown from './ProfileDropdown.vue';

interface NavLink {
  name: string;
  href: string;
}

type Locale = 'UA' | 'RU';

const isSidebarOpen = ref(false);
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

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

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
            <button class="header-content__burger" @click="toggleSidebar">
              <img src="/src/assets/icons/icon-menu.svg" alt="menu" />
            </button>

            <SidebarMenu :is-open="isSidebarOpen" :current-locale="currentLocale" @close="isSidebarOpen = false"
              @update:locale="setLocale" />

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

                <ProfileDropdown v-if="isProfileOpen" @close="isProfileOpen = false" />
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
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 16px;

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
}

@media (max-width: 1024px) {

  .header-content {
    padding: 10px 0;
  }

  .nav-top,
  .header__divider {
    display: none;
  }

  .header-content__logo {
    width: 68px;
    height: auto;
    margin-left: 8px;
  }

  .header-content__lang {
    display: flex;
    font-size: 11px;
    gap: 4px;
    margin-left: auto;
    margin-right: 15px;

    .lang-switcher__divider {
      height: 10px;
      align-self: center;
    }
  }

  .user-actions {
    gap: 12px;

    &__btn img {
      width: 20px;
    }
  }

  .header__mainlink {
    padding: 10px 0;
    background-color: $header-bg;
    margin: 0 -16px;
  }

  .main-link {
    display: flex;
    flex-wrap: nowrap;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    padding: 0 16px;
    gap: 20px;

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
      color: $header-text;
      padding-bottom: 8px;

      &.is-active::after {
        bottom: 0;
      }
    }
  }
}

@media (max-width: 360px) {
  .header-content__left {
    gap: 8px;
  }

  .header-content__lang {
    margin-right: 8px;
  }

  .user-actions {
    gap: 8px;
  }
}
</style>
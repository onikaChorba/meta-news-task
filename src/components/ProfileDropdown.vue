<script setup lang="ts">
const emit = defineEmits(['close']);
</script>

<template>
  <div class="user-profile__dropdown dropdown-menu">
    <ul class="dropdown-menu__list">
      <li class="dropdown-menu__item" @click="emit('close')">Мій профіль</li>
      <li class="dropdown-menu__item" @click="emit('close')">Налаштування</li>
      <li class="dropdown-menu__item" @click="emit('close')">
        Вихід
        <img src="/src/assets/icons/icon-exit.svg" alt="exit" />
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
.user-profile {
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
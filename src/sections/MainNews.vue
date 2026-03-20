<script setup lang="ts">
import { ref, computed } from 'vue';
import mainImg from '@/assets/imgs/main-news.png';
import news1Img from '@/assets/imgs/news1.png';
import news2Img from '@/assets/imgs/news2.png';
import tagIcon from '@/assets/icons/icon-tag.svg'

const categories = [
  { id: 1, name: 'COVID-19' },
  { id: 2, name: 'Выборы 2020' },
  { id: 3, name: 'Антимонопольный закон' },
  { id: 4, name: 'Евро 2020' },
  { id: 5, name: 'Антимонопольный закон', }
];

const newsFeed = ref([
  { id: 1, time: "08:45", title: "Коронавірус у Києві: за добу діагноз підтвердили у 559 осіб, 23 пацієнти померли", text: "За минулу добу коронавірус діагностували у 559 киян, 23 людини померли. Джерело: мер Києва Віталій Кличко під час онлайн пресконференції.", img: mainImg },
  { id: 2, time: '08:30', title: 'Україна зібрала вже 80% врожаю зернових культур', text: "", img: news1Img },
  { id: 3, time: '08:45', title: 'Росія заборонила співробітникам ФСБ мати дозвіл на проживання...', img: news2Img },
  { id: 4, time: '09:30', title: 'Вибори-2020: ЦВК не попереджає, що підрахунок голосів буде', img: news1Img },
  { id: 5, time: '10:20', title: 'У Білорусі почали відраховувати студентів за протести: з університету - 5 осіб', img: news2Img },
  { id: 6, time: '10:20', title: 'Україна зібрала вже 80% врожаю зернових культур', img: news1Img },
]);

const activeId = ref(newsFeed.value[0].id);

const selectedNews = computed(() => {
  return newsFeed.value.find(news => news.id === activeId.value) || newsFeed.value[0];
});

const selectNews = (id: number) => {
  activeId.value = id;
};
</script>

<template>
  <section class="main-news wrapper">
    <div class="main-news__category-bar category-bar">
      <div class="category-bar__list">
        <a v-for="item in categories" :key="item.id" href="#" class="category-tag">
          <div class="category-tag__icon">
            <img :src="tagIcon" :alt="item.name" />
          </div>
          <span class="category-tag__name">{{ item.name }}</span>
        </a>
      </div>
    </div>

    <div class="main-grid">
      <div class="main-grid__mobile-title hide-mobile">
        <h2 class="news-feed__title">ГЛАВНОЕ</h2>
        <div class="news-feed__list">
          <article v-for="item in newsFeed" :key="item.id" class="news-item"
            :class="{ 'news-item--active': activeId === item.id }" @click="selectNews(item.id)">
            <time class="news-item__time">{{ item.time }}</time>
            <h3 class="news-item__title">
              <span class="news-item__link">{{ item.title }}</span>
            </h3>
          </article>
        </div>
      </div>

      <div class="main-grid__sidebar hide-desktop">
        <aside class="news-feed">
          <h2 class="news-feed__title">ГЛАВНОЕ</h2>
        </aside>
      </div>

      <div class="main-grid__content">
        <article class="news-card">
          <img :src="selectedNews.img" :alt="selectedNews.title" class="news-card__img" />
          <h2 class="news-card__title">{{ selectedNews.title }}</h2>
          <p class="news-card__text">{{ selectedNews.text }}</p>
        </article>
      </div>

      <div class="main-grid__aside">
        <div class="news-card-small-wrapper">
          <article class="news-card-small">
            <img src="@/assets/imgs/news1.png" alt="news" class="news-card-small__img" />
            <h3 class="news-card-small__title">Під кінець робочого тижня температура поповзе вгору</h3>
          </article>
          <article class="news-card-small">
            <img src="@/assets/imgs/news2.png" alt="news" class="news-card-small__img" />
            <h3 class="news-card-small__title">Україна зібрала вже 80% врожаю зернових культур</h3>
          </article>
        </div>

        <div class="news-feed__list feed-list-mobile">
          <article v-for="item in newsFeed" :key="item.id" class="news-item news-item--mobile"
            :class="{ 'news-item--active': activeId === item.id }" @click="selectNews(item.id)">
            <div class="news-item__image-wrapper">
              <img :src="item.img" :alt="item.title" class="news-item__img" />
            </div>

            <div class="news-item__content">
              <time class="news-item__time">{{ item.time }}</time>
              <h3 class="news-item__title">
                <span class="news-item__link">{{ item.title }}</span>
              </h3>
            </div>
          </article>
        </div>

        <button class="more-news-btn">
          Больше новостей
          <span class="more-news-btn__arrow"><img src="/src/assets/icons/icon-arrow-large.svg" /></span>
        </button>
      </div>
    </div>
  </section>
</template>

<style lang="scss">
.main-news {
  display: flex;
  flex-direction: column;
  gap: 32px;

  &__category-bar {
    margin-top: 24px;
  }
}

.category-bar {
  &__list {
    display: flex;
    align-items: center;
    gap: 12px;
  }
}

.category-tag {
  display: flex;
  align-items: center;
  background: $tag-bg;
  border-radius: 18px;
  padding: 9px 16px 8px 10px;
  gap: 8px;
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 19px;
  color: $tag-color;
}

.news-feed {
  display: flex;
  flex-direction: column;
  gap: 16px;

  &__title {
    color: $primary-blue;
    font-family: 'Roboto Slab';
    font-style: normal;
    font-weight: 700;
    font-size: 20px;
    line-height: 26px;
  }

  &__list {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }
}

.news-item {
  display: flex;
  flex-direction: column;
  gap: 4px;

  &__image-wrapper {
    flex: 0 0 100px;
    aspect-ratio: 1 / 1;
    border-radius: 6px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  &__time {
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    color: $grey;
  }

  &__title {
    font-family: 'Roboto Slab';
    font-style: normal;
    font-weight: 500;
    font-size: 16px;
    line-height: 22px;
    color: $tag-color;
    cursor: pointer;

    &:hover {
      color: $secondary-blue;
    }
  }
}

.main-grid {
  display: grid;
  grid-template-columns: 1fr 2.5fr 1.2fr;
  gap: 40px;
  align-items: start;

  @media (max-width: 1024px) {
    grid-template-columns: 1fr;
  }

  &__aside {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
  }
}

.news-card {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 12px;

  &__img {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
    border-radius: 12px;
    display: block;
  }

  &__title {
    font-family: 'Roboto Slab';
    font-style: normal;
    font-weight: 700;
    font-size: 32px;
    line-height: 40px;
    color: $tag-color;
  }

  &__text {
    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 18px;
    line-height: 26px;
    color: $grey ;
  }
}

.news-card-small-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.news-card-small {
  display: flex;
  flex-direction: column;
  gap: 8px;

  &__img {
    &__img {
      width: 100%;
      aspect-ratio: 3 / 2;
      border-radius: 8px;
      overflow: hidden;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
  }

  &__title {
    font-family: 'Roboto Slab';
    font-style: normal;
    font-weight: 700;
    font-size: 18px;
    line-height: 24px;
    color: $tag-color;
  }
}

.more-news-btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  align-self: center;
  max-width: 276px;
  width: 100%;
  height: 40px;
  border-radius: 28px;
  border: 2px solid $primary-blue;
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 700;
  font-size: 16px;
  line-height: 19px;
  color: $tag-color;
  cursor: pointer;
  transition: background 0.3s;
  padding: 0px 20px;

  &:hover {
    background: $primary-blue;
    color: #fff;
  }
}

.hide-desktop {
  display: none;
}

.feed-list-mobile {
  display: none;
}

@media (max-width: 1024px) {

  .hide-mobile {
    display: none;
  }

  .hide-desktop {
    display: block;
  }

  .feed-list-mobile {
    display: block;
    margin-bottom: 32px;
  }

  .main-news {
    padding: 0 16px;
    gap: 16px;
  }

  .category-bar__list {
    overflow-x: auto;
    white-space: nowrap;
    padding-bottom: 8px;

    &::-webkit-scrollbar {
      display: none;
    }
  }

  .main-grid {
    display: flex;
    flex-direction: column;
    gap: 20px;

    &__mobile-title {
      margin-bottom: -10px;
    }
  }

  .news-card-small {
    &__img {
      height: auto;
      aspect-ratio: 16 / 10;
    }
  }

  .news-card-small-wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 20px;
  }

  .news-card-small__img {}

  .mobile-feed {
    display: flex;
    flex-direction: column;
    gap: 0;
    margin-bottom: 20px;
  }

  .news-item--mobile {
    flex-direction: row;
    align-items: flex-start;
    gap: 12px;
    padding: 12px 0;

    .news-item__image-wrapper {
      flex: 0 0 90px;
      height: 60px;
      aspect-ratio: auto;
    }
  }

  .more-news-btn {
    border-radius: 20px;
  }
}
</style>
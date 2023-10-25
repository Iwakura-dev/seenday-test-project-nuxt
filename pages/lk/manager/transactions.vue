<script setup lang="ts">
// TODO: Imports

import { onMounted, reactive, ref, watch } from "vue";
import { useAPIFetch } from "@/composables/useAPIFetch";
import type { IUnload } from "~/types/unload";
import LeftSideTitle from "~/components/transactions/leftSide/LeftSideTitle";
import RightSideNotice from "~/components/transactions/rightSide/RightSideNotice";
import UnloadCards from "~/components/transactions/unloadCards/UnloadCards.vue";
import ActiveUnload from "~/components/transactions/rightSide/ActiveUnload.vue";

// FIXME: CODE

// Создаем переменную в которую будет сохранять приходящие данные из API
const allUnloadsList: IUnload[] = reactive([]);
// Создаем переменную в которой будет хранится единственная активная карточки, и уже из нее будет вытаскивать данные для копирования ссылки
const firstActiveUnload: IUnload = ref({});
// при рендере создаем исскуственную загрузку с запросом данных
onMounted(() => {
  setTimeout(async () => {
    const { data } = await useAPIFetch("https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get", {
      query: {
        page: "pages:unload",
        event: "get"
      }
    });
    // добавляем распаршенные данные в конец массива
    allUnloadsList.push(...JSON.parse(data.value as string).response.data);
  }, 200);
});
// создаем состояние для показа выгрузки
const show = ref(false);
// создаем функцию для вытягивания единственной карточки
const getActiveUnload = async (activeUnloadId: string) => {
  const { data } = await useAPIFetch("https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get", {
    query: {
      page: "pages:unload",
      event: "get",
      unload_id: activeUnloadId
    }
  });
  // добавляем распаршенные данные в конец массива
  if (JSON.parse(data.value).response.data.length) {
    firstActiveUnload.value = JSON.parse(data.value).response.data[0];
  }
};
// создаем runtime для единственной карточки, и в зависимости от firstActiveUnload следим, нажал ли пользователь на карточку либо же нет
// после меняем наше состояние show, для рендера выгрузки
watch(
  () => firstActiveUnload,
  () => {
    if (!show.value) {
      show.value = true;
    } else {
      show.value = false;
    }
  },
  { deep: true }
);
</script>

<template>
  <main>
    <!-- Left Side Block -->
    <div class="flex base-gap">
      <LeftSideTitle />
      <!-- Right Side Block -->
      <ActiveUnload v-if="show" :unload="firstActiveUnload" />
      <RightSideNotice v-else />
      <!-- Unload Component -->
      <div class="flex w50 f-d_c base-gap mt-24">
        <UnloadCards
          v-for="unload in allUnloadsList"
          :key="unload.id"
          :unload="unload"
          @active-unload-card="getActiveUnload"
        />
      </div>
    </div>
  </main>
</template>

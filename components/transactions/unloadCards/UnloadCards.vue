<script setup lang="ts">
import { onActivated, defineEmits } from "vue";
import { Unload } from "#build/types/types";
// Создаем интерфейс для приходящих к нам пропсов
interface PropsUnloadCards {
  unload: Unload;
}
// создаем эмит по событию
defineEmits(["activeUnloadCard"]);
// создаем переменную в которой будет хранится выгрузка карт
const unloadCard: HTMLDivElement | null = ref(null);
// создаем пропсы и в качестве дженерика добавляем наши пропсы
const props = defineProps<PropsUnloadCards>();
// вызываем функцию onActived, которая регестрирует обратный вызов наших карт
onActivated(() => {
  // создаем переменную из которой будем менять статус на success ~ danger
  let status: string = "";
  // создаем свитч-кейс и в зависимости от статус будем менять цвет нашего border в карточках
  switch (props.unload.status) {
    case "green":
      status = "--color_success";
      break;
    default:
      status = "--color_danger";
      break;
  }
  // создаем условие при котором, если наша карточка не равняется null, то добавляем к карточке border с литеральной строкой
  if (unloadCard.value !== null) {
    unloadCard.style.borderLeft = `10px solid ${status}`;
  }
});
</script>

<template>
  <!-- Cards -->
  <div class="block default-inner-gap border-1-success w50">
    <!-- Cards information -->
    <div ref="unload" @click="$emit('activeUnloadCard', unload.id)">
      <p v-html="unload.task_date" />

      <p>
        Статус задачи: <span class="text-bold">{{ unload.status_text }}</span>
      </p>
      <p>
        ID выгрузки: <span class="text-bold">{{ unload.id }}</span>
      </p>
      <p v-html="unload.event" />
      <p>Выгружено заказов: <span class="text-bold">6 из 6</span></p>
      <p>
        Размер выгрузки: <span class="text-bold">{{ unload.size }}</span>
      </p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@include start-at(xslg) {
  .unload {
    grid-template-rows: auto 1fr;
    grid-template-columns: 1fr 1fr;
    &-cards {
      grid-column: 1/2;
    }
    &-main {
      grid-column: 2/-2;
      grid-row: -3/-1;
    }
  }
}
@include active-by(xslg) {
  .unload {
    grid-template-rows: auto;
    grid-template-columns: 1fr;
    &-cards {
      grid-column: 1/2;
    }
    &-main {
      grid-column: 1/2;
    }
  }
}
</style>

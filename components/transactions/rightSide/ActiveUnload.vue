<script setup lang="ts">
import { Unload } from "#build/types/types";
// создаем пропс для входящих данных
interface ActiveUnloadProps {
  unload: Unload;
}
defineProps<ActiveUnloadProps>();
// создаем функцию которая при клике див будет вытаскивать dowload link
const copyLink = async (link: string) => {
  try {
    await navigator.clipboard.writeText(link);
    alert("Ссылка скопирована");
  } catch (e) {
    alert("Произошла ошибка");
  }
};
</script>

<template>
  <div class="block">
    <div class="content-header">
      <div class="content-id">
        <span>{{ unload.id }}</span>
      </div>
      <div class="content-event text-bold">
        <span>{{ unload.event }}</span>
      </div>
      <div class="content-close">
        <button type="button">X</button>
      </div>
    </div>
    <div class="content-information">
      <div class="content-notice">
        <p>Если после клика на ссылку загрузка не пошла, проверьте, не блокирует ли браузер скачивание архива</p>
      </div>
      <div class="content-title text-bold">
        <span>Ссылка для скачивания архива Выгрузки (.zip):</span>
      </div>
      <div class="content-download">
        <a href="#">{{ unload.download_link }}</a>
        <div class="text-bold span-link" @click="copyLink(unload.download_link)">
          <span>Скопировать ссылку</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
%close-span {
  content: "";
  display: block;
  background-color: #fff;
  width: 1px;
  height: 25px;
  position: absolute;
}
.content {
  &-header {
    display: flex;
    align-items: center;
    background-color: #f5f5f9;
    justify-content: space-between;
  }
  &-close {
    background-color: #cd5c5c;
    position: relative;
    height: 50px;
    width: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    button {
      background-color: transparent;
      border: none;
      color: #fff;
      font-size: 24px;
      cursor: pointer;
    }
  }
  &-information {
    padding: 15px;
  }
  &-notice {
    margin-bottom: 8px;
    padding: 10px;
    text-align: center;
    background-color: #eceff3;
    border-radius: 5px;
  }
  &-id {
    padding: 15px 10px;
    background-color: #5f5ea0;
    font-size: 16px;
    margin-right: 10px;
    color: #fff;
  }
  &-event {
    flex: 0 1 80%;
  }
  a {
    margin-right: 10px;
    color: #9875d1;
  }
  &-download {
    margin-top: 5px;
  }
}
.span-link {
  cursor: pointer;
  text-decoration-line: underline;
  text-decoration-style: dashed;
  text-underline-offset: 3px;
}
</style>

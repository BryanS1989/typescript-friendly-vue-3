<script setup lang="ts">
import { ref, reactive, onMounted, computed } from "vue";
import fetchCount from "./fetchCount";

interface AppInfo {
  name: string;
  slogan: string
}

/*
  We did not need to set type because of type inference.
  The type will be inferred by the ref value.
  In this case inferred type is Ref<number>

    const count = ref(0);
*/
/*
    If we initialize count with null, type inferred is null
    then we cant set count initial value as zero...
    then we set type as <number | null> to avoid error
*/
const count = ref<number | null>(null);

/*
  In this case, the type could be a number (count.value is a number)
  and if we +1 still a number.
  But we also return null then the type inferred will be:
    
    ComputedRef<number | null>

const nextCount = computed(() => {
  if (count.value !== null) {
    return count.value + 1;
  }

  return null;
});
*/

const appInfo : AppInfo = reactive({
  name: 'Counter',
  slogan: 'an app you can count on'
});

/*
  In this case will be an error, because initialCount is type any.

  type inference can only make its best guess when the callback 
  function is passed inline to fetchCount. TypeScript knows what 
  fetchCount accepts as a parameter, so it just “connects the dots.”
  
const cb = (initialCount) => {
  count.value = initialCount
}

fetchCount(cb)
*/

onMounted(() => {
  // Type inference is able to infer the parameter type if the function is used as an inline callback function.
  fetchCount((initialCount) => {
    count.value = initialCount;
  });
});

function addCount (num: number) {
  if (count.value !== null) {
    count.value += num;
  }
}
</script>

<template>
  <header>
    <div>
      <a href="https://vitejs.dev" target="_blank">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
      </a>
      <a href="https://vuejs.org/" target="_blank">
        <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
      </a>
    </div>

    <h1>{{ appInfo.name }}</h1>
    <h2>{{ appInfo.slogan }}</h2>
  </header>

  <p>{{ count }}</p>

  <p>
    <button @click="addCount(1)">Add</button>
  </p>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

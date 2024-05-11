<script setup lang="ts">
import { ref, onMounted } from "vue";
import fetchCount from "../fetchCount";

/*
    In order to maker a prop as optional we must use:
    '?' in the prop and withDefaults. withDefault function
    needs a second parameter to set the default values
*/
interface Props {
    limit: number;
    alertMessageOnLimit?: string;
}

/*
    defineProps and withDefaults are compiler macros.
    These functions are only called during compile-time;
    they won’t appear in your runtime code. 
    That’s why we didn’t have to import them before using 
    them. These compile-time functions can only be used 
    within <script setup>.
*/
const props = withDefaults(defineProps<Props>(), {
    alertMessageOnLimit: 'This is the default value'
});

const count = ref<number | null>(null);

onMounted(() => {
  // Type inference is able to infer the parameter type if the function is used as an inline callback function.
    fetchCount((initialCount) => {
        count.value = initialCount;
    });
});

function addCount (num: number) {
    if (count.value !== null) {
        if (count.value >= props.limit) {
            alert(props.alertMessageOnLimit);
        } else {
            count.value += num;
        }
    }
}

</script>

<template>
    <div>
        <p>{{ count }}</p>
        <p>
            <button @click="addCount(1)">Add</button>
        </p>
    </div>
</template>
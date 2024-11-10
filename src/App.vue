<script setup>
import { ref } from "vue";
const appKey = "e808c4b5e05d774af9876745559360b0";
const query = ref("");
const urlBase = "https://api.openweathermap.org/data/2.5/";
const weather = ref(null);

const fetchData = async () => {
    if (query.value) {
        try {
            const response = await fetch(
                `${urlBase}weather?q=${query.value}&units=metric&appid=${appKey}`
            );

            if (!response.ok) {
                throw new Error("Failed");
            }

            const data = await response.json();
            weather.value = data;
        } catch (error) {
            console.log(error);
        } finally {
            query.value = "";
        }
    }
};
</script>

<template>
    <div
        class="h-screen bg-gradient-to-r from-cyan-500 to-blue-500 w-full flex justify-center">
        <div class="w-1/3 flex flex-col gap-2 items-center m-20">
            <div class="text-white leading-5 font-sans font-semibold">
                Weather App
            </div>

            <div class="text-white flex gap-1 items-center">
                <div class="">
                    <input
                        class="text-black h-3.5 border-x-0 rounded-sm text-sm/[1px]"
                        type="text"
                        placeholder="Enter a location"
                        v-model="query"
                        @keyup.enter="fetchData" />
                </div>
                <div>
                    <button
                        @click="fetchData"
                        class="text-center rounded-sm w-10 h-5 text-sm font-sans font-semibold">
                        Enter
                    </button>
                </div>
            </div>
            <div v-if="weather" class="font-sans font-semibold text-white">
                <h2>
                    {{ weather.name }}
                </h2>
                <p>{{ Math.floor(weather.main.temp) }} ℃</p>
            </div>
        </div>
    </div>
</template>

<style scoped></style>

<script setup>
import { ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import ShadowDomTestbed from "./components/ShadowDomTestbed.vue";

const activeTab = ref("home");
</script>

<template>
    <nav class="tab-nav">
        <button
            :class="{ active: activeTab === 'home' }"
            @click="activeTab = 'home'"
        >
            Home
        </button>
        <button
            :class="{ active: activeTab === 'shadow' }"
            @click="activeTab = 'shadow'"
        >
            Shadow DOM Testbed
        </button>
    </nav>

    <template v-if="activeTab === 'home'">
        <div>
            <a href="https://vitejs.dev" target="_blank">
                <img src="/vite.svg" class="logo" alt="Vite logo" />
            </a>
            <a href="https://vuejs.org/" target="_blank">
                <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
            </a>
        </div>
        <HelloWorld msg="Vite + Vue" />
    </template>

    <ShadowDomTestbed v-else-if="activeTab === 'shadow'" />
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

.tab-nav {
    display: flex;
    gap: 8px;
    justify-content: center;
    padding: 16px;
    border-bottom: 1px solid #2a2a3a;
    margin-bottom: 24px;
}

.tab-nav button {
    font-family: inherit;
    background: transparent;
    border: 1px solid #2a2a3a;
    color: #888;
    padding: 8px 20px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 0.85rem;
    transition: all 0.15s;
}

.tab-nav button.active {
    border-color: #00e5ff;
    color: #00e5ff;
    background: rgba(0, 229, 255, 0.06);
}

.tab-nav button:hover:not(.active) {
    border-color: #555;
    color: #ccc;
}
</style>

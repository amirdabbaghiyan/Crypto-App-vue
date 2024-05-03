<template>
    <div class="pagination">
        <button
        @click="previousHandler"
        :class="{ disable : page === 1 }">Previous</button>
        
        <p :class="{ selected : page === 1 }" @click="selectNum">{{ 1 }}</p>
        
        <p :class="{ selected : page === 2 }" @click="selectNum">{{ 2 }}</p>
        
        <template v-if="page > 2 && page < 9">
            <span>...</span>
            <p :class="{ selected : page }">{{ page }}</p>
        </template>
        
        <span>...</span>
        
        <p :class="{ selected : page === 9 }" @click="selectNum">{{ 9 }}</p>
        
        <p :class="{ selected : page === 10 }" @click="selectNum">{{ 10 }}</p>
        
        <button
        @click="nextHandler"
        :class="{ disable : page === 10 }">Next</button>
    </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const page = ref(1);

const previousHandler = () => {
    if (page.value <= 1) return;
    page.value --;
    sendPage();
};

const nextHandler = () => {
    if (page.value >= 10) return;
    page.value ++;
    sendPage()
};

const selectNum = (e) => {
    page.value = Number(e.target.textContent);
    sendPage()
}

const emit = defineEmits(['page']);
const sendPage = () => {
    emit('page', page.value)
}
</script>

<style>
.pagination {
    width: 400px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: auto;
    margin-bottom: 100px;
}

.pagination button {
    width: 80px;
    background-color: var(--primary-color);
    color: var(--text-color);
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    font-size: 1rem;
    cursor: pointer;
}

.pagination p {
    border: 1px solid var(--primary-color);
    width: 25px;
    text-align: center;
    border-radius: 5px;
    cursor: pointer;
}

.disable {
    opacity: 0.3;
}

.selected {
    background-color: var(--primary-color);
}
</style>
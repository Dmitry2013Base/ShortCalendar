<template>
    <div class="month-container">
        <div class="info">
            <input type="text" v-model="selectDate" name="date" class="date" readonly>
            <select name="lang" v-model="selectLang" class="lang">
                <option v-for="lang in languages" :key="lang" :value="lang">{{ lang }}</option>
            </select>
        </div>
        <Month :language="selectLang" @changeDate="selectDate = new Date($event).toLocaleDateString()"/>
    </div>
</template>

<script>
    import Month from "@/components/Month.vue";
    import { languages } from "@/data";

    export default {
        data: () => ({
            languages: languages,
            selectLang: 'ru',
            selectDate: '',
        }),
        components: { Month }
    }
</script>

<style scoped>
    .month-container {
        border: 1px solid #000;
        padding: 10px;
    }

    .info {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 5px;
    }

    .date,
    .lang {
        font-size: 16px;
    }
</style>

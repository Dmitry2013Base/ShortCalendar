<template>
    <div class="month">
        <div class="month-header">
            <div class="left-arrow" @click="getNextMonth(false)"></div>
            <p>{{ getLang.months[viewDate.getMonth()] }} {{ viewDate.getFullYear() }}</p>
            <div class="right-arrow" @click="getNextMonth(true)"></div>
        </div>
        <div class="day-list">
            <div
                v-for="weekday in getLang.weekdays"
                :key="weekday"
                class="weekday">
                {{ weekday }}
            </div>
            <div
                v-for="(day, index) in getMonth"
                :key="index"
                :class="[
                    'weekday',
                    (day !== '') && 'day',
                    (viewDate.getFullYear() === currentDate.getFullYear() && viewDate.getMonth() === currentDate.getMonth() && currentDate.getDate() === Number(day)) && 'current'
                ]"
                @click="$emit('changeDate', `${viewDate.getFullYear()}-${viewDate.getMonth() + 1}-${day}`)">
                {{ day }}
            </div>
        </div>
    </div>
</template>

<script>
import { months, weekdays } from '@/data';

    export default {
        data: () => ({
            currentDate: new Date(),
            viewDate: new Date(),
        }),
        props: {
            language: {
                type: String,
                default: 'ru'
            },
            date: {
                type: String,
                default: ''
            }
        },
        mounted() {
            if (this.date !== '') {
                this.viewDate = new Date(this.date);
            }
        },
        methods: {
            getNextMonth(direction) {
                let year = this.viewDate.getFullYear();
                let month = this.viewDate.getMonth();
                let nextMonth = (direction) ? ((Number(month) + 1 > 11) ? 0 : Number(month) + 1) : ((Number(month) - 1 < 0) ? 11 : Number(month) - 1);
                let nextYear = (Math.abs(month - nextMonth) !== 1) ? Number(year) + ((direction) ? 1 : -1) : year;

                this.viewDate = new Date(`${nextYear}-${nextMonth + 1}`);
            },
            getFirstDayOfMonth(year, month) {
                return new Date(year, month, 1);
            },
            getLastDayOfMonth(year, month) {
                return new Date(year, month + 1, 0);
            },
        },
        computed: {
            getLang() {
                let wds = weekdays[this.language];
                let mth = months[this.language];

                if (typeof(wds) === 'undefined') {
                    wds = Object.values(weekdays)[0];
                }

                if (typeof(mth) === 'undefined') {
                    mth = Object.values(months)[0];
                }
                return { weekdays: wds, months: mth };
            },
            getMonth() {
                let year = this.viewDate.getFullYear();
                let month = this.viewDate.getMonth();
                let weekday = this.getFirstDayOfMonth(year, month).getDay();

                return [
                    ...Array((weekday === 0) ? 6 : weekday - 1).fill(''),
                    ...Array.from({ length: this.getLastDayOfMonth(year, month).getDate() }, (_, i) => `${i + 1}`),
                ]
            }
        }
    }
</script>

<style scoped>
    .month {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 10px 0;
    }

    .month-header {
        width: 100%;
        font-size: 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 10px;
        padding: 0 10px;
    }

    .left-arrow,
    .right-arrow {
        border: 12px solid transparent;
        display: inline-flex;
        cursor: pointer;
    }

    .left-arrow {
        border-right: 12px solid #000;
        border-left: none;
    }

    .right-arrow {
        border-left: 12px solid #000;
        border-right: none;
    }
    
    .left-arrow:hover {
        border-right: 12px solid #0000ff;
    }

    .right-arrow:hover {
        border-left: 12px solid #0000ff;
    }

    .day-list {
        width: 100%;
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        grid-template-rows: repeat(7, 1fr);
        gap: 2px;
        aspect-ratio: 1 / 1;
    }

    .weekday {
        font-size: 16px;
        font-weight: 600;
        display: flex;
        justify-content: center;
        align-items: center;
        pointer-events: none;
        border: 1px solid transparent;
    }

    .day {
        font-weight: 400;
        pointer-events: fill;
    }

    .day:hover {
        border-color: #0000ff;
        cursor: pointer;
    }

    .current {
        border: 1px solid #0000ff;
    }
</style>

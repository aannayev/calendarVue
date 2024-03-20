<template>
    <div class="calendar">
        <div class="header">
            <button @click="previousMonth">&lt;</button>
            <span>{{ currentMonth }}</span>
            <button @click="nextMonth">&gt;</button>
        </div>
        <div class="weekdays">
            <span
                v-for="day in weekdays"
                :key="day"
            >{{ day }}</span>
        </div>
        <div class="days">
            <span
                v-for="blank in firstDayOfMonth"
                :key="blank"
            ></span>
            <span
                v-for="day in daysInMonth"
                :key="day"
                @click="selectDate(day)"
                :class="{ 'selected': isSelected(day) }"
            >
                {{ day }}
            </span>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        initialDate: {
            type: String,
            default: null
        },
        language: {
            type: String,
            default: 'en'
        }
    },
    data() {
        return {
            currentDate: new Date(),
            selectedDate: null,
            weekdays: [],
            months: []
        };
    },
    computed: {
        currentMonth() {
            return `${this.months[this.currentDate.getMonth()]} ${this.currentDate.getFullYear()}`;
        },
        firstDayOfMonth() {
            const firstDay = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth(), 1).getDay();
            return Array(firstDay).fill('');
        },
        daysInMonth() {
            const daysInMonth = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth() + 1, 0).getDate();
            return Array.from({ length: daysInMonth }, (_, i) => i + 1);
        }
    },
    methods: {
        initializeWeekdaysAndMonths() {
            if (this.language === 'en') {
                this.weekdays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
                this.months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
            } else if (this.language === 'ru') {
                this.weekdays = ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'];
                this.months = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'];
            }
        },
        previousMonth() {
            this.currentDate = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth() - 1, 1);
        },
        nextMonth() {
            this.currentDate = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth() + 1, 1);
        },
        selectDate(day) {
            this.selectedDate = new Date(this.currentDate.getFullYear(), this.currentDate.getMonth(), day);
            this.$emit('date-selected', this.selectedDate);
        },
        isSelected(day) {
            if (!this.selectedDate) return false;
            return (
                this.selectedDate.getFullYear() === this.currentDate.getFullYear() &&
                this.selectedDate.getMonth() === this.currentDate.getMonth() &&
                this.selectedDate.getDate() === day
            );
        }
    },
    watch: {
        language() {
            this.initializeWeekdaysAndMonths();
        }
    },
    mounted() {
        if (this.initialDate) {
            this.selectedDate = new Date(this.initialDate);
            this.currentDate = new Date(this.initialDate);
        }
        this.initializeWeekdaysAndMonths();
    }
};
</script>

<style scoped>
.calendar {
    width: 300px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-family: Arial, sans-serif;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
}

.weekdays {
    display: flex;
    justify-content: space-between;
    padding: 5px 10px;
}

.weekdays span {
    width: calc(100% / 7);
    text-align: center;
}

.days {
    display: flex;
    flex-wrap: wrap;
    padding: 5px 10px;
}

.days span {
    width: calc(100% / 7);
    text-align: center;
    padding: 5px 0;
    cursor: pointer;
}

.days span.selected {
    background-color: #007bff;
    color: #fff;
}
</style>
<script setup>
import { onMounted, ref, useTemplateRef } from 'vue';

const currentMonthYear = ref("")

const days_grid = useTemplateRef("daysGrid");
const popup = useTemplateRef("popup");
const showPopup = ref(false);


//to be changed
const prevMonthBtn = document.getElementById('prevMonth');
const nextMonthBtn = document.getElementById('nextMonth');
let     currentDate = new Date(); // Stores the currently displayed month/year


const renderCalendar = () => {
    // Clear previous days
    days_grid.value.innerHTML = '';

    // Get current month and year from the `currentDate` object
    const currentMonth = currentDate.getMonth();
    const currentYear = currentDate.getFullYear();

    // Set the month and year display in the header
    currentMonthYear.value = currentDate.toLocaleDateString('en-US', {
        month: 'long',
        year: 'numeric'
    });

    // Get the first day of the current month (e.g., July 1, 2025)
    const firstDayOfMonth = new Date(currentYear, currentMonth, 1);
    // Get the day of the week for the first day (0 for Sunday, 1 for Monday, etc.)
    const startDayOfWeek = firstDayOfMonth.getDay(); 

    // Get the last day of the current month (e.g., July 31, 2025)
    const lastDayOfMonth = new Date(currentYear, currentMonth + 1, 0);
    // Get the number of days in the current month
    const daysInMonth = lastDayOfMonth.getDate();

    // Get the last day of the previous month
    const lastDayOfPrevMonth = new Date(currentYear, currentMonth, 0);
    const daysInPrevMonth = lastDayOfPrevMonth.getDate();

    // Get today's actual date (to highlight)
    const today = new Date();
    const todayDay = today.getDate();
    const todayMonth = today.getMonth();
    const todayYear = today.getFullYear();

    // 1. Add inactive days from the end of the previous month
    // `startDayOfWeek` tells us how many empty cells (or prev month days) we need
    for (let i = startDayOfWeek; i > 0; i--) {
        const dayDiv = document.createElement('div');
        dayDiv.classList.add('inactive');
        dayDiv.textContent = daysInPrevMonth - i + 1;
        days_grid.value.appendChild(dayDiv);
    }

    // 2. Add days of the current month
    for (let i = 1; i <= daysInMonth; i++) {
        const dayDiv = document.createElement('div');
        dayDiv.classList.add('day');
        dayDiv.id = `${currentYear}:${currentMonth}:${i}`;
        dayDiv.textContent = i;

        // Check if this day is today's date
        if (
            i === todayDay &&
            currentMonth === todayMonth &&
            currentYear === todayYear
        ) {
            dayDiv.classList.add('today');
        }
        days_grid.value.appendChild(dayDiv);
    }

    // 3. Add inactive days from the start of the next month
    // We need a total of 42 cells (6 rows * 7 days) to ensure consistent calendar layout
    // Or simply fill until a new row starts
    const totalCells = days_grid.value.children.length;
    const remainingCells = 42 - totalCells; // Max 6 rows * 7 days = 42 cells
                                           // Min 5 rows * 7 days = 35 cells
    const daysToAdd = remainingCells < 7 ? (7 - (totalCells % 7)) % 7 : remainingCells;


    for (let i = 1; i <= daysToAdd; i++) {
        const dayDiv = document.createElement('div');
        dayDiv.classList.add('inactive');
        dayDiv.textContent = i;
        days_grid.value.appendChild(dayDiv);
    }
};

const prevMonth = () => {
    currentDate.setMonth(currentDate.getMonth() - 1); // Go to previous month
    renderCalendar();
}

const nextMonth = () => {
    currentDate.setMonth(currentDate.getMonth() + 1); // Go to next month
    renderCalendar();
}

const id = ref(null);
const setEvent=(e)=>{
    if(e.target.classList.contains('day')){
        id.value = e.target.id;
       renderPopup(e.pageX,e.pageY);
       console.log(e)
    }
}

const renderPopup=(x,y)=>{ 
    console.log(popup.value)
    popup.value.style.left = `${x}px`;
    popup.value.style.top = `${y}px`;
    showPopup.value = true;
   
}


onMounted(()=>{
   renderCalendar();        
})

       

</script>

<template>
     <div class="calendar-container">
        <div class="calendar-header">
            <div class="current-month-year bold">
                {{ currentMonthYear }}
            </div>
            <div class="calendar-controls bold">
            <span @click="prevMonth" class="nav-icon material-symbols-outlined">chevron_left</span>
            <span @click="nextMonth" class="nav-icon material-symbols-outlined">chevron_right</span>
        </div>
        </div>

        <div class="calendar-grid">
            <div class="weekdays has-text-weight-bold is-size-5">
                <div>Sun</div>
                <div>Mon</div>
                <div>Tue</div>
                <div>Wed</div>
                <div>Thu</div>
                <div>Fri</div>
                <div>Sat</div>
            </div>
            <div @click="setEvent($event)" class="days" ref="daysGrid">
                
            </div>
        </div>
    </div>
    <div v-show="showPopup" ref="popup" class="popup">
        <div class="popup-content">
            <div class="date" style="padding-bottom: 8px;" ><span>{{ id }}</span> <span @click="showPopup=false" style="float: inline-end; color: red; cursor:pointer">X</span> </div>
            <input type="text" placeholder="Event">
            <button>Add Event</button>
        </div>
    </div>

</template>

<style>
        /*
         * CSS for the Centered Calendar
         */
        
        .calendar-container {
            background-color: var(--calendar-bg);
            border: 1px solid var(--calendar-border-color);
            border-radius: 12px;
            box-shadow: 0 10px 25px var(--calendar-shadow);
            width: 100%;
            max-width: 500px; /* Max width for the calendar card */
            overflow: hidden; /* Ensures border-radius applies nicely */
            padding: 1.5rem 1rem;
        }

        .calendar-header {
            color: var(--color-dark) !important;
            padding: 15px 20px;
            display: flex;
            font-size: 1.5rem;
            justify-content: space-between;
            align-items: center;
            text-transform: capitalize; /* For month name */
        }

        .calendar-header .nav-icon {
            font-family: 'Material Symbols Outlined';
            font-size: 1.5rem;
            font-weight: 400;
            cursor: pointer;
            user-select: none;
            color: var(--icon-color);
        }
        .calendar-controls {
            display: flex;
            align-items: center;
            justify-content: end;
            gap: .5rem;
        }
        .calendar-grid {
            padding: 20px;
        }

        .weekdays, .days {
            display: grid;
            grid-template-columns: repeat(7, 1fr); /* 7 equal columns */
            text-align: center;
            justify-items: center;
        }

        .weekdays div {
            font-weight: 500;
            color: var(--color-dark);
            padding: 10px 0;
            font-size: 0.9em;
            text-transform: uppercase;
        }

        .days div {
            padding: 10%;
            border-radius: 50%; /* Make day numbers circular */
            display: flex;
            width: 2rem;
            height: 2rem;
            justify-content: center;
            align-items: center;
            font-weight: 400;
            cursor: pointer;
            transition: background-color 0.2s ease, color 0.2s ease;
        }

        .days div.inactive {
            color: var(--inactive-day-color);
            cursor: default;
        }

        .days div.today {
            background-color: var(--today-background);
            color: var(--today-text);
            font-weight: 700;
        }

        .days div:not(.inactive):hover {
            background-color: rgba(116, 74, 255, 0.411);
            color: var(--header-bg);
        }
        
        /* re-usable styles */
        .bold{
            font-weight: 700;
            letter-spacing: .1rem;
        }

        /* Basic Responsive adjustment */
        @media (max-width: 480px) {

            .calendar-container {
                margin: 20px;
                max-width: 400px;
            }
            .calendar-header {
                font-size: 1.1em;
                padding: 12px 15px;
            }
            .weekdays div, .days div {
                font-size: 0.85em;
                padding: 8px 0;
            }
            .days div {
                padding: 10%;
                border-radius: 50%; /* Make day numbers circular */
                display: flex;
                width: 2rem;
                height: 2rem;
            }
        }    
</style>
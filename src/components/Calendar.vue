<script setup>

  // Optional: A small piece of JS to set the current date dynamically
        // For a full calendar, this would be much more complex.
        document.addEventListener('DOMContentLoaded', () => {
            const date = new Date();
            const options = { year: 'numeric', month: 'long' };
            const currentMonthYear = date.toLocaleDateString('en-US', options);
            document.querySelector('.current-month-year').textContent = currentMonthYear;

            // Mark today's date dynamically (only if it's the current month/year)
            const today = date.getDate();
            const daysContainer = document.querySelector('.days');
            Array.from(daysContainer.children).forEach(dayElement => {
                if (!dayElement.classList.contains('inactive') && parseInt(dayElement.textContent) === today) {
                    // Remove existing 'today' class if any (e.g., from static HTML)
                    document.querySelectorAll('.days .today').forEach(el => el.classList.remove('today'));
                    dayElement.classList.add('today');
                }
            });
        });

</script>
<template>
     <div class="calendar-container">
        <div class="calendar-header">
            <div class="current-month-year bold">
                July 2025
            </div>
            <div class="calendar-controls bold">
            <span class="nav-icon material-symbols-outlined">chevron_left</span>
            <span class="nav-icon material-symbols-outlined">chevron_right</span>
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
            <div class="days">
                <div class="inactive">29</div>
                <div class="inactive">30</div>

                <div>1</div>
                <div class="today">2</div> <div>3</div>
                <div>4</div>
                <div>5</div>
                <div>6</div>
                <div>7</div>
                <div>8</div>
                <div>9</div>
                <div>10</div>
                <div>11</div>
                <div>12</div>
                <div>13</div>
                <div>14</div>
                <div>15</div>
                <div>16</div>
                <div>17</div>
                <div>18</div>
                <div>19</div>
                <div>20</div>
                <div>21</div>
                <div>22</div>
                <div>23</div>
                <div>24</div>
                <div>25</div>
                <div>26</div>
                <div>27</div>
                <div>28</div>
                <div>29</div>
                <div>30</div>
                <div>31</div>

                <div class="inactive">1</div>
                <div class="inactive">2</div>
                <div class="inactive">3</div>
                <div class="inactive">4</div>
                <div class="inactive">5</div>
                <div class="inactive">6</div>
            </div>
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
            justify-content: space-between;
            align-items: center;
            text-transform: capitalize; /* For month name */
        }

        .calendar-header .nav-icon {
            font-family: 'Material Symbols Outlined';
            font-size: 24px;
            cursor: pointer;
            user-select: none;
            color: var(--icon-color);
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
            background-color: rgba(0, 123, 255, 0.1); /* Light blue hover */
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
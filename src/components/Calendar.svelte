<script>
    import { createEventDispatcher } from 'svelte';
    import ArrowRight from '../assets/ArrowRight.svelte';
    import ArrowLeft from '../assets/ArrowLeft.svelte';
    import ToogleIcon from '../assets/ToogleIcon.svelte';

    const dispatch = createEventDispatcher()

    export let activeDate
    let month, currentDay, year, daysOfMonth, firstDay, lastDay, daysPrevMonth, daysNextMonth, datesToDisplay = []
    
    const getDaysInMonth = (date) => {
        currentDay = date.getDay();
        month = date.getMonth();
        year = date.getFullYear();
        daysOfMonth = new Date(year, month + 1, 0).getDate()
        firstDay = new Date(year, month, 1).getDay()
        lastDay = new Date(year, month, daysOfMonth).getDay()

        daysPrevMonth = firstDay > 0 ? firstDay - 1:7 -1
        daysPrevMonth = daysPrevMonth == 0 ? 7:daysPrevMonth
        daysNextMonth = lastDay < 6 ? 7- lastDay:7 - lastDay
    }

    const getPreviousDates = (number, date)  => {
        const lastDateOfMonth = new Date(date.getFullYear(), date.getMonth(), 0).getDate()
        const result = []

        for (let i = lastDateOfMonth + 1 - number; i <= lastDateOfMonth; i++) {
            result.push(i)
        }

        return result
    }

    
    $: if (activeDate) {
        getDaysInMonth(activeDate)
    
        const datesPrevMonth = getPreviousDates(daysPrevMonth, activeDate)
        const datesCurrentMonth = [...Array(daysOfMonth + 1).keys()]
        datesCurrentMonth.shift()
        
        const datesNextMonth = [...Array(daysNextMonth + 1).keys()]
        datesNextMonth.shift()
    
        datesToDisplay = [...datesPrevMonth].concat(datesCurrentMonth).concat(datesNextMonth)
    }

    
    
    




    const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']

    
</script>

<style>
    .calendar > div {
        width: 44px;
        height: 44px;
        font-size: 18px;
        font-weight: 500;
    }
    .active,.selected {
        color: #FFFFFF;
        background-color: #0047FF;
        border-radius: 100%;
    }
    .inactive {
        color: rgba(0, 23, 84, 0.15);
    }
    .month, .year {
        box-shadow: 0px 1px 1px rgba(0, 14, 51, 0.05);
        border-radius: 6px;
        padding: 10px 12px;
        width: min-content;
    }
    .day-name {
        color: #1F1F1F;
        font-size: 18px;
        font-weight: 500;
    }
</style>

{#if datesToDisplay.length}

    <div class="border border-gray-300/75 rounded-xl space-y-3.5 max-w-full md:w-426px p-6">

        <div class="flex items-center justify-between">
            <div style="width:44px;height:44px" class="grid place-content-center rounded-full shadow-sm"><ArrowLeft /></div>
            <div class="flex items-center gap-3.5 pr-3" style="color:#141414;font-weight:700;font-size:24px">
                <div class="month relative">
                    {months[month]} 
                    <div class="absolute" style="top:1.8rem;right:2px"><ToogleIcon /></div>
                </div>
                <div class="year relative">
                    {year}
                    <div class="absolute" style="top:1.8rem;right:2px"><ToogleIcon /></div>
                </div>
            </div>
            <div style="width:44px;height:44px" class="grid place-content-center rounded-full shadow-sm"><ArrowRight /></div>

        </div>

        <div class="grid grid-cols-7 place-items-center day-name">
            <div>Mo</div>
            <div>Tu</div>
            <div>We</div>
            <div>Th</div>
            <div>Fr</div>
            <div>Sa</div>
            <div>Su</div>
        </div>

        <div class="grid grid-cols-7 place-items-center calendar">
            {#each datesToDisplay as d, i (i)}
            <div class="grid place-content-center cursor-pointer i={i} daysprev={daysPrevMonth} daysofmonth={daysOfMonth}" class:inactive={i < daysPrevMonth || i >= (daysOfMonth + daysPrevMonth)} class:active={i >= daysPrevMonth && i < datesToDisplay.length - daysNextMonth && d == new Date(activeDate).getDate()} on:click={() => dispatch('select', {year, month: i < daysPrevMonth ? month-1:(i >= (daysOfMonth + daysPrevMonth) ? month+1:month), date: d})}>{d}</div>            
            {/each}
        </div>

    </div>

{/if}
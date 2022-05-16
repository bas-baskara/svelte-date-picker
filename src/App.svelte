<script>
    import { fade } from 'svelte/transition';
    import './output.css'
    import Calendar from './components/Calendar.svelte';

    export let activeDate = new Date()
    let selectedDate
    let calendar = false, posCalendar = 'bottom', wrapper

    const selectingDate = (e) => {
        const {year, month, date} = e.detail
        console.log(e.detail)

        if (month == -1) {
            activeDate = new Date(year-1, 11, date)
        } else if (month == 12) {
            activeDate = new Date(year+1, 0, date)
        } else {
            activeDate = new Date(year, month, date)
        }

        selectedDate = new Date(activeDate).toLocaleDateString('id-ID', {year: 'numeric', month: 'long', day: 'numeric'})
        
        console.log('selected date', new Date(selectedDate))
    }

    const hideCalendar = () => {
        calendar = false
    }

    const showCalendar = () => {
        const docHeight = document.documentElement.scrollHeight
        console.log(docHeight - wrapper.offsetTop)
        if (docHeight - wrapper.offsetTop < 450) posCalendar = 'top'
        else posCalendar = 'bottom'

        calendar = true
    }

	
</script>

<div class="">

    <div class="relative" style="height:45px;" bind:this={wrapper}>
        <input type="text" class="rounded-full w-full h-full bg-transparent pl-4 cursor-default" on:blur={hideCalendar} on:focus={showCalendar} on:keydown={(e) => e.preventDefault()} bind:value={selectedDate}>
        <div class="-z-10 bg-ahm-blue-1 w-min rounded-r-full grid place-content-center pb-1 absolute top-0 right-0" style="height:45px;width:54px">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect x="1" y="4" width="18" height="15" rx="2" stroke="white"/>
                <path d="M1 8C1 6.11438 1 5.17157 1.58579 4.58579C2.17157 4 3.11438 4 5 4H15C16.8856 4 17.8284 4 18.4142 4.58579C19 5.17157 19 6.11438 19 8V8H1V8Z" fill="white"/>
                <path d="M5 1L5 4" stroke="white" stroke-linecap="round"/>
                <path d="M15 1L15 4" stroke="white" stroke-linecap="round"/>
                <rect x="5" y="10" width="4" height="2" rx="0.5" fill="white"/>
                <rect x="5" y="14" width="4" height="2" rx="0.5" fill="white"/>
                <rect x="11" y="10" width="4" height="2" rx="0.5" fill="white"/>
                <rect x="11" y="14" width="4" height="2" rx="0.5" fill="white"/>
                </svg>
                
        </div>
        
        {#if calendar}
            <div transition:fade class="absolute left-4" class:top-12={posCalendar == 'bottom'} class:bottom-12={posCalendar == 'top'}><Calendar {activeDate} on:select={selectingDate} /></div>
        {/if}
    </div>
</div>


<script>
    import left from "../../../src/left.png";
    import right from "../../../src/right.png";
    import Modal from "../Modal.svelte";

    const WEEK_DAYS = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    const MONTHS = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEPT','OCT','NOV','DEC']
    export let selectedStartDate = new Date();
    export let selectedEndStartDate = new Date(2023,4,25);
    const currentDate = new Date();
    let startDate = currentDate ;
    let endDate = new Date();
    let startMonth = currentDate.getMonth() , startYear = currentDate.getFullYear() , startDay = currentDate.getDate();
    let endMonth = endDate.getMonth() + 1 , endYear = endDate.getFullYear();
    let showModal = false;
    // let selectedDatesRange = [{start :selectedStartDate,end:selectedEndStartDate}];


    function handleStartDateBackyear(){
        startDate.setMonth(startDate.getMonth() -1);
        startMonth = startDate.getMonth();
        startYear = startDate.getFullYear();
        startDay = startDate.getDate();
    }

    function handleStartDateForwardyear(){
        while(startDate < endDate){
            startDate.setMonth(startDate.getMonth() +1);
            startMonth = startDate.getMonth();
            startYear = startDate.getFullYear();
            startDay = startDate.getDate();
        }
    }

    function handleEndDateBackyear(){
        while(startDate < endDate){
            endDate.setMonth(endDate.getMonth() -1);
            endMonth = endDate.getMonth();
            endYear = endDate.getFullYear();
        }
    }

    function handleEndDateForwardyear(){
        endDate.setMonth(endDate.getMonth() +1);
        endMonth = endDate.getMonth();
        endYear = endDate.getFullYear();
    }

    function getDaysInMonth(year, month) {
        return new Date(year, month+1,0).getDate();
    }

 

function getCalendarDaysForStartDate(startYear, startMonth,startDay) {
    const daysInMonth = getDaysInMonth(startYear, startMonth);
    const firstDayOfWeek = new Date(startYear, startMonth, 1).getDay();
    const days = [];
    for(let i = 0 ;i<firstDayOfWeek;i++){
        const day = {
            dayOfWeek: firstDayOfWeek,
            isWeekend:  firstDayOfWeek === 0 || firstDayOfWeek === 6,
            dayOfMonth: 0,
            date : new Date(startYear,startMonth,i),
            endDateFlag : new Date(startYear,startMonth,i).getTime() == selectedEndStartDate.getTime(),
            startDateFlag : new Date(startYear,startMonth,i).getTime() == selectedStartDate.getTime(),
        };
        days.push(day);
    }
    
    for (let i = 1; i <= daysInMonth; i++) {
        const dayOfWeek = new Date(startYear, startMonth, i).getDay();
        const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
        const day = {
            dayOfWeek: dayOfWeek,
            isWeekend: isWeekend,
            dayOfMonth: i,
            date : new Date(startYear , startMonth,i),
            endDateFlag : new Date(startYear,startMonth,i).getTime() == selectedEndStartDate.getTime(),
            startDateFlag : new Date(startYear,startMonth,i).getTime() == selectedStartDate.getTime(),

          //  color : (new Date(startYear,startMonth,i) >= selectedStartDate  &&  new Date(startYear,startMonth,i) <=selectedEndStartDate),
        };
        days.push(day);
    }
    return days;
}


function getCalendarDaysForEndDate(endYear, endMonth) {
    const daysInMonth = getDaysInMonth(endYear, endMonth);
    const firstDayOfWeek = new Date(endYear, endMonth, 1).getDay();
    const days = [];
    for(let i = 0 ;i<firstDayOfWeek;i++){
        const day = {
        dayOfWeek: firstDayOfWeek,
        isWeekend:  firstDayOfWeek === 0 || firstDayOfWeek === 6,
        dayOfMonth: 0 ,
        date : new Date(endYear,endMonth,i),
        startDateFlag : new Date(endYear,endMonth,i).getTime() == selectedStartDate.getTime(),
        endDateFlag : new Date(endYear,endMonth,i).getTime() == selectedEndStartDate.getTime(),

     //   color : false,
      };
      days.push(day);
    }
    for (let i = 1; i <= daysInMonth; i++) {
      const dayOfWeek = new Date(endYear, endMonth, i).getDay();

      const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
      const day = {
        dayOfWeek: dayOfWeek,
        isWeekend: isWeekend,
        dayOfMonth: i,
        date : new Date(endYear , endMonth,i),
        startDateFlag : new Date(endYear,endMonth,i).getTime() == selectedStartDate.getTime(),
        endDateFlag : new Date(endYear,endMonth,i).getTime() == selectedEndStartDate.getTime(),

     //   color : (new Date(endYear,endMonth,i) >= selectedStartDate  &&  new Date(endYear,endMonth,i) <=selectedEndStartDate),
    };
      days.push(day);
    }
    return days;
}

function calendarStartHandle(day){
    if(selectedStartDate < selectedEndStartDate){
        if(selectedStartDate > day.date && day.date < selectedEndStartDate){
            selectedStartDate = day.date;
            getCalendarDaysForStartDate(selectedStartDate.getFullYear(),selectedStartDate.getMonth(),selectedStartDate.getDate());
        }else if(day.date > selectedStartDate && day.date < selectedEndStartDate){
            selectedStartDate = day.date;
            getCalendarDaysForStartDate(selectedStartDate.getFullYear(),selectedStartDate.getMonth(),selectedStartDate.getDate());
            
        }

        if(selectedEndStartDate < day.date && selectedStartDate < day.date){
            selectedEndStartDate = day.date;
           getCalendarDaysForEndDate(selectedEndStartDate.getFullYear(),selectedEndStartDate.getMonth(),selectedEndStartDate.getDate());
        }
    }
}


</script>
<div>
    <button class="calendar-button" on:click={() => (showModal = true)} type="button">
        <i class="fa fa-calendar"></i>
        <span>{selectedStartDate.getDate()} {MONTHS[selectedStartDate.getMonth()]} {selectedStartDate.getFullYear()} - {selectedEndStartDate.getDate()} {MONTHS[selectedEndStartDate.getMonth()]} {selectedStartDate.getFullYear()}</span>
        <i class="fa fa-angle-down"></i>
</button>

<Modal bind:showModal>

<div class="main-container">
    <div class="left-calendar">
        <div class="left-heading">
            <div class="left-month-year">
                <button class="left-month-button">{MONTHS[startMonth]} {startYear}</button>
            </div>
            <div class="left-month-left-right-arrow">
                <img src={left} alt="left" on:click={handleStartDateBackyear}/>
                <img src={right} alt="right"  on:click={handleStartDateForwardyear}/>
            </div>
        </div>

        <div class="left-weekdays">
            {#each WEEK_DAYS as day} 
                <div class="weekday">{day}</div>
            {/each}
        </div>

        <div class="left-days">
            {#each getCalendarDaysForStartDate(startYear, startMonth , startDay) as day, i} 
            {#if day.startDateFlag == true || day.endDateFlag == true}
            <div key={day.date} class="day selected"  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>
            {:else if day.date > selectedStartDate && day.date < selectedEndStartDate} 
            <div key={day.date} class={day.date > selectedStartDate && day.date < selectedEndStartDate && day.dayOfMonth !=0 ? "day selected-range" : "day"}  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>
            {:else}
            <div key={day.date} class="day"  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>

            {/if}
                <!-- <div key={day.date} on:click={calendarStartHandle(day)} class={day.date >= selectedStartDate && day.date <= selectedEndStartDate ? "day selected" : "day"}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div> -->
            {/each}
        </div>


    </div>


    <div class="left-calendar">
        <div class="left-heading">
            <div class="left-month-year">
                <button class="left-month-button">{MONTHS[endMonth]} {endYear}</button>
            </div>
            <div class="left-month-left-right-arrow">
                <img src={left} alt="left" on:click={handleEndDateBackyear}/>
                <img src={right} alt="right"  on:click={handleEndDateForwardyear}/>
            </div>
        </div>

        <div class="left-weekdays">
            {#each WEEK_DAYS as day} 
                <div class="weekday">{day}</div>
            {/each}
        </div>

        <div class="left-days">
            {#each getCalendarDaysForEndDate(endYear, endMonth) as day, i} 
            {#if day.startDateFlag == true || day.endDateFlag == true}
            <div key={day.date} class="day selected"  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>

            {:else if day.date > selectedStartDate && day.date < selectedEndStartDate} 
            <div key={day.date} class={day.date > selectedStartDate && day.date < selectedEndStartDate && day.dayOfMonth != 0 ? "day selected-range" : "day"}  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>
            {:else}
            <div key={day.date} class="day"  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div>

            {/if}
            <!-- <div key={day.date} class={day.date >= selectedStartDate && day.date <= selectedEndStartDate ? "day selected" : "day"}  on:click={calendarStartHandle(day)}>{day.dayOfMonth == 0 ? ' ' : day.dayOfMonth}</div> -->
            {/each}
        </div>


    </div>
</div>
</Modal>


</div>
<style>
    .calendar-button {
        padding: 10px 20px;
        border: 1px solid black;
        display: flex;
        gap: 2rem;
        text-align: center;
        width: var(--button-width);
        text-decoration: none;
        cursor: pointer;
        background: #FFFFFF;
        color: green;
        border-radius: 7px;
        box-shadow: 0px 0px 3px rgba(0, 0, 0, 0.1);
        margin-left: 20rem;
        margin-top: 3rem;
    }
    .main-container {
        display: grid;
        justify-content: center;
        width: 100%;
        background: #FFFFFF;
        grid-template-columns: repeat(2 , 400px);
     }

    .left-calendar {
        width: 350px;
    }

    .left-heading {
        height: 24px;
        left: 0px;
        right: 0px;
        top: 0px;
        display: grid;
        grid-template-columns: repeat(2 , 1fr);
    }

    .left-month-year {
        justify-content: center;
        align-items: center;
        padding: 10px 8px;
        top: 0px;
        border-radius: 6px;
    }

    .left-month-button {
        width: fit-content;
        height: 21px;
        font-family: 'Inter';
        font-style: normal;
        font-weight: 500;
        font-size: 19px;
        line-height: 115%;
        color: #2D3748;
        border: none; 
        background: transparent;
        border-radius: 6px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        padding: 10px 8px;
        gap: 4px;
        width: 200px;
        height: 24px;
    }
    
    .left-month-left-right-arrow{
        display: flex;
        flex-direction: row;
        justify-content: flex-end;
        align-items: center;
        padding: 0px;
        gap: 8px;
    }

    .left-weekdays {
        display: grid;
        justify-content: space-between;
        grid-template-columns: repeat(7,1fr);

    }

    img {
        width: 40px;
        height: 40px;
    }

    .weekday {
        color: #718096;
        font-size: 14px;
        font-weight: bold;
        text-align: center;
        padding: 10px;
        width: calc(100% / 7);
        margin-top: 3rem;
    }

    .left-days {
        display: grid;
        grid-template-columns: repeat(7,1fr);
        justify-content: space-between;
    }

    .day {
        background-color: #fff;
        color: #2D3748;
        font-size: 14px;
        padding: 10px;
        text-align: center;
        font-family: 'Inter';
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 150%;
    }

    .day:hover {
        /* background-color: transparent; */
        cursor: pointer;
    }

    img:hover {
        background-color: #f7f7f7;
        cursor: pointer;
    }

    .selected {
        background-color: #e40046;
        color : #FFFFFF;
    }

    .selected-range {
        background-color: #ffe4ec;
        color: #FFFFFF;
    }

</style>
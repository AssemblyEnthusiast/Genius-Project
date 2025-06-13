<script>
    import { onMount } from 'svelte';

    let exercises = [
        { id: 1, name: 'Running' },
        { id: 2, name: 'Cycling' },
        { id: 3, name: 'Jump Rope' },
        { id: 4, name: 'Rowing' },
        { id: 5, name: 'Swimming' }
    ];

    let week = [
        { day: 'Sunday', exercises: [] },
        { day: 'Monday', exercises: [] },
        { day: 'Tuesday', exercises: [] },
        { day: 'Wednesday', exercises: [] },
        { day: 'Thursday', exercises: [] },
        { day: 'Friday', exercises: [] },
        { day: 'Saturday', exercises: [] }
    ];

    let draggedExercise = null;

    function handleDragStart(exercise) {
        draggedExercise = exercise;
    }

    function handleDrop(dayIndex) {
        if (draggedExercise) {
            // Prevent duplicates in the same day
            if (!week[dayIndex].exercises.find(e => e.id === draggedExercise.id)) {
                week[dayIndex].exercises = [...week[dayIndex].exercises, draggedExercise];
            }
            draggedExercise = null;
        }
    }

    function handleDragOver(event) {
        event.preventDefault();
    }

    function removeExercise(dayIndex, exerciseId) {
        week[dayIndex].exercises = week[dayIndex].exercises.filter(e => e.id !== exerciseId);
    }
</script>

<style>
    .container {
        display: flex;
        gap: 2rem;
        padding: 2rem;
        flex-wrap: wrap;
    }
    .exercises, .calendar {
        background: #f8f8f8;
        border-radius: 8px;
        padding: 1rem;
        min-width: 220px;
        box-shadow: 0 2px 8px #0001;
    }
    .exercises {
        flex: 1 1 200px;
        max-width: 250px;
    }
    .exercise {
        background: #e0f7fa;
        margin: 0.5rem 0;
        padding: 0.5rem 1rem;
        border-radius: 4px;
        cursor: grab;
        user-select: none;
        transition: background 0.2s;
    }
    .calendar {
        flex: 3 1 600px;
        display: flex;
        gap: 1rem;
        overflow-x: auto;
    }
    .day {
        flex: 1 1 120px;
        min-width: 120px;
        background: #fff;
        border-radius: 6px;
        padding: 0.5rem;
        border: 1px solid #ddd;
        min-height: 120px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .day h3 {
        margin: 0 0 0.5rem 0;
        font-size: 1.1rem;
        color: #1976d2;
    }
    .day .exercise {
        background: #ffe082;
        margin: 0.3rem 0;
        cursor: default;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .remove-btn {
        background: none;
        border: none;
        color: #d32f2f;
        font-size: 1.1rem;
        cursor: pointer;
        margin-left: 0.5rem;
    }
</style>

<div class="container">
    <div class="exercises">
        <h2>Cardio Exercises</h2>
        {#each exercises as exercise}
            <div
                class="exercise"
                draggable="true"
                on:dragstart={() => handleDragStart(exercise)}
            >
                {exercise.name}
            </div>
        {/each}
    </div>
    <div class="calendar">
        {#each week as day, i}
            <div
                class="day"
                on:dragover={handleDragOver}
                on:drop={() => handleDrop(i)}
            >
                <h3>{day.day}</h3>
                {#each day.exercises as ex}
                    <div class="exercise">
                        {ex.name}
                        <button class="remove-btn" on:click={() => removeExercise(i, ex.id)} title="Remove">×</button>
                    </div>
                {/each}
            </div>
        {/each}
    </div>
</div>
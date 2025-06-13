
<script>
    // Miles Tracker logic  
    let entries = [];
    let date = '';
    let miles = '';

    // Calories Calculator logic
    let weight = '';
    let exercise = 'running';
    let time = '';
    let calories = null;

    const exerciseMETs = {
        running: 9.8,
        walking: 3.8,
        cycling: 7.5,
        swimming: 8.0
    };

    function addEntry() {
        if (!date || !miles || miles < 0) return;
        entries = [...entries, { date, miles: parseFloat(miles) }];
        date = '';
        miles = '';
    }

    function getMonthKey(dateStr) {
        const d = new Date(dateStr);
        return d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0');
    }

    $: monthlyTotals = (() => {
        const totals = {};
        for (const entry of entries) {
            const month = getMonthKey(entry.date);
            totals[month] = (totals[month] || 0) + entry.miles;
        }
        return totals;
    })();

    function calculateCalories() {
        if (!weight || !time || time <= 0) {
            calories = null;
            return;
        }
        const met = exerciseMETs[exercise] || 1;
        // Calories burned per minute = (MET * 3.5 * weight in kg) / 200
        // Total = above * minutes
        calories = ((met * 3.5 * parseFloat(weight)) / 200) * parseFloat(time);
    }
</script>

<h2>Miles Run Tracker</h2>
<form on:submit|preventDefault={addEntry}>
    <label>
        Date:
        <input type="date" bind:value={date} required>
    </label>
    <label>
        Miles:
        <input type="number" bind:value={miles} min="0" step="0.01" required>
    </label>
    <button type="submit">Add Entry</button>
</form>
<h3>Entries</h3>
<table>
    <thead>
        <tr>
            <th>Date</th>
            <th>Miles</th>
        </tr>
    </thead>
    <tbody>
        {#each entries as entry}
            <tr>
                <td>{entry.date}</td>
                <td>{entry.miles.toFixed(2)}</td>
            </tr>
        {/each}
    </tbody>
</table>
<h3>Monthly Totals</h3>
<table>
    <thead>
        <tr>
            <th>Month</th>
            <th>Total Miles</th>
        </tr>
    </thead>
    <tbody>
        {#each Object.keys(monthlyTotals).sort() as month}
            <tr>
                <td>{month}</td>
                <td>{monthlyTotals[month].toFixed(2)}</td>
            </tr>
        {/each}
    </tbody>
</table>

<!-- Calories Burned Calculator -->
<h2>Calories Burned Calculator</h2>
<form on:input={calculateCalories} style="margin-bottom: 1em;">
    <label>
        Weight (kg):
        <input type="number" bind:value={weight} min="1" step="0.1" required>
    </label>
    <label>
        Exercise:
        <select bind:value={exercise}>
            <option value="running">Running</option>
            <option value="walking">Walking</option>
            <option value="cycling">Cycling</option>
            <option value="swimming">Swimming</option>
        </select>
    </label>
    <label>
        Time (minutes):
        <input type="number" bind:value={time} min="1" step="1" required>
    </label>
</form>
{#if calories !== null}
    <div>
        <strong>Calories Burned:</strong> {calories.toFixed(2)}
    </div>
{/if}

<style>
    body { font-family: Arial, sans-serif; margin: 2em; }
    label, input, select, button { margin: 0.5em 0; display: block; }
    table { border-collapse: collapse; margin-top: 1em; }
    th, td { border: 1px solid #ccc; padding: 0.5em 1em; }
    form { margin-bottom: 1em; }
</style>

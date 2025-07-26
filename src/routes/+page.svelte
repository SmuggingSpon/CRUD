<script>
    let people = $state([
        {first: "Cristiano", last: "Ronaldo"},
        {first: "Lionel", last: "Messi"},
        {first: "Harry", last: "Potter"}
    ]);

    let prefix = $state('');
    let first = $state('');
    let last = $state('');
    let i = $state(0);

    let filteredPeople = $derived(
        prefix ? people.filter((person) => {
            const name = `${person.last}, ${person.first}`;
            return name.toLowerCase().startsWith(prefix.toLowerCase());
        }) : people
    );
    let selected = $derived(filteredPeople[i]);
    
    function create(){
        people = people.concat({first, last});
        i = people.length - 1;
        first = last = '';
    }

    function update(){
        selected.first = first;
        selected.last = last;
        people = people;
    }

    function remove(){
        const index = people.indexOf(selected);
        people = [...people.slice(0, index), ...people.slice(index + 1)];

        first = last = '';
        i = Math.min(i, filteredPeople.length - 2);
    }

    function reset_inputs(person){
        first = person ? person.first : '';
        last = person ? person.last : '';
    }

    $effect(() => {
        reset_inputs(selected);
    });
</script>

<div class="flex items-center justify-center h-screen bg-gray-100">
    <div class="flex flex-col items-start p-12 bg-white rounded-2xl shadow-lg space-y-6 text-lg">
        <input placeholder="filter prefix" bind:value={prefix} class="w-60 p-3 border rounded">

        <div class="flex flex-row gap-6">
            <select bind:value={i} size={6} class="w-60 p-3 border rounded">
                {#each filteredPeople as person, i}
                    <option value={i}>{person.last}, {person.first}</option>
                {/each}
            </select>

            <div class="flex flex-col gap-4">
                <input class="w-60 p-3 border rounded" placeholder="first" bind:value={first}>
                <input class="w-60 p-3 border rounded" placeholder="last" bind:value={last}>
            </div>
        </div>

        <div class="flex flex-row gap-4 pt-4">
            <button class="bg-green-500 text-white px-6 py-3 rounded shadow hover:bg-green-600 disabled:opacity-50 disabled:cursor-not-allowed" onclick={create} disabled={!first || !last}>
                Create
            </button>
            <button class="bg-blue-500 text-white px-6 py-3 rounded shadow hover:bg-blue-600 disabled:opacity-50 disabled:cursor-not-allowed" onclick={update} disabled={!first || !last || !selected}>
                Update
            </button>
            <button class="bg-red-500 text-white px-6 py-3 rounded shadow hover:bg-red-600 disabled:opacity-50 disabled:cursor-not-allowed" onclick={remove} disabled={!selected}>
                Remove
            </button>
        </div>
    </div>
</div>
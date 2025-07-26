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
    let selected = $derived(filterPeople[i]);
    
    function create(){
        people = people.concat({first, last});
        i = people.length - 1;
        first = last = '';
    }

    function update(){
        selected.first = first;
        selected.second = second;
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
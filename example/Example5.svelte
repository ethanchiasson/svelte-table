<script>
  // import SvelteTable from "../src/SvelteTable.svelte";
  import SvelteTable from "../src/index.js";
  import { generateFilter } from "./helper.js";
  // import SvelteTable from "svelte-table";
  import faker from "faker";
  faker.seed(15);

  let sortBy = "id";
  let sortOrder = 1;
  let iconAsc = "↑";
  let iconDesc = "↓";
  let selection = {first_name: "a" };

  const colums = [
    {
      key: "id",
      title: "ID",
      value: v => v.id,
      sortable: false,
      headerClass: "text-left"
    },
    {
      key: "first_name",
      title: "FIRST NAME",
      value: v => v.first_name,
      sortable: true,
      searchValue: v => v.first_name
    },
    {
      key: "last_name",
      title: "LAST NAME",
      value: v => v.last_name,
      sortable: true,
      searchValue: v => v.last_name
    },
    {
      key: "email",
      title: "EMAIL",
      value: v => v.email,
      sortable: true,
      class: "text-center"
    },
    {
      key: "age",
      title: "Age",
      value: v => v.age,
      sortable: true,
      filterOptions: generateFilter("age")
    },
    {
      key: "ip_address",
      title: "IP ADDRESS",
      value: v => v.ip_address,
      sortable: true
    }
  ];

  const numRows = 50;
  const data = Array(numRows)
    .fill("")
    .map((n, i) => {
      let d = {
        id: i,
        first_name: faker.name.firstName(),
        last_name: faker.name.lastName(),
        gender: faker.random.number(1) ? "Female" : "Male",
        age: 26 + faker.random.number(37)
      };
      d.email =
        d.first_name[0].toLowerCase() +
        d.last_name.toLowerCase() +
        "@zipit.org.ca";
      return d;
    });

  function setFilter(key, value = undefined) {
    console.log(selection, key, selection[key]);
    if (selection[key] || value != undefined) {
      selection[key] = value;
    }
  }

  function clearAll() {
    selection = {};
  }
</script>

<style>
  div :global(.g_female) {
    color: #f9e;
  }
  div :global(.g_male) {
    color: #99e;
  }
  div :global(.text-center) {
    text-align: center;
  }
  div :global(.text-left) {
    text-align: left;
  }
</style>

<div class="btn-group d-flex justify-content-center" role="group">
  <button
    class="btn btn-primary"
    disabled={!Object.values(selection).some(v => v != undefined)}
    on:click={() => clearAll()}>
    CLEAR ALL
  </button>
  <button
    class="btn btn-primary"
    disabled={selection['first_name'] === undefined}
    on:click={() => setFilter('first_name')}>
    CLEAR FIRST NAME
  </button>
  <button
    class="btn btn-primary"
    disabled={selection['last_name'] === undefined}
    on:click={() => setFilter('last_name')}>
    CLEAR LAST NAME
  </button>
  <button
    class="btn btn-primary"
    disabled={selection['age'] === undefined}
    on:click={() => setFilter('age')}>
    CLEAR AGE
  </button>
  <button
    class="btn btn-primary"
    on:click={() => setFilter('first_name', 'R') + setFilter('age', 48)}>
    Find Rosie
  </button>
</div>
<SvelteTable
  classNameTable="table"
  columns={colums}
  rows={data}
  bind:filterSelections={selection} />

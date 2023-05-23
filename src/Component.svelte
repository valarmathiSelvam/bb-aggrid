
<script>
  import { onMount } from "svelte";
  import { csvGenerator } from "./csvGenerator";
  import AgGrid from "@budibase/svelte-ag-grid";
  import 'ag-grid-community/styles/ag-grid.css';
  import 'ag-grid-community/styles/ag-theme-alpine.css';
  import AgGridSvelte from 'ag-grid-svelte';

  const columnDefs = [  
    { headerName: "User Id", field: "userId", sortable: true, filter: true, resizable: true, maxWidth: 120, flex: 1  },
    { headerName: "ID", field: "id", sortable: true, editable: true, filter: true, resizable: true, maxWidth: 70, flex: 1,
    cellRenderer: function (params) {
          if (params.value === 1) {
            return `<div style="padding:10px" ><button mat-button disabled style="background-color: #49ae3166;"> ${params.value} </button></div>`;
          }else{
            return params.value
          }
      // console.log(params,'params');
      // if(params.value <=5){
      //   return `<input type='checkbox' ${params.value <= 5 ? 'checked' : ''} />`;
      // }
    }
  },
    { headerName: "Title", field: "title", sortable: true, editable: true, filter: true, resizable: true, minWidth: 100, flex: 1 },
    { headerName: "Description", field: "body", sortable: true, editable: true, filter: true, resizable: true, minWidth: 250, flex: 1 },
];

  let rowData = [];
  let tableHeader = ["User Id", "ID", "Title", "Description"];
  const gridOptions = {
    
    pagination: true,
    paginationPageSize: 10,
    };


  function onGridReady() {
    fetch('https://jsonplaceholder.typicode.com/posts') 
      .then((resp) => resp.json())
      .then((data) => (rowData = data));
      console.log("hiiii");
  }

    function downloadHandler() {
    let tableKeys = Object.keys(rowData[0]); //extract key names from first Object
    console.log(columnDefs,'columnDefs');
    csvGenerator(rowData, tableKeys, tableHeader, "Report.csv");
  }

  // let data = [
  //   { make: "Toyota", model: "Celica", price: 35000 },
  //   { make: "Ford", model: "Mondeo", price: 32000 },
  //   { make: "Porsche", model: "Boxter", price: 72000 },
  // ];

  // let columnDefs = [
  //   { headerName: "Make", field: "make", sortable: true, editable: true },
  //   { headerName: "Model", field: "model", sortable: true },
  //   { headerName: "Price", field: "price", sortable: true },
  // ];

  // var tableData = [];
  // let columDefsApi = [
  //   { headerName: "User Id", field: "userId", sortable: true },
  //   { headerName: "ID", field: "id", sortable: true, editable: true },
  //   { headerName: "Title", field: "title", sortable: true },
  //   { headerName: "Description", field: "body", sortable: true },
  // ];

  // let tableHeader = ["User Id", "ID", "Title", "Description"];
  // let header = ["Make", "Model", "Price"];


  

  //     const url = 'https://jsonplaceholder.typicode.com/posts/1';
  // const promise = fetch(url).then(response => response.json());
  // {#await promise then data}
  //     <Post title={data.title} />
  // {/await}

  // let items = []

  // onMount(fetchItems)

  // async function fetchItems() {
  //     // ...
  //     const res = await fetch(`https://jsonplaceholder.typicode.com/posts`);
  //       tableData = await res.json();
  //       console.log(tableData);
  //     }

  // let datas = [];

  // onMount(async () => {
  //   const res = await fetch(`https://jsonplaceholder.typicode.com/posts`);
  //   tableData = await res.json();
  //   console.log(tableData, "td");
  //   // document.getElementById('test').innerHTML = `<AgGrid bind:${tableData} {columnDefs}/>}`
  // });

  //  const getData = new Promise((resolve, reject) => {
  //     const res =  fetch(`https://jsonplaceholder.typicode.com/posts`);
  //     tableData =  res;
  //     resolve(tableData);
  //  });
  //  getData().then((data)=>{
  //   console.log("getData",getData);
  //  });
  
  // async function initTodos() {
  //   const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  //   todos = await response.json();
  //   console.log(todos, "todos");
  // }

// let post = []
//   onMount(async () => {
//  post = await getPosts()
//   });
  
//   const getPosts = async () => {
//     const res = await fetch("https://jsonplaceholder.typicode.com/posts");
//     const data = await res.json();
//     return data;
//   };


  // function downloadHandler() {
  //   let tableKeys = Object.keys(data[0]); //extract key names from first Object
  //   console.log(header, "header");
  //   csvGenerator(data, tableKeys, header, "jsonReport.csv");
  // }
  // function downloadHandlerApi() {
  //   let tableKeys = Object.keys(data[0]); //extract key names from first Object
  //   console.log(header, "header");
  //   csvGenerator(tableData, tableKeys, header, "apiReport.csv");
  // }
  // console.log(JSON.stringify(todos), "todos");
</script>

<div class="container">
  <div class="header">
    <h1>Report</h1>
  </div>
  <div class="main">
    <div class="btncontainer">
      <button on:click={downloadHandler}>Download</button>
    </div>
<div style:height="500px" class="ag-theme-alpine">
  <AgGridSvelte {rowData} {columnDefs} {onGridReady} {gridOptions}/>
</div>
  </div>
</div>

<!-- <div>
  <hi>
    {JSON.stringify(todos)}
  </hi>
  {#await initTodos() then whatever}
    <h1>{todos.length} todos!!!</h1>

    <AgGrid bind:todos {columDefsApi} />
  {/await}
</div>
<div>
 
</div> -->
<!-- <div class="test">
  <h1>Ag-Grid with json data</h1>
  <button on:click={downloadHandler}>Download</button>
  <AgGrid bind:data {columnDefs} />
</div> -->

<!-- <div class="test">
  <h1>Ag-Grid with API data</h1>
 {JSON.stringify(aumm)}
 <AgGrid bind:[aumm] {columDefsApi} />

  <button on:click={downloadHandlerApi}>Download</button>
</div> -->

<!-- <div class="container">
  <div class="header">
    <h1>Report</h1>
  </div>
  <div class="main">
    <table>
      <thead>
        <tr>
          {#each tableHeader as header}
            <th>{header}</th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each tableData as item}
          <tr>
            <td>{item.userId}</td>
            <td>{item.id}</td>
            <td>{item.title}</td>
            <td>{item.body}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div> -->

<style>
  /* .container {
    max-width: 1140px;
    margin: auto;
  } */
  /* ::ng-deep.active-button {
  display: flex;
  height: 24px;
  align-items: center;
  width: 75px;
  top: 8px;
  position: relative;
  background-color: #49ae3166;
  font-weight: 400;
  border-radius: 4px;
  color: #358222;
  border:none;
  justify-content: center;
  font: normal normal normal 12px/16px Roboto;
letter-spacing: 0.3px;
} */
  
  .header {
    display: flex;
    justify-content: space-between;
    display: flex;
    justify-content: space-between;
    background: orange;
    padding: 10px;
  }
  .btncontainer{
    display: flex;
    justify-content: flex-end;
    padding: 10px 0px 10px;
  }
  /* table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  } */
  .ag-theme-alpine {
    --ag-background-color: #ddd;
  }
  /* td,
  th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  } */
  button {
    border: none; /* Remove borders */
    color: white; /* Add a text color */
    padding: 14px 28px; /* Add some padding */
    cursor: pointer; /* Add a pointer cursor on mouse-over */
    background-color: #4caf50;
    height: fit-content;
  }
  h1 {
    margin: 0px;
  }
</style>

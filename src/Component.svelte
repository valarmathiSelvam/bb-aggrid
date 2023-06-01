<script>
  import { getContext } from "svelte";
  import { csvGenerator } from "./csvGenerator";
  import 'ag-grid-community/styles/ag-grid.css';
  import 'ag-grid-community/styles/ag-theme-alpine.css';
  import AgGridSvelte from 'ag-grid-svelte';

  export let apiData
  export let tableBackgroundColor
  export let tableheaderbackground
  export let rowFontSize
  export let title
  export let Button
  export let showDownloadButton
  export let headerbackground
  export let tableoddrowbackground
  export let buttonBackgroundColor
  export let headerforeground
  export let rowFontStyle
  export let tableHeight
  export let fontWeight
  export let buttonFontColor
  export let buttonRadius
  export let dataColor
  export let titlefontWeight
  export let titleFontColor
  const { stylable } = getContext("sdk")
  const component = getContext("component") 
  const columnDefs = [];
  let apidata =[];
  let rowData = [];

  
  const gridOptions = {
    defaultColDef: {
            sortable: true,
            filter: 'agTextColumnFilter',
            resizable: true
        },
    columnDefs: columnDefs,
    pagination: true,
    paginationPageSize: 100,
    };


  async function onGridReady() {
         await fetch(apiData) 
      .then((resp) => resp.json())
      .then((data) => {rowData = data;
        const colDefs = gridOptions.api.getColumnDefs();
        colDefs.length=0;
        const keys = Object.keys(data[0])
        keys.forEach(key => colDefs.push({field : key}));
        gridOptions.api.setColumnDefs(colDefs); 
  });
  }

    function downloadHandler() {
    let tableKeys = Object.keys(rowData[0]); //extract key names from first Object
    csvGenerator(rowData, tableKeys, tableKeys, "Report.csv");
   }

</script>

  <div src={apidata}  >
    <div class="header" style="--background:{headerbackground}"  >
      <h1 style="--font-weight:{titlefontWeight}; --color:{titleFontColor}">{title}</h1>
    </div>
    <div class="main">
      {#if showDownloadButton}
      <div class="btncontainer">
        <button style="--background-color:{buttonBackgroundColor}; --color:{buttonFontColor}; --border-radius:{buttonRadius}" on:click={downloadHandler}>{Button}</button>
      </div>
      {/if}
      <div style="--ag-background-color:{tableBackgroundColor}; --ag-header-foreground-color:{headerforeground};--ag-odd-row-background-color:{tableoddrowbackground}; --font-family:{rowFontStyle}; --ag-header-background-color:{tableheaderbackground}; --font-size:{rowFontSize}: --height:{tableHeight}; --font-weight:{fontWeight}; --ag-data-color:{dataColor} " class="ag-theme-alpine">
        <AgGridSvelte  {rowData} {columnDefs} {onGridReady} {gridOptions}/>
      </div>
    </div>
  </div>

<style>
  .header {
    display: flex;
    justify-content: space-between;
    display: flex;
    justify-content: space-between;
    background: var(--background,  orange );
    padding: 10px;
    font-weight: var(--font-weight, 500);
  }
  h1{
    font-weight: var(--font-weight, 500 ) ;
    color:var(--color,white) ;
  }
  .btncontainer {
    display: flex;
    justify-content: flex-end;
    padding: 10px 0px 10px;
  }
  button {
    border-radius:var(--border-radius, 0px);
    border: none; /* Remove borders */
    color:var(--color,white) ; /* Add a text color */
    padding: 14px 28px; /* Add some padding */
    cursor: pointer; /* Add a pointer cursor on mouse-over */
    background-color: var(--background-color, #4caf50 ) ;
    height: fit-content;
  }
  .ag-theme-alpine {
    --ag-background-color:var(--ag-background-color,#ddd) ;
    --ag-header-height:var(--ag-header-height, 150px) ;
  --ag-header-foreground-color:var(--ag-header-foreground-color, #1b4e9b)  ;
  --ag-header-background-color:var(--ag-header-background-color, #ddd) ;
  --ag-odd-row-background-color:var(--ag-odd-row-background-color, rgba(237, 237, 237, 0.6)) ;
  --ag-data-color:var(--ag-data-color,black);
    height:   500px;
    color: var(--color, #1b4e9b) ;
  font-size: var(--font-size, 13px ) ;
  font-family: var(--font-family, Roboto ) ;
  font-weight: var(--font-weight, 500 ) ;
  letter-spacing: var(--letter-spacing, 0.33px ) ;
  }

  h1 {
    margin: 0px;
  }
</style>

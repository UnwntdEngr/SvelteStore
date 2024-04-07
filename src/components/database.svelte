<script>
    import { writableArray } from '../store/index'
    import { onMount } from 'svelte';

    let SHEET_ID = '1mlhZiHP20-LEDnhyFk5WJkBmFJJ0LFh-QHl1trZC-pI';
    let SHEET_TITLE = 'forDessertPage';
    let SHEET_RANGE = 'A1:D99';
    let FULL_URL = ('https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/gviz/tq?sheet=' +SHEET_TITLE + '&range=' +SHEET_RANGE);

    async function fetchData() {
        fetch(FULL_URL)
        .then(res => res.text())
        .then(rep=>{
          let data = JSON.parse(rep.substr(47).slice(0,-2));
          let length = data.table.rows.length;
          for( let i = 0; i<length;i++){
            $writableArray = [...$writableArray, {'id': data.table.rows[i].c[0].v, 'name':data.table.rows[i].c[1].v,'price': data.table.rows[i].c[2].v, 'category':data.table.rows[i].c[3].v}]
            console.log(i);
            console.log($writableArray);
      }
    })
    }
  
    onMount(fetchData);
</script>

<template>
  <div class="table">
    <table>
      <thead>
        <tr>
          <th v-on:click="sort(column)" v-for="column in columns" :key="column.dataKey">
            <div class="flex-row" :style="`align-items: center; justify-content: ${column.align}`">
              <div style=" margin-right: 4px;">{{ column.name }}</div>
              <div class="flex-column">
                <small :class="`text-xs ${(sortColumn == column.dataKey && sortOrder == 'asc') ? '' : 'inactive'}`">&#9650;</small>
                <small :class="`text-xs ${(sortColumn == column.dataKey && sortOrder == 'desc')? '' : 'inactive'}`">&#9660;</small>
              </div> 
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="index in getNumOnPage()" :key="getTrueIndex(index)" :class="(index%2 == 0) ? '' : 'striped'">
          <td v-for="column in columns" :key="getTrueIndex(index) + '-' + column.dataKey" :style="`text-align: ${column.align};`">{{ formatValue(column, index) }}</td>
        </tr>
      </tbody> 
    </table> 
    <div class="flex-row footer">
      <div class="flex-row">
        <div :class="['page-back', ((this.page > 0) ? '' : 'disabled')]" v-on:click="pageBack()">&laquo;</div>
        <div v-if="pages === 0" class="page-num active"><small>1</small></div>
        <div v-else v-for="pageIndex in (pages > 10) ? 10 : pages" :key="pageIndex + pageOffset * 10" :class="`page-num ${(pageIndex + pageOffset * 10 - 1 == page) ? 'active' : ''}`" v-on:click="pageNum(pageIndex + pageOffset * 10)">
          <small>{{pageIndex + pageOffset * 10}}</small>
        </div>
        <div :class="['page-forward', ((this.page < this.pages-1) ? '' : 'disabled')]" v-on:click="pageForward()">&raquo;</div>
      </div>
      <div class="page-info">
        <small>{{pages}} pages ({{rows.length}} results)</small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    columns: Array,
    rows: Array,
    perPage: {
      type: Number,
      default: 10,
    } 
  },
  data() {
    return {
      page: 0,
      pageOffset: 0,
      sortColumn: '',
      sortOrder: '',
    }
  },
  computed: {
    pages(){
      return (this.perPage === 0) ? 0 : Math.ceil(this.rows.length/this.perPage);
    }
  },
  methods: {
    formatValue (column, index) {
      let trueIndex = this.getTrueIndex(index);
      return (column.formatValue !== undefined) ? column.formatValue(this.rows[trueIndex][column.dataKey]) : this.rows[trueIndex][column.dataKey];
    },
    getTrueIndex(index){
      return this.page * this.perPage + index - 1;
    },
    pageForward(){
      if(this.page < this.pages-1){
        this.page++;
        if(this.page%10 === 0){
          this.pageOffset++;
        }
      }
    },
    pageBack(){ 
      if(this.page > 0){
        if(this.page%10 === 0){
          this.pageOffset--;
        }
        this.page--;  
      }
    },
    pageNum(page){
      this.page = page - 1;
    },
    getNumOnPage(){
      let numRowsLeft = this.rows.length - this.perPage * (this.page);
      if(numRowsLeft < this.perPage){
        return numRowsLeft;
      }
      else{
        return this.perPage;
      }
    },
    sort(column){
      if(this.sortColumn === column.dataKey){
        if(this.sortOrder === 'asc'){
          this.sortOrder = 'desc';
          this.rows.reverse();
        }
        else if(this.sortOrder === 'desc'){
          this.sortOrder = 'asc';
          this.rows.sort(this.compare(column));
        } 
      }
      else{
        this.sortColumn = column.dataKey;
        this.sortOrder = 'asc';
        this.rows.sort(this.compare(column));
      }
    },
    compare(column){
      let key = column.dataKey;
      return (a,b)=>{
        let val1 = (column.formatValue === undefined) ? a[key] : column.formatValue(a[key]);
        let val2 = (column.formatValue === undefined) ? b[key] : column.formatValue(b[key]);
        if (val1 < val2) {
          return -1;
        }
        if (val1 > val2) {
          return 1;
        }
        return 0;
      }
    }, 
  }
}
</script>


<style scoped>
  .table{
    font-family:Arial, Helvetica, sans-serif;
    background: #f6f7f9;
    margin: 8px;
  } 

  table{
    width: 100%;
    padding: 16px;
    border-spacing: 0px;
  }

  th{
    padding: 8px;
    color: #748c9d;
    border: none;
    cursor: pointer;
  }

  td{
    padding: 8px;
    color: #8797ac;
    border: none;
  }

  .footer{
    padding-left: 16px;
    padding-right: 16px;
    padding-bottom: 16px;
    color: #8797ac;
  }
  
  .flex-row{
    display: flex;
    flex-direction: row;
  }

  .flex-column{
    display: flex;
    flex-direction: column;
  }

  .page-back{
    color: black;
    background: white;
    border: solid #d9dee2 1px;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
    padding-left: 6px;
    padding-right: 6px;
    padding-top: 3px;
    padding-bottom: 3px;
    cursor: pointer;
  }

.page-forward{
    color: black;
    background: white;
    border-top: solid #d9dee2 1px;
    border-right: solid #d9dee2 1px;
    border-bottom: solid #d9dee2 1px;
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    padding-left: 6px;
    padding-right: 6px;
    padding-top: 3px;
    padding-bottom: 3px;
    cursor: pointer;
  }

  .page-num{
    color: black;
    background: white;
    border-top: solid #d9dee2 1px;
    border-bottom: solid #d9dee2 1px;
    border-right: solid #d9dee2 1px;
    padding-left: 6px;
    padding-right: 6px;
    padding-top: 3px;
    padding-bottom: 3px;
    cursor: pointer;
  }

  .active{
    background: #d9dee2;
  }

  .page-info{
    margin-left: 16px;
  }

  .disabled{
    cursor: default;
  }

  .striped{
    background: #e6ecec;
  }

  .text-xs{
    font-size: 8px;
  }

  .inactive{
    color: #d9dee2;
  }
</style>
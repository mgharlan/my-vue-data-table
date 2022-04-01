<template>
  <div class="table">
    <table>
      <thead>
        <tr>
          <th v-for="column in columns" :key="column.dataKey" :style="`text-align: ${column.align};`">{{ column.name }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="index in getNumOnPage()" :key="getTrueIndex(index)" :class="(getTrueIndex(index)%2 == 0) ? 'striped' : ''">
          <td v-for="column in columns" :key="getTrueIndex(index) + '-' + column.dataKey">{{ formatValue(column, index) }}</td>
        </tr>
      </tbody> 
    </table> 
    <div class="flex-row footer">
      <div class="flex-row">
        <div :class="['page-back', ((this.page > 0) ? '' : 'disabled')]" v-on:click="pageBack()">&laquo;</div>
        <div v-if="pages === 0" class="page-num active"><small>1</small></div>
        <div v-else v-for="pageIndex in pages" :key="pageIndex" :class="`page-num ${(pageIndex-1 == page) ? 'active' : ''}`" v-on:click="pageNum(pageIndex)"><small>{{pageIndex}}</small></div>
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
      }
    },
    pageBack(){
      if(this.page > 0){
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
    }
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

  .page-back{
    color: black;
    background: white;
    border: solid #d9dee2 1px;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
    padding-left: 4px;
    padding-right: 4px;
    padding-top: 2px;
    padding-bottom: 2px;
    cursor: pointer;
  }

.page-forward{
    color: black;
    background: white;
    border: solid #d9dee2 1px;
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    padding-left: 4px;
    padding-right: 4px;
    padding-top: 2px;
    padding-bottom: 2px;
    cursor: pointer;
  }

  .page-num{
    color: black;
    background: white;
    border-bottom: solid #d9dee2 1px;
    border-top: solid #d9dee2 1px;
    padding-left: 4px;
    padding-right: 4px;
    padding-top: 2px;
    padding-bottom: 2px;
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
</style>
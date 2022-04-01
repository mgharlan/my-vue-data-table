<template>
  <table>
    <thead>
      <tr>
        <th v-for="column in columns" :key="column.dataKey" :style="`text-align: ${column.align};`">{{ column.name }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="index in perPage" :key="getTrueIndex(index)" :class="(getTrueIndex(index)%2 == 0) ? 'striped' : ''">
        <td v-for="(value, key) in rows[getTrueIndex(index)]" :key="key + '-' + getTrueIndex(index)">{{ formatValue(key, value) }}</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td :colspan="columns.length">
          <div class="page-back" v-on:click="pageBack()">&laquo;</div>
          <div :class="`page-num ${(pageIndex-1 == page) ? 'active' : ''}`" v-for="pageIndex in pages" :key="pageIndex" v-on:click="pageNum(pageIndex)"><small>{{pageIndex}}</small></div>
          <div class="page-forward" v-on:click="pageForward()">&raquo;</div>
          <div class="page-info">
            <small>{{pages}} pages ({{rows.length}} results)</small>
          </div>
        </td>
      </tr>
    </tfoot>
  </table> 
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
      return Math.ceil(this.rows.length/this.perPage);
    }
  },
  methods: {
    formatValue (key, value) {
      let column = this.columns.find(column => column.dataKey === key)
      return (column.formatValue !== undefined) ? column.formatValue(value) : value;
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
    }
  }
}
</script>


<style scoped>
  table{
    width: 100%;
    font-family:Arial, Helvetica, sans-serif;
    padding: 16px;
    background: #f6f7f9;
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

  tfoot tr td{
    padding-top: 16px;
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

  .striped{
    background: #e6ecec;
  }
</style>
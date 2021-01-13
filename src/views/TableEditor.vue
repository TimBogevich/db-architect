<template>
  <div id="app">
    <div class="header-button">
        <div class="toggle">
            <button class="button_table" @click="Table()">TABLE</button>
            <button class="button_table" @click="Ddl()">DDL</button>
        </div>
    </div>
    <div class="table" v-show="table">
      <div class="header-change" >
        <div class="header-modal" v-for="(item, index) in headers" :key="index">
            <label >
            Name: 
            </label>
            <input type="text" v-model="item.headerName">
            <label for="" >
            Value:   <select  v-model="item.headerType" >
                        <option >int</option>
                        <option >varchar</option>
                        <option >date</option>
                        <option >datetime</option>
                    </select>
            </label>
        </div>
      </div>
    
    <div class="table-main">
        <vue-table
        :tbody-data="products"
        :headers="headers"
        :custom-options="customOptions"
        :style-wrap-vue-table="styleWrapVueTable"
        :disable-cells="disableCells"
        :disable-sort-thead="disableSortThead"
        :loading="loading"
        :parent-scroll-element="parentScrollElement"
        :select-position="selectPosition"
        :submenu-tbody="submenuTbody"
        :submenu-thead="submenuThead"
        v-on:tbody-change-data="changeData"
        v-on:thead-td-sort="sortProduct"
        >
      <div slot="loader">
        Loader
      </div>
      </vue-table>
        <div class="footer-table">
          <div class="button">
            <button @click="show = !show" >New Column</button>
          </div>
          <div class="button">
            <button @click="AddRow()" >New Row</button>
          </div>
        </div>
        <div class="modal-show" v-if="show">
          <div @click="show = !show" class="modal-close">X</div>
          <div class="show-input">Name <input v-model="headerName"></div>
          <div class="show-input">Key <input v-model="headerKey"></div>
          <div class="show-input">Type:  <label for="" >
              <select  v-model="headerType" >
                          <option >int</option>
                          <option >varchar</option>
                          <option >date</option>
                          <option >datetime</option>
                      </select>
              </label></div>
          <button @click="AddColumn(); show = !show" class="show-button">New Column</button>
        </div>
      </div>
    </div>

    <div class="ddl"  v-show="ddl">
      <div class="generate-table">
          <span>CREATE TABLE categories</span>
          <br>
          (<span v-for="(items, index) in headers" :key="index">
              {{items.headerName}} {{items.headerType}} NOT NULL, <br>
          </span>)

      </div>
      <div class="generate" v-for="(item, index) in products" :key="index">
          <span>INSER INTO categories</span>
          <br>
          (<span v-for="(items, index) in headers" :key="index">
                  {{items.headerName}},
              </span> );
          <br>
          VALUES
          <br>
          <div>
              ({{index+1}} <span v-for="(item, index) in item" :key="index">
                        {{item.value}},
                    </span>);
          </div>
      </div>
    </div>
  </div>
</template>

<script>

import VueTable from 'vuejs-spreadsheet';

export default {
  name: 'app',
  data() {
    return {
      table: true,
      ddl: false,
      headerName: "",
      headerType: "",
      headerKey: "",
      show: false,
      customOptions: {
        tbodyIndex: true,
        sortHeader: true,
        trad: {
          lang: 'fr',
          en: {
            select: {
              placeholder: 'Search by typing',
            },
          },
          fr: {
            select: {
              placeholder: 'Taper pour chercher',
            },
          },
        },
        newData: {
          type: 'input',
          value: '',
          active: false,
          style: {
            color: '#000',
          },
        },
        fuseOptions: {
          shouldSort: true,
          threshold: 0.2,
          location: 0,
          distance: 30,
          maxPatternLength: 64,
          minMatchCharLength: 1,
          findAllMatches: false,
          tokenize: false,
          keys: [
            'value',
          ],
        },
      },
      submenuTbody: [
      ],
      submenuThead: [
      ],
      disableCells: ['a'],
      loading: false,
      parentScrollElement: {
        attribute: 'html',
        positionTop: 0,
      },
      selectPosition: {
        top: 0,
        left: 0,
      },
      disableSortThead: ['a'],
      styleWrapVueTable: {
        fontSize: '12px',
        comment: {
          borderColor: '#696969',
          borderSize: '8px',
          widthBox: '120px',
          heightBox: '80px',
        },
      },
      headers: [
        {
          headerName: 'B',
          headerKey: 'b',
          headerType: 'varchar',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
            opacity: '0'
          },
        },
      ],
      products: [
        {
          a: {
            type: 'input',
            value: '',
            active: false,
            style: {
              color: '#000',
            },
          },
        },
      ],
    };
  },
  components: {
    VueTable,
  },
  mounted() {
    this.loading = true;
    setTimeout(() => {
      this.loading = false;
    }, 300);
  },
  methods: {
    changeData(row, header) {
      console.log(row, header);
    },
    sortProduct() {
      console.log('sort product');
    },
    AddColumn() {
      this.headers.push({
          headerName: this.headerName,
          headerKey: this.headerKey,
          headerType: this.headerType,
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
            opacity: '0'
          },
      })
    },
    AddRow() {
      this.products.push({
        a: {
            type: 'input',
            value: '',
            active: false,
            style: {
              color: '#000',
            },
          },
        })
    },
    Table() {
      this.table = true
      this.ddl = false
    },
    Ddl() {
      this.table = false
      this.ddl = true
    },
  },
};
</script>

<style lang="scss">
#app {
  padding: 20px;
}
::-moz-selection {
  color: #2c3e50;
  background: transparent;
}
::selection {
  color: #2c3e50;
  background: transparent;
}
.button {
  margin-right: 10px;
  background-color: #e7ecf5;
  padding: 10px 15px;
}
.modal-show {
  display: block;
  position: fixed;
  top: 50%;
  left: 45%;
  width: 300px;
  height: 200px;
  padding: 20px;
  background-color: #e7ecf5;
  display: -webkit-box;
  display: -ms-flexbox;
  text-align: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center;
  .show-button {
    border: 1px solid black;
    padding: 5px 10px;
  }
  .modal-close {
    text-align: right;
    margin-bottom: 10px;
    cursor: pointer;
  }
}
.show-input {
  display: flex;
  margin-bottom: 10px;
  font-weight: bold;
  input {
    border: 1px solid black;
    margin-left: 5px;
  }
}
.generate-table {
    padding: 10px;
    background-color: #e6ecf6;
    border-left: 3px solid #478bf8;
    margin-bottom: 10px;
    text-align: center;
}
.generate { 
    padding: 10px;
    background-color: #e6ecf6;
    border-left: 3px solid #478bf8;
}
.button_table {
    padding: 5px 10px;
    border: 2px solid #e6ecf6;
    margin-bottom: 2px;
    margin-right: 2px;
    &:hover {
        border: 2px solid #9fa3aa;
        background: #e6ecf6;
    }
    &:focus {
        outline: none
    }
}

.header-modal { 
  width: 99px;
  min-width: 99px;
  color: #000;
  background-color: #e6ecf6;
  margin-right: 1px;
  padding: 5px;
  font-size: 12px;
  input {
    width: 50px;
  }
}


.footer-table {
  display: flex;
  align-items: center;
  margin-top: 10px;
}
.table {
  position: relative;
  .header-change {
    position: absolute;
    top: 0;
    display: flex;
    margin-left: 29px;
    z-index: 11;
  }
  .table-main {
    position: absolute;
    z-index: 10;
  }
}
</style>
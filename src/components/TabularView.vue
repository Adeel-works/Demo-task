<template>
  <div class="p-5">
    <div style="display: flex" class="mb-3">
      <div class="d-flex m-1" :key="item" v-for="item in currencies">
        <b-button>
          {{ item }}
        </b-button>
      </div>
      <div style="width: 50px" />
      <div class="d-flex m-1" :key="item" v-for="item in years">
        <b-button :variant="selectedYears.find(i=>i==item) ? 'primary':'secondary'" @click="()=>handleYears(item)"> {{ item }} Yrs </b-button>
      </div>
    </div>
    <div class="mb-3 mt-5 col-3">
      <b-form-input
        :value="searchQuery"
        @change="handleSearch"
        placeholder="Filter by company name"
      />
    </div>
    <div class="tableContainer">
    <div class="d-flex justify-content-between">
      <div class="d-flex">
     <h5 class="headingTitle">date sent</h5>
     <div style="width:10px"/>
     <h5 class="headingTitle">company</h5>
    </div>
    <!-- <div class="d-flex">
     <div v-for="items in years" :key="items">
      <div class="d-flex flex-column ">
      <p class="headingTitle">
        {{ items }} Years
      </p>
      <div class="d-flex">
      <span class="headingTitle">
        FIX
      </span>
      <span class="headingTitle">
        FRN
      </span>
    </div>
    </div>
  </div>
     </div> -->
    </div>

    <div class=" justify-content-between"> 
      <div  v-for="items in results" :key="items">
      <div class="d-flex align-items-start">
     <!-- <h5 class="headingTitle mt-2">{{ items.DateSent }}</h5> -->
     <!-- <div style="width:50px"/> -->
     <!-- <h5 class="headingTitle mt-2">{{ items.Company }}</h5> -->
     <div class="d-flex flex-column">
     <h5 class="headingTitle mt-2">{{  }}</h5>
     <h5 class="headingTitle mt-2">{{  }}</h5>
     <h5 class="headingTitle mt-2">{{  }}</h5>
     <h5 class="headingTitle mt-2">{{  }}</h5>
     <div class="d-flex">
      <!-- <div style="width:120px"/> -->
     <h5 class="headingTitle mt-2">{{ items.DateSent }}</h5>

     <h5 style="width:100px" class="headingTitle mt-2">{{ items.Company }}</h5>
    </div>
     <h5 class="headingTitle mt-2">{{  }}</h5>
     <div class="d-flex">
      <div style="width:120px"/>

     <h5 style="width:100px" class="headingTitle mt-2">Yield</h5>
     
      </div>
     </div>
     <div class="d-flex">
    
    <div v-for="item in items.Quote" :key="item">
      
      <div class="d-flex" v-if="selectedYears.find(i=>i==item.Years)">
      <div class="d-flex flex-column">
      <p style="padding: 10px">{{ item.Years }} Years</p>
      <div class="d-flex">
      <div class="d-flex flex-column">
      <p style="padding: 10px">FIX</p>
      <p style="padding: 10px" v-if="item.CouponType=='FIX'">{{ item.Yield }}</p>
     </div>
     <div class="d-flex flex-column">
      <p style="padding: 10px">FRN</p>
      <p style="padding: 10px" v-if="item.CouponType=='FRN'">{{ item.Yield }}</p>
      </div>
    </div>
      </div> 
    </div>

    </div>
  </div>
    </div>
 
  </div>
    </div>

  </div>

    <!-- <div class="">
      <b-table
        :items="results"
        :fields="fields"
        label-sort-asc=""
        label-sort-desc=""
        label-sort-clear=""
        responsive="sm"
      >
     
      
        <template  #thead-top>
          <b-tr class="headerTop">
            <template v-for="(item,index) in years">
            <b-th></b-th>
            <b-th></b-th>
            <b-th v-bind="index" colspan="4">
              {{ item }} Years
            </b-th>
            
          </template>
          </b-tr>   
        </template>

        <template 
        #cell(fix)="data">
          <div 
          :key="i"
          v-for="i in  data.item.Quote">
          <span v-if="i.CouponType=='FIX'">
            {{ i?.Amount }} ({{ i?.Years }})
          </span>
      
        </div>
        </template>

        <template 
        #cell(DateSent)="data">
          <div >
            <b-button :variant="'transparent'">
          <b-icon-arrow-down></b-icon-arrow-down>
        </b-button>
          <span >
            {{data.item.DateSent}}
          </span>
      
        </div>
        </template>  
        <template 
        #cell(frn)="data">
          <div 
          :key="i"
          v-for="i in  data.item.Quote">
          <span v-if="i.CouponType=='FRN'">
            {{ i?.Amount }} ({{ i?.Years }})
          </span>
        </div>
        </template>
      </b-table>
    </div> -->
  </div>
</template>

<script>
import {  BButton, BFormInput} from "bootstrap-vue";
import data from "../../DATA.json";

export default {
  components: {
    
   
    BFormInput,
    BButton,
  },
  data() {
    return {
      sortDesc: false,
      selectedCurrency: "",
      selectedYears:[],
      searchQuery: "",
      fields: [
        { key: "DateSent", sortable: true, label: "DATE SENT" },
        { key: "Company", sortable: true, label: "COMPANY" },
        { key: "FIX", sortable: false, label: "FIX" },
        { key: "FRN", sortable: false, label: "FRN" },
      ],
      items: data.Items,
    };
  },
  methods: {
    handleYears(item){
      console.log(item)
      if(this.selectedYears.find(i=>i==item)){
        const filteredYears = this.selectedYears.filter(i=>i!=item);
        this.selectedYears = filteredYears;
      }else{
        this.selectedYears.push(item)
      }
    },
    handleSearch(text) {
      console.log("text", text);
      console.log(this.currencies);
      this.searchQuery = text;
    },
    handleCurrency() {},
  },
  created() {
    console.log(data.Items);
  },
  updated() {
  },
  computed: {
    results() {
      if (this.searchQuery) {
        return this.items.filter((i) => i.Company.toLowerCase().includes(this.searchQuery.toLowerCase()));
      } else {
        return this.items;
      }
    },
    years() {
      if (data) {
        const temp = [...data.Items];
        const years = [];
        temp.forEach((i) => {
          if (i.Quote && i.Quote.length) {
            i.Quote.forEach((x) => {
              years.push(x.Years);
            });
          }
        });
        // if(this.selectedYears)
    
        

        return [...new Set(years)].sort((x,y)=>x-y);
      }else{
        return []
      }
    },
    currencies() {
      if (data) {
        const temp = [...data.Items];
        const currencies = [];
        temp.forEach((i) => {
          if (i.Quote && i.Quote.length) {
            i.Quote.forEach((x) => {
              currencies.push(x.Currency);
            });
          }
        });
        return [...new Set(currencies)];
        // return [...new Set(currencies)];
        // const currency = [];
        // temp.forEach(i=>{
        //   i.Quote.forEach(x=>{
        //     currency.push(x.Currency)
        //   })
        // })
      } else return [];
    },
  },
  setup() {
 

  },
};
</script>

<style scoped>
.headerTop {
  color: #cbcbcb;
}
.button {
  border-radius: 0px;
}
.buttonRadius {
  border-top-right-radius: 0px;
  border-bottom-right-radius: 0px;
}
.buttonEnd {
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
}
.headingTitle{
  text-transform: capitalize;
  font-size: 20px;
  font-weight: 600;
  padding: 0px 20px;
  padding-left: 0px;
}

.dataFieldText{
  text-transform: capitalize;
  text-align: left;
  font-size: 15px;
  padding: 0px 20px;
  padding-left: 0px;
}
.tableContainer{
  border: 1px solid rgb(182, 182, 182);
  padding: 10px;
  border-radius: 8px;
}
</style>
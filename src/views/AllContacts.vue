<template>
  <v-container fluid id="containerAll">
    <v-text-field
      v-model="searchQuery"
      append-icon="mdi-magnify"
      label="Search for a contact"
      hide-details
    ></v-text-field>
    <Pagination
      v-if="filtredTable"
      v-model="pagination"
      :totalRecords="filtredTable.length"
      :perPageOptions="perPageOptions"
    />
    <Table
      v-if="filtredTable"
      :theData="computedTableData"
      :config="config"
      :perPage="perPageOptions"
      :search="search"
    />
  </v-container>
</template>

<script>
import Table from '../components/Table'
import Pagination from '../components/Pagination'
import mockData from '../store/mock-data.js'
const perPageOptions = [6]

export default {
  components: {
    Pagination,
    Table
  },
  data: function () {
    return {
      perPageOptions,
      page: 1,
      perPage: perPageOptions[0],
      tableData: mockData,
      pagination: { page: 1, perPage: perPageOptions[0] },
      searchQuery: '',
      config: [
        {
          value: 'email',
          text: 'Email'
        },
        {
          text: 'Source',
          sortable: false
        },
        {
          value: 'familyName',
          text: 'First Name'
        },
        {
          value: 'givenName',
          text: 'Last Name'
        },
        {
          value: 'applicationStatus',
          text: 'App Status',
          sortable: false
        },
        {
          text: 'Profile Status',
          sortable: false
        },
        {
          value: 'value',
          text: 'Value'
        },
        {
          value: 'mortgageRenewalDate',
          text: 'Renewal Date'
        },
        {
          value: 'referrer.familyName',
          text: 'Referrer',
          sortable: false
        },
        {
          text: 'OPT-IN',
          sortable: false
        }
      ]
    }
  },
  computed: {
    computedTableData () {
      if (!this.filtredTable) return []
      else {
        const firstIndex = (this.pagination.page - 1) * this.pagination.perPage
        const lastIndex = this.pagination.page * this.pagination.perPage
        return this.filtredTable.slice(firstIndex, lastIndex)
      }
    },
    filtredTable(){
      if(this.searchQuery){
      return this.tableData.filter((item)=>{
        return this.searchQuery.toLowerCase().split(' ').every(x => 
        (item.givenName.toLowerCase().includes(x) ||
         item.familyName.toLowerCase().includes(x) ||
         item.email.toLowerCase().includes(x)))
      })
      }else{
        return this.tableData;
      }
    }
  }
}
</script>

<style>
  #containerAll{
    max-width: auto;
    padding-block: 0px;
    padding-inline: 0px;
  }
</style>
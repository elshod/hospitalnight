<template>
  <div id="app">
    <div class="container">
      <h1>Test Bootstrap</h1>
      <div class="d-flex justify-content-between">
        <b-button v-b-modal.modal-newbemor>Yangi bemor</b-button>
        <b-form-select v-model="search"  class="mb-3" :options="searchOption"></b-form-select>
      </div>
      <b-table striped hover 
        :fields="fields" 
        :items="filteredPatients">
        <template #cell(btn)="data">
          <b-button @click='del(data.index)' variant='danger'>
            <b-icon-person-x-fill></b-icon-person-x-fill>
          </b-button>
        </template>
      </b-table>

      <b-modal id="modal-newbemor" hide-footer title="Yangi bemor">
        <b-form-input v-model="patient.name" class="mb-3" placeholder="Ismini yozing"></b-form-input>
        <label for="example-datepicker">Tug'ilgan sana</label>
        <b-form-datepicker id="example-datepicker" v-model="patient.bdate" class="mb-3"></b-form-datepicker>
        <b-form-group label="Jinsni tanlang" v-slot="{ ariaDescribedby }" >
          <div class="d-flex">
            <div class="mr-3">
              <b-form-radio v-model="patient.gender" :aria-describedby="ariaDescribedby" name="gender" class="mr-3" value="Erkak">Erkak</b-form-radio>
            </div>
            <div class="mr-3">
              <b-form-radio v-model="patient.gender" :aria-describedby="ariaDescribedby" name="gender" class="ml-3" value="Ayol">Ayol</b-form-radio>
            </div>
          </div>
        </b-form-group>
        <label for="doctor" class="mt-3">Shifokorni tanlang</label>
        <b-form-select v-model="patient.doctor" id="doctor" class="mb-3 form-control" :options="doctors"></b-form-select>
        <label for="time">Qabul vaqtini tanlang</label>
        <b-form-timepicker id="time" v-model="patient.time" locale="ru"></b-form-timepicker>
        <b-button variant='success' class="mt-3" @click='add()'>Qo'shish</b-button>
      </b-modal>
    </div>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      patient:{},
      doctors: ['Bahodirov Aziz','Yo`ldoshev Farhod','Olimova Aziza'],
      patients: [],
      search:'Barchasi',
      searchOption: ['Barchasi','Erkak','Ayol'],
      fields: [
          { key: 'name', label: 'Ism' },
          { key: 'bdate', label: 'Tugilgan sana'},
          { key: 'gender', label: 'Jinsi'},
          { key: 'doctor', label: 'Davolovchi shifokor'},
          { key: 'time', label: 'Qabul vaqti'},
          { key: 'btn', label: 'O`chirish'},
        ],
    }
  },
  methods: {
    add(){
      this.patients.push(this.patient)
      this.patient = {}
      localStorage.setItem('patients',JSON.stringify(this.patients))
      this.$bvModal.hide('modal-newbemor')
    },
    del(index){
      this.patients.splice(index,1)
      localStorage.setItem('patients',JSON.stringify(this.patients))
    }
  },
  computed: {
    filteredPatients(){
      if (this.search == 'Barchasi') return this.patients
      return this.patients.filter(patient => {
        return patient.gender == this.search
      })
    }
  },
  created(){
    if (localStorage.getItem('patients')){
      try {
        this.patients = JSON.parse(localStorage.getItem('patients'))
      } catch (error) {
        localStorage.removeItem('patients')
      }
    }
  }
}
</script>
 
<style>
  .custom-radio{
    margin-right: 10px;
  }
</style>

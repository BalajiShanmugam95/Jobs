<template>
  <div class="home">
    <Header msg="Welcome to Your Vue.js App" />
    <div class="container">
      <b-card style="min-height:500px">
        <div class="d-flex align-items-center justify-content-between">
          <h5 class="card-title text-left"> Job Lists</h5>
          <div>
            <b-button variant="primary" class="px-2 mr-3" @click="addJob(1)"><i class="fas fa-plus mr-2"></i>Add Job</b-button>
            <a href="/files/jobsList.csv" download><b-button variant="primary" class="px-2 mr-2"><i class="fas fa-file-download mr-2"></i>Download</b-button></a>
            <b-button variant="primary" class="px-2" @click="confirmDelete()" v-if="deleteIds.length"><i class="far fa-trash-alt mr-2"></i>Delete</b-button>
          </div>
        </div>
        <div class="py-2">
          <b-table class="admin-table"  ref="jobtabel" hover :current-page="currentPage" :items="items" :fields="header" :per-page="perPage" id="job-table">
            <template slot="cell(select)" slot-scope="row" >
                <input type="checkbox" class="cursor-pointer" :key="row.item.id" @click="handleCheckbox($event,row)">
            </template>
            <template slot="cell(action)" slot-scope="row" >
                  <i  v-for="(action,index) in actions" :class="'mx-2 cursor-pointer '+action" @click="handleAction(index,row)" :key="index"></i>
            </template>
          </b-table>

          <b-pagination
            v-if="items.length > perPage"
            align="center" 
            v-model="currentPage"
            :total-rows="items.length"
            :per-page="perPage"
            aria-controls="job-table">
          </b-pagination>
        </div>
      </b-card>
    </div>
    
    <b-modal ref="addjob_modal" id="addjob-modal" :title="addType==1?'Add Job':'Edit Job'" hide-footer>
        <AddJob :activejob="activeJob" :updateVal="updateValue"></AddJob>
    </b-modal>

    <b-modal ref="jobdetails_modal" id="jobdetails-modal" title="Job Details" hide-footer>
        <JobDetails :activejob="activeJob"></JobDetails>
    </b-modal>
  </div>
</template>

<script>
import $ from "jquery";
import Header from "@/components/Header.vue";
import AddJob from "./addJob";
import JobDetails from "./jobDetails"
import jobslist from '../assets/jobs.json'

export default {
  name: "Home",
  components: {
    Header,
    AddJob,
    JobDetails
  },
  data() {
    return {
      header:["select","title","description","companyName","action"],
      currentPage:1,
      items:jobslist,
      actions:["fas fa-eye","fas fa-pencil-alt"],
      perPage:9,
      addType:1,
      activeJob:{},
      deleteIds:[],
    }
  },
  methods: {
      handleAction(index, row) {
        this.activeJob = row;
        if (index == 0) {
           this.$refs.jobdetails_modal.show();
        } else {
          this.addJob(2)
        }
      },

      handleCheckbox(e,row) {
        if(e.target.checked)
          this.deleteIds.push(row.item.id)
        else{
        const index = this.deleteIds.indexOf(row.item.id);
          if (index > -1) {
            this.deleteIds.splice(index, 1);
          }
        }
        console.log( this.deleteIds)
      },

      pageChange(page) {
        this.currentPage = page;
      },

      updateValue(value, id) {
        if (id == -1)
          this.items.push(value);
        else {
          var dataset = this.items;
          dataset[id] = value;
          this.items = dataset;
          this.$refs.jobtabel.refresh();
        }
        this.alterMyids();
        this.$refs.addjob_modal.hide()
      },

      addJob(type) {
        if (type == 1) {
          this.activeJob = {};
        }
        this.addType = type;
        this.$refs.addjob_modal.show()
      },

      alterMyids() {
        this.items.forEach((element, i) => {
          element["id"] = i;
        });
      },
      confirmDelete(){
        this.$swal({
              title: "Are you sure want to delete??",
              icon: "warning",
              buttons: true,
              dangerMode: true,
              showCancelButton: true,
              confirmButtonText:"Yes",
              cancelButtonText:"No",
              confirmButtonColor: '#3085d6',
              cancelButtonColor: '#d33',
          }).then((willDelete) => {
            if (willDelete.isConfirmed) {
              this.delete_data();
            }
          })
      },
      showToast(){
        const Toast =this.$swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.addEventListener('mouseenter', this.$swal.stopTimer)
            toast.addEventListener('mouseleave', this.$swal.resumeTimer)
          }
        })
        
        Toast.fire({
          icon: 'success',
          title: 'Job(s) Deleted Successfully'
        })
      },
      delete_data(){
        var self=this;
        this.deleteIds.forEach(item =>{
          self.items.splice(self.items.findIndex(function(i){
              return i.id === item;
            }), 1)
        })
        this.currentPage=1;
        this.deleteIds=[];
        $("input[type=checkbox]").prop("checked",false);
        this.alterMyids();
        this.showToast();
      },
      clickDownload(){
        let link = document.createElement('a');
        link.setAttribute('type', 'hidden');
        link.href = '../assets/jobsList.csv';
        link.download = "../assets/jobsList.csv";
        document.body.appendChild(link);
        link.click();
        link.remove();
      }
  },
  mounted() {
    this.alterMyids();
  },
};
</script>

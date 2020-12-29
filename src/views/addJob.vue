<template>
  <div class="addjob">
     <form id="addjobfrom">
            <div class="row m-0">
                <div class="col-12">
                    <div class="form-group">
                    <label for="jobtitle" class="title-text">Job Title <span class="text-danger">*</span></label>
                    <input class="form-control" v-model="title" type="text" name="title" required id="jobtitle"/>
                    <label class="mb-0 error-hint">Please enter Job title</label>
                    </div>
                </div>
            </div>
            <div class="row m-0">
                <div class="col-12">
                    <div class="form-group">
                    <label for="company" class="title-text">Company Name <span class="text-danger">*</span></label>
                    <input class="form-control" v-model="company" type="text" name="company" required id="company"/>
                    <label class="mb-0 error-hint">Please enter Company Name</label>
                    </div>
                </div>
            </div>
            <div class="row m-0">
                <div class="col-12">
                    <div class="form-group ">
                    <label for="jobtitle" class="title-text">Description</label>
                    <textarea class="form-control" v-model="desc" rows="3" type="text" id="description"/>
                    </div>
                </div>
            </div>
      </form>
      <div class="row mx-0 my-2">
          <div class="col-12">
              <div class="object-fit float-right">
                 <b-button variant="primary" @click="saveJobs()">Save</b-button>
              </div>
          </div>
      </div>
  </div>
</template>
<script>
import $ from "jquery";
import validate from 'jquery-validation';
export default {
    name:"AddJob",
    props:["activejob","updateVal"],
    data() {
        return {
            title:"",
            company:"",
            desc:"",
            valu:validate,
        }
    },
    methods: {
        saveJobs(){
            if($("#addjobfrom").valid()){
                var addDetails={
                    "title":this.title,
                    "description":this.desc,
                    "companyName":this.company
                }
                this.updateVal(addDetails,(this.activejob.item?this.activejob.item.id:-1))
            }
        }
    },
    mounted(){
        if(this.activejob.item){
            var items=this.activejob.item;
            this.title=items.title;
            this.desc=items.description;
            this.company=items.companyName;
        }
    }
}
</script>

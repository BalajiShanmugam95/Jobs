<template>
  <div class="jobdetails m-2">
     <div>
         <label class="detailslabel">Title :</label>
         <span>{{this.activejob.item.title}}</span>
     </div>
     <div>
         <label class="detailslabel">Company Name :</label>
         <span>{{this.activejob.item.companyName}}</span>
     </div>
     <div>
         <label class="detailslabel">Description :</label>
         <span class="text-warp">{{this.activejob.item.description}}</span>
     </div>
  </div>
</template>
<script>

export default {
    name:"JobDetails",
    props:["activejob"],
}
</script>
<style scoped>
.detailslabel{
    font-size: 10pt;
    font-weight: bold;
    min-width: 150px;
}
</style>

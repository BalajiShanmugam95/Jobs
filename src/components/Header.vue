<template>
    <div class="container-fluid">
        <div class="d-flex align-items-center justify-content-end py-3">
            <label class="mr-2 font-weight-bold mb-0">Hi, Admin</label>
            <b-button variant="primary" class="px-4" @click="logout()">Logout</b-button>
        </div>
    </div>
</template>
<script>
export default {
    name:"Header",
    methods: {
        logout(){
            localStorage.clear();
            this.$router.push('/')
        }
    },
}
</script>
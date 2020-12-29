  <template>
  <div class="app login_bg">
    <div class="container d-flex align-items-center width-fit justify-content-center h-100">
      <div class="login-form">
          <b-card no-body class="bg-transparent border-0">
              <b-card-body>
                <b-form>
                   <div class="col-lg-12 login-key">
                        <i class="fa fa-key" aria-hidden="true"></i>
                    </div>
                    <div class="col-lg-12 login-title">
                        ADMIN PANEL
                    </div>
                  <b-input-group class="mb-3">
                    <b-input-group-prepend><b-input-group-text><i class="fas fa-at"></i></b-input-group-text></b-input-group-prepend>
                    <b-form-input type="text" v-model="username" class="form-control" placeholder="Username" autocomplete="username email" />
                  </b-input-group>
                  <b-input-group class="mb-4">
                    <b-input-group-prepend><b-input-group-text><i class="fas fa-lock"></i></b-input-group-text></b-input-group-prepend>
                    <b-form-input type="password" v-model="password" class="form-control" placeholder="Password" autocomplete="current-password" />
                  </b-input-group>
                  <div v-if="isloginerror" class="text-center">
                      <label class="text-warning">Login Failed !</label>
                  </div>
                  <b-row>
                    <b-col cols="12">
                      <b-button variant="primary" class="px-4" @click="auth()">Login</b-button>
                    </b-col>
                  </b-row>
                </b-form>
              </b-card-body>
            </b-card>
      </div>
    </div>
  </div>
</template>
<script>
export default {
    name:"Login",
    data() {
        return {
            username:"",
            password:"",
            UserAuth:{
                username:"admin@admin.com",
                pass:"123456",
            },
            isloginerror:false,
        }
    },
    beforeCreate()
     {
      if (localStorage.isUserloggedin) {
            this.$router.push('/home');
        }  
     },
    methods: {
        auth(){
            this.isloginerror=false;
            if(this.username === this.UserAuth.username && this.password === this.UserAuth.pass){
                localStorage.isUserloggedin=true;
                this.$router.push('/home');
            }
            else
                this.isloginerror=true;
        }
    },
}
</script>

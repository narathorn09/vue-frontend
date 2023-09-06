<template>
  <v-from ref="LoginForm" :lazy-validation="lazy">
    <v-card class="card-container">
      <v-card-title> เข้าสู่ระบบ </v-card-title>
      <v-card-text>
        <v-row donse>
          <v-col cols="12" align="start">
            <sapn>ชื่อผู้ใช้</sapn>
            <v-text-field
              v-model="username"
              :rules="Rules.usernameRules"
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row donse>
          <v-col cols="12" align="start">
            <sapn>รหัสผ่าน</sapn>
            <v-text-field
              v-model="password"
              :rules="Rules.passwordRules"
            ></v-text-field>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-btn block class--text @click="login()">เข้าสู่ระบบ</v-btn>
      </v-card-actions>
    </v-card>
  </v-from>
</template>

<script>
export default {
  name: "LoginPage",
  data() {
    return {
      lazy: false,
      username: "",
      password: "",
      Rules: {
        usernameRules: [(v) => !!v || "กรุณากรอกชื่อผู้ใช้งาน"],
        passwordRules: [(v) => !!v || "กรุณากรอกรหัสผ่าน"],
      },
    };
  },
  methods: {
    login() {
      //   if (this.$refs.LoginForm.validate(true)) {
      localStorage.setItem("username", this.username);
      this.$EventBus.$emit("getUsername");
      this.$EventBus.$emit("checkLogin");
      this.$router.push({ path: "/" }).catch(() => {});
      //   }
    },
  },
};
</script>

<style scoped>
.card-container {
  width: 50%;
  height: 100%;
}
</style>

<template>
  <div id="Login" class="center-both">
    <v-container fill-height fluid>
      <v-card class="mx-auto rounded-lg" max-width="900px" min-width="700px" outlined>
        <v-card-title class="headline">Login</v-card-title>
        <v-list-item three-line>
          <v-row no-gutters>
            <v-col class="col-2">
              <v-icon class="center-both">mdi-account</v-icon>
            </v-col>
            <v-col class="col-10">
              <v-text-field
                class="pa-2"
                label="Usuario"
                required
                @keyup.enter="login"
                v-model="username"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-list-item>
        <v-list-item three-line>
          <v-row no-gutters>
            <v-col class="col-2">
              <v-icon class="center-both">mdi-textbox-password</v-icon>
            </v-col>
            <v-col class="col-10">
              <v-text-field
                type="password"
                class="pa-2"
                label="Contraseña"
                required
                @keyup.enter="login"
                v-model="password"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-list-item>
        <v-card-actions>
          <v-btn class="mx-auto" x-large color="primary" @click="login" @keyup.enter="login">Entrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    login() {
      this.$store
        .dispatch("auth/login", {
          username: this.username,
          password: this.password,
        })
        .then(() => {
          this.$router.push("home");
        })
        .catch((err) => {
          console.error(err);
        });
    },
  },
};
</script>

<style lang="css">
.center-both {
  width: 100%;
  height: 100%;
}
</style>

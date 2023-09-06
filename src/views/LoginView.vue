<template lang="">
  <v-container>
    <v-row justify="center" alihn="center">
      <v-col cols="12" align="center">
        <LoginPage />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  components: { LoginPage: () => import("@/components/LoginPage.vue") },
};
</script>

<style scoped></style>

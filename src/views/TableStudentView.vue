<template>
    <v-container>
      <v-row>
        <v-col cols="12">
          <TableStudent />
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import TableStudent from "../components/TableStudent";
  
  export default {
    components: {
      TableStudent,
    },
  };
  </script>
  
  <style></style>
  
<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="studentItem"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Table Student</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            dark
            class="mb-2"
            @click="openDialog('add', defaultItem)"
          >
            เพิ่มข้อมูล
          </v-btn>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="openDialog('edit', item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">{{ formTitle }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="studentId"
                  label="รหัสนักศึกษา"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field v-model="fname" label="ชื่อ"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field v-model="lname" label="นามสกุล"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field v-model="email" label="Email"></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="close()"> ยกเลิก </v-btn>
          <v-btn color="blue darken-1" text @click="save(formTitle)">
            บันทึกข้อมูล
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title class="text-h5"
          >Are you sure you want to delete this item?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="deleteItemConfirm"
            >OK</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: "",
    studentId: "",
    fname: "",
    lname: "",
    email: "",
    dialog: false,
    dialogDelete: false,
    studentItem: [],
    formTitle: "",
    idFordelete: "",
    headers: [
      {
        text: "ID",
        align: "start",
        sortable: false,
        value: "id",
      },
      { text: "รหัสนักศึกษา", value: "studentId" },
      { text: "ชื่อ", value: "fname" },
      { text: "นามสกุล", value: "lname" },
      { text: "Email", value: "email" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    openDialog(Action, item) {
      this.formTitle = "";
      if (Action === "add") {
        this.dialog = true;
        this.formTitle = "เพิ่มข้อมูล";
      } else {
        console.log(Action, item);
        this.dialog = true;
        this.formTitle = "แก้ไขข้อมูล";
        this.id = item.id;
        this.studentId = item.studentId;
        this.fname = item.fname;
        this.lname = item.lname;
        this.email = item.email;
      }
    },

    close() {
      this.dialog = false;
      this.id = null;
      this.studentId = "";
      this.fname = "";
      this.lname = "";
      this.email = "";
    },

    closeDelete() {
      this.dialogDelete = false;
    },

    deleteItem(item) {
      console.log("item", item.id);
      this.idFordelete = item.id;
      this.dialogDelete = true;
    },

    async initialize() {
      this.studentItem = [];
      try {
        var response = await this.axios.get("http://localhost:9000/students");
        console.log("data employee ====>", response.data);
        this.studentItem = response.data;
      } catch (error) {
        console.log("error", error);
      }
    },

    async save(action) {
      var data = {
        studentId: this.studentId,
        fname: this.fname,
        lname: this.lname,
        email: this.email,
      };

      if (action === "เพิ่มข้อมูล") {
        try {
          var dataResponse = await this.axios.post(
            "http://localhost:9000/students",
            data
          );
          console.log("dataReponse ===>", dataResponse);
          this.close();
          this.initialize();
        } catch (error) {
          console.log(error.massage);
        }
      } else {
        try {
          var dataReponseEdit = await this.axios.put(
            "http://localhost:9000/student/" + this.id,
            data
          );
          console.log("dataReponseEdit ===>", dataReponseEdit);
          this.close();
          this.initialize();
        } catch (error) {
          console.log(error.massage);
        }
      }
      this.close();
    },

    async deleteItemConfirm() {
      try {
        var response = await this.axios.delete(
          "http://localhost:9000/student/" + this.idFordelete
        );
        console.log("response ===>", response);
        this.initialize();
      } catch (error) {
        console.log(error.massage);
      }
      this.closeDelete();
    },
  },
};
</script>

<style></style>

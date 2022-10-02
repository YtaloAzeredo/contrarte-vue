<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <h1>Cadastro de Usuários</h1>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col cols="12">
        <v-data-table dense :headers="headers" :items="users" item-key="name" class="elevetion-1">
          <template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title>Usuários</v-toolbar-title>
              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="500px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                    Novo Cadastro
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="headline">{{ formTitle }}</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12" sm="6" md="2">
                          <v-text-field v-model="editedItem.id" label="Id">
                          </v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="8">
                          <v-text-field v-model="editedItem.name" label="Nome">
                          </v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                          <v-text-field v-model="editedItem.email" label="Email">
                          </v-text-field>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field v-model="editedItem.location" label="Localização">
                          </v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close"> Cancelar </v-btn>
                    <v-btn color="blue darken-1" text @click="save"> Salvar </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-dialog v-model="dialogDelete" max-with="500px">
                <v-card>
                  <v-card-title class="headline">
                    Realmente deseja deletar este item?
                  </v-card-title>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="closeDelete"> Cancelar </v-btn>
                    <v-btn color="blue darken-1" text @click="deleteItemConfirm"> Ok </v-btn>
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          <!-- <template v-slot:items.actions="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
            <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
          </template> -->
          <template v-slot:no-data>
            <v-btn color="primary" @click="initialize"> Reset </v-btn>
          </template>
        </v-data-table>
      </v-col>
    </v-row>    
  </v-container>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'UserCrud',  
    data: () => ({
      dialog: false,
      dialogDelete: false,
      headers: [
        { text: "Id", value: "id" },
        { text: "Nome", value: "name" },
        { text: "Email", value: "email" },
        { text: "Localização", value: "location" },
        { text: "Actions", value: "actions", sortable: false },
      ],
      users: [],
      editedIndex: 0,
      editedItem: {
        id: "",
        name: "",
        email: "",
        location: ""
      },
      defaultItem: {
        id: "",
        name: "",
        email: "",
        location: ""
      }
    }),
    methods: {
      start() {
        axios("http://localhost:3000/users")
          .then((response) => {
            this.users = response.data
          })
          .catch((error) => console.log(error))
      },
      close() {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      }
    },
    created() {
      this.start()
    }
  };
  </script>
  
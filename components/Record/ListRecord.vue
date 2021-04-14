<template lang="html">
  <v-container grid-list-xs>
    <template>
      <v-card>
        <v-card-title>
          Estado evolutivo de los casos
          <v-spacer></v-spacer>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="records"
          :search="search"
          item-key="name"
          sort-by="name"
          class="elevation-1"
          show-group-by
        >
          <template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title>My CRUD</v-toolbar-title>
              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="500px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    dark
                    class="mb-2"
                    v-bind="attrs"
                    v-on="on"
                  >
                    Agregar Expediente
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="headline">{{ formTitle }}</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.record_number"
                            label="EXP/Nº"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.title"
                            label="Nombre"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.expertise_status"
                            label="Estado Pericial"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.payment_status"
                            label="Estado de Cobro"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.arrangement_type"
                            label="Tipo de Arreglo"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.updated_state"
                            label="Estado Actualizado"
                          ></v-text-field>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close">
                      Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save">
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-dialog v-model="dialogDelete" max-width="500px">
                <v-card>
                  <v-card-title class="headline"
                    >Are you sure you want to delete this item?</v-card-title
                  >
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="closeDelete"
                      >Cancel</v-btn
                    >
                    <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                      >OK</v-btn
                    >
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
          </template>
        </v-data-table>
      </v-card>
      sjaopdsdpoajdsa
    </template>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      search: '',
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'EXP/Nº',
          align: 'left',
          value: 'record_number',
          groupable: false,
        },
        { text: 'Nombre', value: 'title', align: 'left', groupable: false },
        {
          text: 'Estado Pericial',
          value: 'expertise_status',
          align: 'right',
          groupable: false,
        },
        {
          text: 'Estado de Cobro',
          value: 'payment_status',
          align: 'right',
          groupable: false,
        },
        {
          text: 'Tipo de Arreglo',
          value: 'arrangement_type',
          align: 'right',
          groupable: false,
        },
        {
          text: 'Estado Actualizado',
          value: 'updated_state',
          align: 'right',
          groupable: false,
        },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      records: [],
      editedIndex: -1,
      editedItem: {
        record_number: '',
        title: '',
        expertise_status: '',
        payment_status: '',
        arrangement_type: '',
        updated_state: '',
      },
      defaultItem: {
        record_number: '',
        title: '',
        expertise_status: '',
        payment_status: '',
        arrangement_type: '',
        updated_state: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1
        ? 'Agregar Expediente'
        : 'Editar Expediente'
    },
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  methods: {
    async getRecords() {
      const path = 'records/'
      await this.$axios
        .get(path)
        .then((response) => {
          this.records = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    editItem(item) {
      this.editedIndex = this.records.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.records.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    async deleteItemConfirm() {
      try {
        const resp = await this.$axios.delete(`records/${this.editedItem.id}/`)
        this.records.splice(this.editedIndex, 1)
        console.log(resp)
      } catch (error) {}
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    async save() {
      if (this.editedIndex > -1) {
        // EDIT
        try {
          const resp = await this.$axios.put(
            `records/${this.editedItem.id}/`,
            this.editedItem
          )
          Object.assign(this.records[this.editedIndex], this.editedItem)
          console.log(resp)
        } catch (error) {}
      } else {
        // CREATE
        try {
          const resp = await this.$axios.post(`records/`, this.editedItem)
          this.records.push(this.editedItem)
          console.log(resp)
        } catch (error) {}
      }
      this.close()
    },
  },

  created() {
    this.getRecords()
  },
}
</script>
<style lang=""></style>

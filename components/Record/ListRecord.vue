<template lang="html">
  <v-container grid-list-xs>
    <template>
      <v-card>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Buscar"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="records"
          :search="search"
          item-key="title"
          sort-by="title"
          class="elevation-1"
          show-group-by
          single-expand
          :expanded.sync="expanded"
          show-expand
        >
          <template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title class="text-uppercase font-weight-thin">
                Estado evolutivo de los casos</v-toolbar-title
              >
              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="900px">
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
                        <v-col cols="12" sm="6" md="2">
                          <v-text-field
                            v-model="editedItem.record_number"
                            label="EXP/Nº"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.title"
                            label="Carátula"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="3">
                          <v-select
                            v-model="editedItem.dependency"
                            :items="select_dependency"
                            label="Dependencia"
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6" md="3">
                          <v-select
                            v-model="editedItem.city"
                            :items="select_city"
                            label="Ciudad"
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-select
                            v-model="editedItem.expertise_status"
                            :items="select_expertise_status"
                            label="Estado Pericial"
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-select
                            v-model="editedItem.payment_status"
                            :items="select_payment_status"
                            label="Estado de Cobro"
                          ></v-select>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-select
                            v-model="editedItem.arrangement_type"
                            :items="select_arrangement_type"
                            label="Arreglo"
                          ></v-select>
                        </v-col>
                        <v-col cols="12">
                          <v-textarea
                            filled
                            name="input-7-4"
                            v-model="editedItem.action_lawyer"
                            label="Acción Letrado"
                            no-resize
                            rows="1"
                          ></v-textarea>
                        </v-col>
                        <v-col cols="12">
                          <v-textarea
                            filled
                            name="input-7-4"
                            v-model="editedItem.comments"
                            label="Comentarios"
                            no-resize
                            rows="3"
                          ></v-textarea>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close">
                      Cancelar
                    </v-btn>
                    <v-btn color="green darken-3" @click="save">
                      Guardar
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-dialog v-model="dialogDelete" max-width="600px">
                <v-card>
                  <v-card-title class="headline red--text"
                    >¡Este elemento se elminará definitivamente!</v-card-title
                  >
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="closeDelete"
                      >Cancelar</v-btn
                    >
                    <v-btn color="red" @click="deleteItemConfirm"
                      >CONFIRMAR</v-btn
                    >
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          <template v-slot:item.payment_status="{ item }">
            <v-chip
              :color="getColor(item.payment_status)"
              text-color="black"
              dark
            >
              {{ item.payment_status }}
            </v-chip>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
          </template>
          <template v-slot:expanded-item="{ headers, item }">
            <td
              :colspan="headers.length / 2"
              class="blue-grey darken-4 text-left pa-3"
            >
              <span class="blue-grey--text text--lighten-3">COMENTARIOS</span>
              <br />
              <br />
              <span> {{ item.comments }}</span>
            </td>
            <td
              :colspan="headers.length / 4"
              class="blue-grey darken-4 text-center text-uppercase"
            >
              {{ item.dependency }}
            </td>
            <td
              :colspan="headers.length / 4"
              class="blue-grey darken-4 text-center text-uppercase"
            >
              {{ item.city }}
            </td>
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
      expanded: [],
      search: '',
      dialog: false,
      dialogDelete: false,
      select_expertise_status: [
        'Aceptado',
        'Realizado',
        'Terminado',
        'Imp-Respondido',
      ],
      select_payment_status: [
        'FEO',
        'ATENCION',
        'NORMAL',
        'TRATATIVA',
        'COBRADO',
      ],
      select_arrangement_type: ['AH', 'AEJ', 'SPI', 'SC', 'SF'],
      select_dependency: ['Civil I', 'Familia'],
      select_city: ['Oberá', 'Aristóbulo del Valle'],
      headers: [
        {
          text: 'EXP/Nº',
          align: 'left',
          value: 'record_number',
          groupable: false,
        },
        { text: 'Carátula', value: 'title', align: 'left', groupable: false },
        {
          text: 'Estado Pericial',
          value: 'expertise_status',
          align: 'left',
          groupable: false,
          cellClass: 'text-uppercase',
        },
        {
          text: 'Estado de Cobro',
          value: 'payment_status',
          align: 'center',
          groupable: false,
        },
        {
          text: 'Arreglo',
          value: 'arrangement_type',
          align: 'center',
          groupable: false,
        },
        {
          text: 'Acción Letrado',
          value: 'action_lawyer',
          align: 'center',
          groupable: false,
        },
        {
          text: 'Acciones',
          value: 'actions',
          align: 'center',
          sortable: false,
          groupable: false,
        },
        {
          text: 'Otros',
          value: 'data-table-expand',
          align: 'center',
          sortable: false,
          groupable: false,
        },
      ],
      records: [],
      editedIndex: -1,
      editedItem: {
        record_number: '',
        title: '',
        expertise_status: '',
        payment_status: '',
        arrangement_type: '',
        action_lawyer: '',
        comments: '',
        dependency: '',
        city: '',
      },
      defaultItem: {
        record_number: '',
        title: '',
        expertise_status: '',
        payment_status: '',
        arrangement_type: '',
        action_lawyer: '',
        comments: '',
        dependency: '',
        city: '',
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

    getColor(payment_status) {
      if (payment_status === 'FEO') return 'red'
      else if (payment_status === 'ATENCION') return 'yellow'
      else if (payment_status === 'NORMAL') return 'green'
      else if (payment_status === 'TRATATIVA') return 'blue'
      else return 'deep-purple'
    },
  },

  created() {
    this.getRecords()
  },
}
</script>
<style lang=""></style>

<template>
 <v-container fluid>
   <v-data-iterator
      :items="disciplinas"
      :search="search"
      >
    <template v-slot:header >
    <v-toolbar color="#FAFAFA" class="mb-1">
      <v-text-field
        v-model="search"
        clearable
        flat
        solo-inverted
        hide-details
        prepend-inner-icon="mdi-magnify"
        label="Pesquisar"
      ></v-text-field>
    </v-toolbar>
    </template>
    <template v-if="disciplina && disciplinas.length" v-slot='pro'>
      <v-row >
        <v-col
          v-for="disciplina in pro.items"
          :key="disciplina.id"
          cols="16"
          sm="12"
          md="6"
          lg="4"
        >
          <v-card elevation="4" class="my-2" dark>
            <v-card-title id="titulo" dark class="text-body-1">{{
              disciplina.nome
            }}</v-card-title>
            <v-card-actions class="corpo">
              <template>
                <v-col cols="auto">
                  <v-dialog max-width="600">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn text v-bind="attrs" v-on="on">
                        <v-icon small>mdi-pencil</v-icon>
                        EDITAR
                      </v-btn>
                    </template>
                    <template v-slot:default="dialog2">
                      <v-card>
                        <v-card-title class="headline"
                          >EDITAR DISCIPLINA
                        </v-card-title>
                        <v-card-text>
                          <v-container>
                            <v-form ref="form" v-model="isValid">
                              <v-text-field
                                label="nome"
                                v-model="disciplina.nome"
                              >
                                {{ disciplina.nome }}
                              </v-text-field>
                              <v-text-field
                                label="abreviatura"
                                v-model="disciplina.abreviatura"
                              >
                                {{ disciplina.abreviatura }}
                              </v-text-field>
                              <v-text-field
                                label="cr??ditos"
                                v-model="disciplina.credito"
                              >
                                {{ disciplina.credito }}
                              </v-text-field>
                              <v-row align="center">
                                <v-col>
                                  <v-select
                                    v-model="disciplina.professores"
                                    :items="professores"
                                    item-text="nome"
                                    item-value="id"
                                    label="professores"
                                    multiple
                                  >
                                  </v-select>
                                </v-col>
                              </v-row>
                              <v-row align="center">
                                <v-col>
                                  <v-select
                                    v-model="disciplina.locais"
                                    :items="salas"
                                    item-text="nome"
                                    item-value="id"
                                    label="locais"
                                    multiple
                                  >
                                  </v-select>
                                </v-col>
                              </v-row>
                            </v-form>
                          </v-container>
                        </v-card-text>
                        <v-spacer></v-spacer>
                        <v-card-actions class="justify-end">
                          <v-spacer></v-spacer>
                          <v-btn text @click="dialog2.value = false"
                            >VOLTAR</v-btn
                          >
                          <v-btn
                            color="success"
                            @click="
                              atualizarDisciplina(disciplina.id, disciplina)
                              ,dialog2.value = false
                            "
                            >ATUALIZAR</v-btn
                          >
                        </v-card-actions>
                      </v-card>
                    </template>
                  </v-dialog>
                </v-col>
              </template>
              <template>
                <v-col cols="auto">
                  <v-dialog max-width="600">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn text v-bind="attrs" v-on="on">
                        <v-icon small>mdi-delete</v-icon>
                        REMOVER
                      </v-btn>
                    </template>
                    <template v-slot:default="dialog2">
                      <v-card>
                        <v-card-title class="text-h5 red white--text lighten-2"
                          >EXCLUIR</v-card-title
                        ><v-card-text></v-card-text>
                        <v-card-text class="text-md-body-1 black--text">
                          Deseja remover {{ disciplina.nome }}?
                        </v-card-text>
                        <v-divider></v-divider>
                        <v-card-actions class="justify-end">
                          <v-spacer></v-spacer>
                          <v-btn text @click="dialog2.value = false"
                            >voltar</v-btn
                          >
                          <v-btn
                            color="error"
                            @click="deleteDisciplina(disciplina.id)"
                            >REMOVER</v-btn
                          >
                        </v-card-actions>
                      </v-card>
                    </template>
                  </v-dialog>
                </v-col>
              </template>
              <v-btn text>
                <v-icon small>mdi-format-list-bulleted-square</v-icon>detalhes
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </template>
    </v-data-iterator>
    <template>
      <v-row justify="center">
        <v-dialog v-model="dialog" persistent max-width="600px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              v-bind="attrs"
              v-on="on"
              fixed
              bottom
              right
              fab
            >
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">Nova disciplina</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form ref="form" v-model="isValid">
                  <v-text-field
                    required
                    :rules="[(v) => !!v || 'Precisamos disso :c']"
                    label="nome"
                    v-model="disciplina.nome"
                  ></v-text-field>
                  <v-text-field
                    label="abreviatura"
                    v-model="disciplina.abreviatura"
                  ></v-text-field>
                  <v-text-field
                    required
                    :rules="[(v) => !!v || 'Precisamos disso :c']"
                    label="cr??ditos"
                    v-model="disciplina.credito"
                  ></v-text-field>
                  <v-row align="center">
                    <v-col>
                      <v-select
                        v-model="selectedTeacher"
                        :items="professores"
                        item-text="nome"
                        item-value="id"
                        label="professores"
                        multiple
                      ></v-select>
                    </v-col>
                  </v-row>
                  <v-row align="center">
                    <v-col>
                      <v-select
                        v-model="selectedPlaces"
                        :items="salas"
                        item-text="nome"
                        item-value="id"
                        label="locais"
                        multiple
                      ></v-select>
                    </v-col>
                  </v-row>
                </v-form>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn text @click="dialog = false"> Voltar </v-btn>
              <v-btn
                :disabled="!isValid"
                color="success"
                text
                @click="cadastrarDisciplina()"
              >
                Cadastrar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </template>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      search: '',
      disciplinas: "",
      disciplina: {
        nome: null,
        abreviatura: null,
        credito: null,
      },
      selectedTeacher: null,
      selectedPlaces: null,
      dialog: false,
      isValid: true,
    };
  },
  mounted() {
    this.PegarDisciplinas()
    this.axios
      .get("http://otime-api2.herokuapp.com/professores/")
      .then((response) => (this.professores = response.data))
      .catch((error) => console.log("Erro na requisi????o GET: " + error));
    this.axios
      .get("http://otime-api2.herokuapp.com/salas/")
      .then((response) => (this.salas = response.data))
      .catch((error) => console.log("Erro na requisi????o GET: " + error));
  },
  methods: {
    cadastrarDisciplina() {
      const disciplina = {
        nome: this.disciplina.nome,
        abreviatura: this.disciplina.abreviatura,
        credito: this.disciplina.credito,
        professores: this.selectedTeacher,
        locais: this.selectedPlaces,
      };
      this.axios
        .post("http://otime-api2.herokuapp.com/disciplinas/", disciplina)
        .then(
          (response) =>
            (this.disciplinas = [...this.disciplinas, response.data])
        )
        .catch((error) => console.log(error));
      this.dialog = false;
      this.disciplina.nome = null;
      this.disciplina.abreviatura = null;
      this.disciplina.credito = null;
    },
    atualizarDisciplina(disciplinaId, disciplina) {
      this.axios
        .put(
          "https://otime-api2.herokuapp.com/disciplinas/" + disciplinaId + "/",
          {
            nome: disciplina.nome,
            abreviatura: disciplina.abreviatura,
            credito: disciplina.credito,
            professores: disciplina.professores,
            locais: disciplina.locais,
          }
        )
        .then((response) => {
          this.disciplina = response.data;
          this.PegarDisciplinas();
          this.dialog2 = false;
          })
        .catch((error) => console.log(error));
    },
    deleteDisciplina(disciplinaId) {
      this.axios
        .delete(
          "http://otime-api2.herokuapp.com/disciplinas/" + disciplinaId + "/"
        )
        .then(() => {
          this.disciplinas = this.disciplinas.filter(
            (p) => p.id != disciplinaId
          );
        });
    },
    PegarDisciplinas(){
      this.axios
      .get("http://otime-api2.herokuapp.com/disciplinas/")
      .then((response) => (this.disciplinas = response.data))
      .catch((error) => console.log("Erro na requisi????o GET: " + error));
    }
  },
};
</script>
<style></style>

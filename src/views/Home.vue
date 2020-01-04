<template>
  <div id="home">
    <v-app>
      <v-navigation-drawer v-model="drawer" :clipped="$vuetify.breakpoint.lgAndUp" app>
        <v-list dense>
          <template v-for="item in items">
            <v-layout v-if="item.heading" :key="item.heading" row align-center>
              <v-flex xs6>
                <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
              </v-flex>
              <v-flex xs6 class="text-xs-center">
                <a href="#!" class="body-2 black--text">EDIT</a>
              </v-flex>
            </v-layout>
            <v-list-group
              v-else-if="item.children"
              :key="item.text"
              v-model="item.model"
              :prepend-icon="item.model ? item.icon : item['icon-alt']"
              append-icon
            >
              <template v-slot:activator>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>{{ item.text }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </template>
              <v-list-item v-for="(child, i) in item.children" :key="i">
                <v-list-item-action v-if="child.icon">
                  <v-icon>{{ child.icon }}</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>{{ child.text }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-group>
            <v-list-item v-else :key="item.text">
              <v-list-item-action>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>{{ item.text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-list>
      </v-navigation-drawer>

      <v-app-bar :clipped-left="$vuetify.breakpoint.lgAndUp" app color="blue darken-3" dark>
        <v-toolbar-title style="width: 300px" class="ml-0 pl-3">
          <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
          <span class="hidden-sm-and-down">Empório do caldo</span>
        </v-toolbar-title>

        <v-spacer></v-spacer>
        <v-btn icon>
          <v-icon>mdi-cellphone-android</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon>mdi-bell</v-icon>
        </v-btn>
        <v-btn icon large>
          <v-avatar size="32px" item>
            <v-img src="https://cdn.vuetifyjs.com/images/logos/logo.svg" alt="Vuetify"></v-img>
          </v-avatar>
        </v-btn>
      </v-app-bar>
      <v-content>
        <v-container>
          <v-row>
            <v-layout class="col-sm-8">
              <v-tabs v-model="activeTab" grow>
                <v-tab key="1">
                  <v-icon left>mdi-account</v-icon>Dados do Cliente
                </v-tab>
                <v-tab key="2">
                  <v-icon left>mdi-lock</v-icon>Fazer Pedido
                </v-tab>
                <v-tab-item>
                  <v-card class="p-3" flat>
                    <div class="pt-5">
                      <h6>Dados pessoais</h6>
                    </div>
                    <v-row class="mt-2">
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field dense v-model="datacliente.name" label="Nome" outlined></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field
                          v-model="datacliente.tel"
                          type="number"
                          label="Telefone"
                          outlined
                          dense
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <div>
                      <h6>Endereço</h6>
                    </div>
                    <v-row class="mt-2">
                      <v-col cols="12" sm="4" md="6">
                        <v-autocomplete
                          v-model="datacliente.district"
                          label="Bairro"
                          item-value="components"
                          v-bind:value="components"
                          outlined
                          dense
                          :items="components"
                        ></v-autocomplete>
                      </v-col>
                      <v-col cols="12" sm="4" md="6">
                        <v-text-field
                          label="
                      Rua"
                          dense
                          v-model="datacliente.street"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="12" sm="4" md="2">
                        <v-text-field dense v-model="datacliente.number" label="Nº" outlined></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="4" md="10">
                        <v-text-field
                          label="
                      Complemento"
                          dense
                          v-model="datacliente.complement"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-btn block @click="confirm()" color="primary">Confirmar</v-btn>
                  </v-card>
                </v-tab-item>
                <v-tab-item>
                  <v-card class="p-3" flat>
                    <div class="d-flex justify-content-between">
                      <div class="col-sm-6">
                        <v-text-field
                          label="
                      Buscar"
                          dense
                          append-icon="mdi-magnify"
                          outlined
                        ></v-text-field>
                      </div>
                      <div class="col-sm-6">
                        <v-select dense :items="products" outlined label="Produto"></v-select>
                      </div>
                    </div>
                    <div>
                      <v-row>
                        <div class="container-1 col-sm-6">
                          <v-list shaped>
                            <h6>Tamanho</h6>
                            <v-list-item-group v-model="mountPast.Tamanho" multiple>
                              <template>
                                <v-divider></v-divider>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle}">
                                    <v-list-item-content>
                                      <v-list-item-title>500 gramas</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle }">
                                    <v-list-item-content>
                                      <v-list-item-title>850 gramas</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                              </template>
                            </v-list-item-group>
                          </v-list>
                          <v-list shaped>
                            <h6>Molhos</h6>
                            <v-list-item-group v-model="mountPast.Molhos" multiple>
                              <template>
                                <v-divider></v-divider>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle }">
                                    <v-list-item-content>
                                      <v-list-item-title>Bolonhesa</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle }">
                                    <v-list-item-content>
                                      <v-list-item-title>4 queijos</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle }">
                                    <v-list-item-content>
                                      <v-list-item-title>Sugo</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                                <v-list-item>
                                  <template v-slot:default="{ active, toggle }">
                                    <v-list-item-content>
                                      <v-list-item-title>Branco</v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-action>
                                      <v-checkbox
                                        :input-value="active"
                                        color="deep-purple accent-4"
                                        @click="toggle"
                                      ></v-checkbox>
                                    </v-list-item-action>
                                  </template>
                                </v-list-item>
                              </template>
                            </v-list-item-group>
                          </v-list>
                        </div>
                        <div class="container-2 col-sm-6">
                          <v-list shaped>
                            <h6>Ingredientes</h6>
                            <div class="d-flex justify-content-between">
                              <div>
                                <v-list-item-group v-model="mountPast.Ingredientes" multiple>
                                  <template>
                                    <v-divider></v-divider>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Azeitona</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Bacon</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Calabresa</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Milho</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Palmito</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Ervilha</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Brocolis</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                  </template>
                                </v-list-item-group>
                              </div>
                              <div>
                                <v-list-item-group v-model="mountPast.Ingredientes" multiple>
                                  <template>
                                    <v-divider></v-divider>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Azeitona</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Bacon</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Calabresa</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Milho</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Palmito</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Ervilha</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                    <v-list-item>
                                      <template v-slot:default="{ active, toggle }">
                                        <v-list-item-content>
                                          <v-list-item-title>Brocolis</v-list-item-title>
                                        </v-list-item-content>
                                        <v-list-item-action>
                                          <v-checkbox
                                            :input-value="active"
                                            color="deep-purple accent-4"
                                            @click="toggle"
                                          ></v-checkbox>
                                        </v-list-item-action>
                                      </template>
                                    </v-list-item>
                                  </template>
                                </v-list-item-group>
                              </div>
                            </div>
                          </v-list>
                          <v-btn block @click="confirmPast()" color="primary">Confirmar</v-btn>
                        </div>
                      </v-row>
                    </div>
                  </v-card>
                </v-tab-item>
              </v-tabs>
            </v-layout>
            <v-spacer></v-spacer>
            <v-layout max-width="350px" class="col-sm-4 p-3 bg-white">
              <div>
                <h5>Resumo pedido</h5>
                <v-list>
                  <v-subheader>Dados do Cliente</v-subheader>
                  <div v-if="confirmar">
                    <v-list-item>
                      <v-list-item-content>
                        <v-list-item-title>{{datacliente.name}} - {{datacliente.tel}}</v-list-item-title>
                        <v-list-item-subtitle>
                          {{datacliente.street}}, nº {{datacliente.number}}, {{datacliente.district}}
                          <br />
                          {{datacliente.complement}}
                        </v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                  </div>
                  <div v-else>
                    <v-list-item>
                      <v-list-item-content>
                        <v-alert outlined dense type="info">Vazio !</v-alert>
                      </v-list-item-content>
                    </v-list-item>
                  </div>
                </v-list>
                <v-list>
                  <v-subheader>Pedido</v-subheader>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title>1x caldo de Frango</v-list-item-title>
                      <v-list-item-subtitle>Caldo de frango, queijo, cheiro-verde</v-list-item-subtitle>
                    </v-list-item-content>
                    <v-list-item-icon>
                      <v-icon>mdi-eye</v-icon>
                    </v-list-item-icon>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title>{{mountPast}}</v-list-item-title>
                      <v-list-item-subtitle>Caldo de frango, queijo, cheiro-verde</v-list-item-subtitle>
                    </v-list-item-content>
                    <v-list-item-icon>
                      <v-icon>mdi-eye</v-icon>
                    </v-list-item-icon>
                  </v-list-item>
                </v-list>
              </div>
            </v-layout>
          </v-row>
        </v-container>
      </v-content>
    </v-app>
  </div>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    drawer: false,
    activeTab: 0,
    confirmar: false,
    mountPast: [
       {
       Tamanho: [],
       Molhos:[],
       Ingredientes:[],
       }
    ],





    products: [
      "Caldo", "Macarrão"
    ],
    datacliente: [

    ]
    ,
    components: [
      'Autocompletes', 'Comboboxes', 'Forms', 'Inputs', 'Overflow Buttons', 'Selects', 'Selection Controls', 'Sliders', 'Textareas', 'Text Fields',
    ],
    items: [
      { icon: 'mdi-contacts', text: 'Contacts' },
      { icon: 'mdi-history', text: 'Frequently contacted' },
      { icon: 'mdi-content-copy', text: 'Duplicates' },
      {
        icon: 'mdi-chevron-up',
        'icon-alt': 'mdi-chevron-down',
        text: 'Labels',
        model: true,
        children: [
          { icon: 'mdi-plus', text: 'Create label' },
        ],
      },
      {
        icon: 'mdi-chevron-up',
        'icon-alt': 'mdi-chevron-down',
        text: 'More',
        model: false,
        children: [
          { text: 'Import' },
          { text: 'Export' },
          { text: 'Print' },
          { text: 'Undo changes' },
          { text: 'Other contacts' },
        ],
      },
      { icon: 'mdi-settings', text: 'Settings' },
      { icon: 'mdi-message', text: 'Send feedback' },
      { icon: 'mdi-help-circle', text: 'Help' },
      { icon: 'mdi-cellphone-link', text: 'App downloads' },
      { icon: 'mdi-keyboard', text: 'Go to the old version' },
    ],

  }),
  methods: {
    confirm() {
      this.confirmar = true
      this.activeTab = 1
      console.log(this.datacliente);


    },
     confirmPast() {
     
      console.log(this.mountPast);
  

    }
  }
}
</script>
